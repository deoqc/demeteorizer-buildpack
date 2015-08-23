# Demeteorizer Buildpack

A fork of the popular [Meteor Buildpack Horse](https://github.com/AdmitHub/meteor-buildpack-horse) that uses [Demeteorizer](https://github.com/onmodulus/demeteorizer) to allow installing a TypeScript app.

To use this with your meteor app and heroku:

1. Set up your app to [deploy to heroku with git](https://devcenter.heroku.com/articles/git).
2. Set this repository as the buildpack URL:

        heroku buildpacks:set https://github.com/dominikmayer/demeteorizer-buildpack.git

3. If it isn't set already, be sure to set the ``ROOT_URL`` for meteor (replace URL with whatever is appropriate):

        heroku config:set ROOT_URL=https://<yourapp>.herokuapp.com

4. If you don't yet have a MongoDB, add the MongoLab addon:

        heroku addons:create mongolab

   Otherwiese, make sure to set the `MONGO_URL`:

        heroku config:set MONGO_URL=mongodb://<your MongoDB>

Once that's done, you can deploy your app using this build pack any time by pushing to heroku:

    git push heroku master

## Extras

The basic buildpack should function correctly for any normal-ish meteor app,
with or without npm-container.  For extra steps needed for your particular build,
just add shell scripts to the "extras" folder and they will get sourced into the
build.

Extras included in this branch:
 - ``mongolab.sh``: Set ``MONGO_URL`` to the value of ``MONGOLAB_URI``.
 - ``phantomjs.sh``: Include phantomjs for use with ``spiderable``.

## Where things go

This buildpack creates a directory ``.meteor/heroku_build`` (``$COMPILE_DIR``)
inside the app checkout, and puts all the binaries and the built app in there.
So it ends up having the usual unixy ``bin/``, ``lib/``, ``share`` etc
subdirectories.  Those directories are added to ``$PATH`` and
``$LD_LIBRARY_PATH`` appropriately.

So ``$COMPILE_DIR/bin`` etc are great places to put any extra binaries or stuff
if you need to in custom extras.

## Workarounds

Meteor is under active developement, recent changes in its core broke support for
certain meteor packages designed to access their own assets at first run. The issue
has been reported on https://github.com/meteor/meteor/issues/2606, but it may take
a while to have it fixed. In the meanwhile you can circumvent the problem by setting
the following variable in your Heroku Config Vars:

    BUILDPACK_PRELAUNCH_METEOR
