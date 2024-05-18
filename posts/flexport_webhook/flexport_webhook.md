---
title: Flexport Webhook with Cloud Functions
description: A post about creating a Flexport Webhook with Cloud Functions
date: 2021-08-15
tags:
  - another tag
layout: layouts/post.njk
---

As sometimes is the case I'm a bit late to the game when it comes to new
technologies. This has the effect that when I try out those new technologies
I get excited in the same way folks got excited for it months or years ago.

--excerpt--

One of those technologies is
<a href="https://en.wikipedia.org/wiki/Serverless_computing">Serverless Computing</a>.
I got the chance to use one implementation,
<a href="https://cloud.google.com/functions">Google Cloud Functions</a>
to create a webhook endpoint for the excellent Flexport
<a href="https://apidocs.flexport.com/reference/webhooks">API</a>.

<figure>
    <img src="port.jpg" alt="Hamburg Port Of Hamburg Container Ship Germany">
    <figcaption>Hamburg Port Of Hamburg Container Ship Germany</figcaption>
</figure>

I created an implementation that stores the milestone events Flexport sends
to Google Cloud Storage as well as a BigQuery table. The code uses
<a href="https://en.wikipedia.org/wiki/HMAC">HMAC</a> to authenticate the messages.


```python
import json

import hashlib
import hmac

from datetime import datetime
from google.cloud import storage
from google.cloud import bigquery

CS = storage.Client()
BQ = bigquery.Client()

BUCKET_NAME = "<BUCKET_NAME>"
TABLE_NAME = "<TABLE_NAME>"
SECRET = "<SECRET>"

def process_flexport_webhook(request):
    msg = request.get_data()
    key = SECRET.encode()

    digest = hmac.new(key, msg, hashlib.sha1).hexdigest()

    request_json = request.get_json()

    rid = request_json["id"]
    d = datetime.utcnow().replace(microsecond=0)
    file_name = f'{d.strftime("%Y-%m-%d--%H-%M-%S")}_{rid}'
    x_hub_signture = request.headers.get("X-Hub-Signature")

    # Verify request by computing SHA1 of payload and secret.
    if x_hub_signture ==  f"sha1={digest}":
        request_str = json.dumps(request_json)
        print("digest for  matches")
        blob = CS.get_bucket(BUCKET_NAME).blob(file_name)
        blob.upload_from_string(request_str)
        errors = BQ.insert_rows(BQ.get_table(TABLE_NAME), [[
            rid,
            datetime.utcnow(),
            request_str,
        ]])
        if errors != []:
            print(f"BQ error {errors}")
            return "Error storing data"
        return "Success"
    else:
        print(f"digest mismatch: {x_hub_signture} == sha1={digest}")
        return "Error validating data"
```

The table schema I use to store the messages is as follows:

```json
[
  {
    "mode": "NULLABLE",
    "name": "id",
    "type": "STRING"
  },
  {
    "mode": "NULLABLE",
    "name": "ts",
    "type": "DATETIME"
  },
  {
    "mode": "NULLABLE",
    "name": "payload",
    "type": "STRING"
  }
]
```

With BigQuery <a href="https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions#json_extract">JSON functions</a>
the relevant data can be easily extracted or a transforming view can be created.