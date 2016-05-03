# Deploying

## Setup

Collect static assets, run:

    curl https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.2/css/bootstrap.min.css > assets/static/css/bootstrap.min.css
    curl https://maxcdn.bootstrapcdn.com/font-awesome/4.6.1/css/font-awesome.min.css > assets/static/css/font-awesome.min.css
    curl https://maxcdn.bootstrapcdn.com/font-awesome/4.6.1/fonts/fontawesome-webfont.woff > assets/static/fonts/fontawesome-webfont.woff
    curl https://maxcdn.bootstrapcdn.com/font-awesome/4.6.1/fonts/fontawesome-webfont.ttf > assets/static/fonts/fontawesome-webfont.ttf
    curl https://maxcdn.bootstrapcdn.com/font-awesome/4.6.1/fonts/fontawesome-webfont.svg > assets/static/fonts/fontawesome-webfont.svg
    curl https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js > assets/static/js/jquery.min.js
    curl https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.2/js/bootstrap.min.js > assets/static/js/bootstrap.min.js
    curl https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.2/css/bootstrap.min.css.map > assets/static/css/bootstrap.min.css.map
    curl https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.map > assets/static/js/jquery.min.map

To prepare, run:

    gsutil mb gs://stage.kelproject.com
    gsutil defacl ch -u AllUsers:R gs://stage.kelproject.com/
    lektor build
    gsutil -m rsync -d -r $(lektor project-info --output-path) gs://stage.kelproject.com
    gsutil -m setmeta -h "Cache-Control:private,max-age=0,no-transform" gs://stage.kelproject.com/**
    gsutil web set -m index.html -e 404.html gs://stage.kelproject.com

## Deploy

To deploy, run:

    lektor build
    gsutil -m rsync -d -r $(lektor project-info --output-path) gs://stage.kelproject.com
    gsutil -m setmeta -h "Cache-Control:private,max-age=0,no-transform" gs://stage.kelproject.com/**

TODO: improve the `setmeta` command to cache some files.
