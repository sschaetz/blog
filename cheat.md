# Cheat Sheet

1. generate and develop the blog
   npx eleventy --watch
   npx eleventy --serve

2. publish the blog
   gsutil -m rsync -r ./_site/ gs://www.soundbarrier.io/
