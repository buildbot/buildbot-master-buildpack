Buildpack: Buildbot-master
========================

This is a buildpack intended to deploy [Buildbot](http://buildbot.net)'s master application on
[Heroku](https://heroku.com) or VPS equiped with [Dokku](https://github.com/progrium/dokku).

This buildpack is modified from Heroku's [Python buildpack](https://github.com/heroku/heroku-buildpack-python) and
[NodeJS buildpack](https://github.com/heroku/heroku-buildpack-python). It will install the latest version of Buildbot
from its Github repo instead of the stable version on PyPI.

Usage
-----

Example usage:

The only thing you should provide is an empty git repo with a `.env` file.

    $ cat .env
    export BB_REPO='https://github.com/buildbot/buildbot'
    export BB_CHECKOUT='master'
    export BB_WWW_PLUGINS='base,waterfall_view'
    export BUILDPACK_URL='https://github.com/buildbot/buildbot-master-buildpack.git'

You can assign different repo, branch or frondend app to install by changing `BB_REPO`,
`BB_CHECKOUT` and `BB_WWW_PLUGINS`, which are optional parameters. `BB_CHECKOUT` can be branch name or tag name
and `BB_WWW_PLUGINS` can be one or several plugins joined by a `,` with no extra spaces.

The `BUILDPACK_URL` variable is required for deploying with Dokku, it should point to this build pack.

For deploying with heroku:

    $ heroku create --buildpack git://github.com/shanzi/buildbot-master-buildpack.git
    $ git push heroku master
    ...
    -----> Building buildbot from buildstep...
    -----> Fetching custom buildpack
    -----> app detected
    -----> Installing NodeJS and NPM
    -----> Resolving node version (latest stable) via semver.io...
    -----> Downloading and installing node 0.12.2...
    -----> Using default npm version: 2.7.4
    -----> Installing buildbot dependencies
    -----> Procfile declares types -> web

Deploying with Dokku is the same easy:
    
    $ git remote add dokku dokku@<your server>:buildbot
    $ git push dokku master

You can override the default `master.cfg` by putting one in the root folder of repo.

    $ ls
    master.cfg

Specify a Runtime
-----------------

You can also provide other releases of Python with a `runtime.txt` file.

    $ cat runtime.txt
    python-2.6.9

As buildbot's master only supports to run on Python 2.6.x to 2.7.x, 
so runtime for Python 3 is not supported by this buildpack.
