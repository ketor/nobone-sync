# nobone-sync

A simple remote file sync tool for interal network.

It's also a simple example to show how [nobone](https://github.com/ysmood/nobone) works.

[![NPM version](https://badge.fury.io/js/nobone-sync.svg)](http://badge.fury.io/js/nobone-sync) [![Build Status](https://travis-ci.org/ysmood/nobone-sync.svg)](https://travis-ci.org/ysmood/nobone-sync) [![Deps Up to Date](https://david-dm.org/ysmood/nobone-sync.svg?style=flat)](https://david-dm.org/ysmood/nobone-sync)

## Install

Install globally:

    npm install -g nobone-sync

Install as lib dependency:

    npm install nobone-sync

## Usage

### As CLI Tool

Start a remote file server:

    nobone-sync -s

Start a local client to push change to remote server.

    nobone-sync config.coffee

The defaults of `config.coffee` is:

```coffee
module.exports =
    local_dir: 'local_dir'
    remote_dir: 'remote_dir'
    host: '127.0.0.1'
    port: 8345
    pattern: '**'
    polling_interval: 500
    on_change: (type, path, old_path) ->
        # It can also return a promise.
        console.log('Write your custom code here')
```

The `pattern` can be a string or an array, it takes advantage of `minimatch`.Some simple usages:

- To only watch `js`, `css` and `jpg`: `'**/*.@(js|css|jpg)'`
- To ignore `js` and `css` file: `['**', '!**/*.@(js|css)']`.

Use larger polling_interval if there are too many files to be watched.

#### Push a Path to Remote

Push a file or directory to remote server

    nobone-sync -u local_path host[:port]/path/to/dir

> local_path can be a file, a directory, or a glob pattern(glob pattern should use with quotation marks).

For example:

    nobone-sync -u 'src/css/a.css' 1.2.3.4:8222/home/me/src/css

    nobone-sync -u 'src/js/*.js' 1.2.3.4:8222/home/me/src/js


### As Library

```coffee
client = require 'nobone-sync/client'
server = require 'nobone-sync/server'

conf = {
    local_dir: 'local_dir'
    remote_dir: 'remote_dir'
    host: '127.0.0.1'
    port: 8345
    pattern: '**'
    polling_interval: 500
    on_change: (type, path, old_path) ->
        # It can also return a promise.
        console.log('Write your custom code here')
}

client conf
server conf
```

## FAQ

- Why not use Samba or sshfs?

  > If you search text content in a large project via Samba, it can be very slow.

- Why not svn, git?

  > Use scm is somehow slow, and deploy a general scm server on a development machine is not funny.

- Why not sftp?

  > It depends on ssh, for example, in Baidu, direct access to development machine via ssh is blocked.

- Why ftp?

  > At most times, the IDE plugins are not programmable. You cannot use it as a library and take advantage of file change event.
  >
  > This tool ignores what IDE your different team members are using, the config file can be source controled. They don't have to waste time to decide what plugins is best for the job.

- Auth and https?

  > For now it's only suit for a private network. I don't have any plan for security. I want to keep it simple and fast. If you need it, please open an issue.

