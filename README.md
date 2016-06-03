![MIT License](https://img.shields.io/github/license/mkenney/docker-npm.svg) ![Docker pulls](https://img.shields.io/docker/pulls/mkenney/npm.svg) ![Docker stars](https://img.shields.io/docker/stars/mkenney/npm.svg) ![Github issues](https://img.shields.io/github/issues-raw/mkenney/docker-npm.svg)

![Node.js](https://img.shields.io/badge/Node.js-v6.2.0-026e00.svg) ![npm](https://img.shields.io/badge/npm-v3.8.9-c12127.svg) ![gulp](https://img.shields.io/badge/gulp-v1.2.1-cf4646.svg) ![grunt](https://img.shields.io/badge/Grunt-v1.2.0-e48632.svg)

# Portable npm and related executables

## Synopsys

Essentially, this is just a set of [shell scripts](https://github.com/mkenney/docker-npm/tree/master/bin) that manages a `node` docker image. The combination of the shell script and docker image allows commands to run as either the current user or the owner/group of the current directory.

### Installation

Installation is just a matter of putting the [shell scripts](https://github.com/mkenney/docker-npm/tree/master/bin) somewhere in your path. For example, assuming you want the scripts to live in `/usr/local/bin`, run:
* `sudo wget -nv -O /usr/local/bin/npm https://raw.githubusercontent.com/mkenney/docker-npm/master/bin/npm`
* `sudo wget -nv -O /usr/local/bin/gulp https://raw.githubusercontent.com/mkenney/docker-npm/master/bin/gulp`
* `sudo wget -nv -O /usr/local/bin/grunt https://raw.githubusercontent.com/mkenney/docker-npm/master/bin/grunt`
* Make them executable and add write permission to let the scripts `self-update` as any user
  * `sudo chmod 0777 /usr/local/bin/npm`
  * `sudo chmod 0777 /usr/local/bin/gulp`
  * `sudo chmod 0777 /usr/local/bin/grunt`
* `npm self-update` (all of the scripts have a `self-update` command which pulls down the latest docker image and then updates the shell script itself)

## About

This [image](https://hub.docker.com/r/mkenney/npm/) contains the latest `node` and `npm` binaries for [Debian:jessie](https://hub.docker.com/_/debian/). `npm` has been used to install `gulp-cli` and `grunt-cli` globally. When using the [shell scripts](https://github.com/mkenney/docker-npm/tree/master/bin) available in the [source repository](https://github.com/mkenney/docker-npm), the current directory is mounted into `/src` in the container and a wrapper script executes `npm` as a user who's `uid` and `gid` matches those properties on that directory. This way any output is written as the directory owner/group instead of root or a random user.

If you need additional modules and/or wrapper scripts (`bower`, etc.) [let me know](https://github.com/mkenney/docker-npm/issues).

## Change log

### 2016-05-25

#### Breaking changes

I have added a wrapper script to the container that executes `npm`, `gulp` and `grunt` commands as a user who's `uid` and `gid` matches those properties on the current directory. This way any files are installed as the directory owner/group instead of root or a random user.

If you've been using the [previous version](https://github.com/mkenney/docker-npm/tree/deprecated/bin) of the included shell scripts from the project's `/bin` directory you will probably need to update the permissions of files created using them or the new scripts are likely to have permissions errors because previously the files would have been created by the `root` user. This command should take care of it for you but _make sure you understand what it will do **before** you run it_. I can't help you if you hose your system.

* From your **_project directory_**: `sudo chown -R $(stat -c '%u' .):$(stat -c '%g' .) ./`

If you haven't been using the included scripts, then you don't need to do anything.

If you need to use the [previous version](https://github.com/mkenney/docker-npm/tree/deprecated) of this image it's [tagged](https://hub.docker.com/r/mkenney/npm/tags/) as `deprecated` but the image is still rebuilt automatically when there are changes to [node:latest](https://hub.docker.com/_/node/) or [Debian:jessie](https://hub.docker.com/_/debian/) so it will stay up to date.

## Source Repo

[mkenney/docker-npm](https://github.com/mkenney/docker-npm)

## Docker image

[mkenney/npm](https://hub.docker.com/r/mkenney/npm/) Based on [node:latest](https://hub.docker.com/_/node/) (debian:jessie)

## Commands

The following wrapper scripts are available in the source repository

* [npm](https://github.com/mkenney/docker-npm/blob/master/bin/npm)
* [gulp](https://github.com/mkenney/docker-npm/blob/master/bin/gulp)
* [grunt](https://github.com/mkenney/docker-npm/blob/master/bin/grunt)

## Tagged Dockerfiles

* [latest](https://github.com/mkenney/docker-npm/blob/master/Dockerfile)
* [deprecated](https://github.com/mkenney/docker-npm/blob/deprecated/Dockerfile)
