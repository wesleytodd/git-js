# Simple Git

A light weight interface for running git commands in any [node.js](http://nodejs.org) application.

# Installation

Easiest through [npm](http://npmjs.org): `npm install simple-git`

# Dependencies

Relies on [git](http://git-scm.com/downloads) already having been installed on the system, and that it can be called
using the command `git`.

# Usage

Include into your app using:

    var simpleGit = require('simple-git')( workingDirPath );

where the `workingDirPath` is optional, defaulting to the current directory.

Use `simpleGit` by chaining any of its functions together. Each function accepts an optional final argument which will
be called when that step has been completed. When it is called it has two arguments - firstly an error object (or null
when no error occurred) and secondly the data generated by that call.

`.pull(handlerFn)` pull all updates from the repo

`.tags(handlerFn)` list all tags

`.checkout(checkoutWhat, handlerFn)` checks out the supplied tag, revision or branch

`.checkoutLatestTag(handlerFn)` convenience method to pull then checkout the latest tag

`.add([fileA, ...], handlerFn)` adds one or more files to be under source control

`.commit(message, handlerFn)` commits changes in the current working directory with the supplied message

`.commit(message, [fileA, ...], handlerFn)` commits changes on the named files with the supplied message

# Examples

    // update repo and get a list of tags
    require('simple-git')(__dirname + '/some-repo')
         .pull()
         .tags(function(err, tags) {
            console.log("Latest available tag: %s", tags.latest);
         });


    // update repo and when there are changes, restart the app
    require('simple-git')()
         .pull(function(err, update) {
            if(update && update.summary.changes) {
               require('child_process').exec('npm restart');
            }
         });
