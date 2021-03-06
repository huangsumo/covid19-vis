# [covidvis](https://covidvis.berkeley.edu)

[![Click to visit website](assets/img/screenshot.png)](https://covidvis.berkeley.edu)

Build Process Summary
---------------------

TL;DR: There’s 3 steps. The first step (`scripts/build-charts.py`) builds
charts and spits out `website/_config.yml` to include paths to the javascript
for the charts. The second step is the jekyll build step
(`scripts/build-web.sh`, or `cd website && bundle exec jekyll build`), which
takes `website/_config.yml` and generates the website from all the liquid
templates and markdown in the `website/` directory (compiled website is output
to `website/_site`). The 3rd step (`scripts/deploy-web.sh`) copies
`website/_site` into `../covidvis.github.io`, overwriting everything that was
there before and pushing. Steps 1 & 2: `make`. Step 3: `make deploy` (from root
directory of this repo).

Building the Charts
-------------------
Note that Python 3 is required.
```
pip install -r requirements.txt
./scripts/build-charts.py
```

Now the charts have been added to the website.
(Try `ls website/js/autogen`).

Building the Website
--------------------

Make sure that you have Ruby version >= 2.4. Building the website requires ruby
and bundler. To grab website dependencies, do the following from the `website`
directory:

```
gem install bundler
bundle install
```

Now it should be possible to preview changes using jekyll (from `website`
subdirectory):

`bundle exec jekyll serve`

To build without previewing execute the following (again, from `website`
subdirectory):

`bundle exec jekyll build`

Or simply run the convenience script (from the root directory):

`./scripts/build-web.sh`

Deploying
---------

Deploying to github pages consists of copying the contents of `website/_site`
into `covidvis.github.io`, commiting, and pushing. The command for this is
as follows:

`./scripts/deploy-web.sh`

`make depoy` also works.

* NOTE: this script assumes that the repo `../covidvis.github.io` exists in the
  parent directory.

Staging
---------

Deploying to the staging repo consists of copying the contents of
`website/_site` into the `gh-pages` of the `covidvis-staging` repository,
followed by commiting and pushing. The command for this is as follows:

`./scripts/deploy-web.sh ../covidvis-staging gh-pages`

`make stage` also works.

* NOTE: this script assumes that the repo `../covidvis-staging` exists in the
  parent directory. It may be necessary to first clone and check out the
  `gh-pages` branch. The following commands should be run from the parent
  directory:

```
git clone git@github.com:covidvis/covidvis-staging
cd covidvis-staging
git checkout gh-pages
```


Makefile for End to End Building and Deploying
----------------------------------------------

To execute all build steps end-to-end, simply type `make`.

To stage: `make stage` (just a wrapper around `scripts/deploy-web.sh`)
To deploy: `make deploy` (another wrapper around `scripts/deploy-web.sh`)

* NOTE: the deploy script only pushes the built website. To save your edits,
  you still need to commit / push changes made in the covid19-vis repo.


Editing the Website
-------------------
`website/index.markdown` contains the actual content.
`website/assets/css/main.scss` contains styling.
Edit `website/_includes/head.html` for anything that needs to go between `<head>` and `</head>` tags.
