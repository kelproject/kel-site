# Deploying

## Setup

To prepare, run:

    gsutil mb gs://stage.kelproject.com
    gsutil defacl ch -u AllUsers:R gs://stage.kelproject.com/
    gsutil -m rsync -d -r $(lektor project-info --output-path) gs://stage.kelproject.com
    gsutil -m setmeta -h "Cache-Control:private,max-age=0,no-transform" gs://stage.kelproject.com/**
    gsutil web set -m index.html -e 404.html gs://stage.kelproject.com

## Deploy

To deploy, run:

    gsutil -m rsync -d -r $(lektor project-info --output-path) gs://stage.kelproject.com
    gsutil -m setmeta -h "Cache-Control:private,max-age=0,no-transform" gs://stage.kelproject.com/**

TODO: improve the `setmeta` command to cache some files.
