---
title: Airflow Time-spans
description: A post about Airflow and its times-span intricacies
date: 2021-04-25
tags:
  - another tag
layout: layouts/post.njk
---

A common gotcha when working with airflow is to determine when a DAG is
going to execute, and what the execution date-time is going to be when it
executes. Especially if a DAG relies on processing data for a given
execution date-time only to ensure idempotency. The best way to think about
this is not in dates or timestamps but time-spans. An Airflow DAG run
is going to cover a times-span - i.e. it is going to process data that
was generated/received/added etc. in a given time-span. --excerpt--
The start and end of a time-span can be accessed within operators with:

```python
timespan_start = context["execution_date"]
timespan_end = context["dag"].following_schedule(timespan_start)
```
Looking at this it becomes clear that <code class="language-python">context["execution_date"]</code>
is poorly named. Rather than the date and time when the DAG runs, it describes the
start of the times-pan. Indeed, the DAG executes at
<code class="language-python">context["execution_date"]</code> of the _following_ DAG instance.


<figure>
    <img src="Reloj_Burk_germany.jpg" alt="Test Equipment">
    <figcaption>Bürk brand watch, of German origin, used for controlling entry and exit of personnel,
image by <a href="https://commons.wikimedia.org/wiki/File:Reloj_Burk_germany.jpg">Fedaro</a></figcaption>
</figure>

Here is an example where this counter-intuitive behavior can cause trouble: Say one expects
a set of files to be made available by 1am every morning. A reasonable approach will be to
schedule a DAG for 2am every morning. To only process the newest files one could decide
to filter by <code class="language-python">context["execution_date"]</code>.
Another reason to do this could be to to ensure idempotency - which means that re-running
a DAG instance at a later date will generate the exact same output.

The problem with this approach is that it introduces an unintended 24 hour delay. New files
arrive by 1am, the DAG starts to run at 2am but the DAGs execution-date is set to 1am of the
previous day - so it will process data from the day before - 24 hour old data.

The solution is to not filter data using the incorrectly named
<code class="language-python">context["execution_date"]</code>
but the actual execution date-time which is
<code class="language-python">context["dag"].following_schedule(context["execution_date"])</code>.

Thinking about time-spans instead of execution date/times is the way to go in Airflow in my
opinion. Airflow is a batch system that often will process data that was added during a given
time-span. This is different from an event or streaming system. To simplify such operation I usually
added a <a href="https://github.com/apache/airflow/blob/7a0d4122459289e0f2db78ad2849d5ba42df4468/airflow/providers/google/cloud/operators/gcs.py#L663">GCSTimeSpanFileTransformOperator</a>
which allows fetching data generated within the DAG time-span from Google Cloud Storage,
downloading the files, transforming them and re-uploading them to GCS.
