---
title: Python Packaging
description: Explanation on how to create Python wheels
date: 2023-11-29
tags:
  - another tag
layout: layouts/post.njk
---

Python packaging is a marvelous world of complexity, technical depth and powerful functionality, that works really well in most cases. If it does not, things get hairy very fast.

--excerpt--

512  pyenv install -v 3.11.4
  513  pyenv virtualenv 3.11.4 aws-lambda-build
  514  pyenv activate aws-lambda-build
  515  git clone https://github.com/aws/aws-lambda-python-runtime-interface-client.git
  516  git checkout b45577eeda88c90485811b05a8c7b66e5ed39fe1
  517  cd aws-lambda-python-runtime-interface-client/
  518  git checkout b45577eeda88c90485811b05a8c7b66e5ed39fe1
  519  make init
  520  make build
  521  pip install wheel
  522  python setup.py bdist_wheel
  523  sudo apt-get install autoconf
  524  python setup.py bdist_wheel
  525  sudo apt install libtool
  526  python setup.py bdist_wheel
  527  ls
  528  cd dist/
  529  ls -alh
  530  pwd
  531  ifconfig
  532  ls
  533  cd ..
  534  git clone https://github.com/simplejson/simplejson
  535  cd simplejson/
  536  git checkout c516685
  537  python setup.py bdist_wheel
  538  ls
  539  ls dist/
  540  ls -alh dist/
  541  history
  542  pwd
  543  cd dist/
  544  ls
  545  cd ..
  546  ls
  547  cd ..
  548  ls
  549  cd soundbarrier.io/
  550  ls
  551  cd blogv3-js/
  552  ls
  553  cd _draft/
  554  ls
  555  vim boeing_book.md
  556  cp boeing_book.md python_packaging.md
  557  vim python_packaging.md
  558  history
