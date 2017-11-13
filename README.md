PhantomJS 2.5 Beta
=========

An NPM wrapper for [PhantomJS](http://phantomjs.org/) *2.5 Beta* headless webkit with JS API.

NOTE: phantomjs v2.x is currently under heavy development. Releases should be considered unstable.

[![npm](https://img.shields.io/npm/v/npm.svg)]() [![npm](https://img.shields.io/npm/dm/phantomjs25-beta.svg)](https://www.npmjs.com/package/phantomjs25-beta) [![Travis](https://img.shields.io/travis/takuhii/PhantomJS-25_Beta.svg)](https://travis-ci.org/takuhii/PhantomJS-25_Beta/)

Building and Installing
-----------------------

```shell
npm install phantomjs25-beta
```

The package has been set up to fetch and run Phantom for MacOS (darwin), Ubuntu Linux (Xenial) & Windows.

Running
-------

```shell
bin/phantomjs [phantom arguments]
```

And npm will install a link to the binary in `node_modules/.bin`.

Running via node
----------------

The package exports a `path` string that contains the path to the
phantomjs binary/executable.

Below is an example of using this package via node.

```javascript
var path = require('path')
var childProcess = require('child_process')
var phantomjs = require('phantomjs')
var binPath = phantomjs.path

var childArgs = [
  path.join(__dirname, 'phantomjs-script.js'),
  'some other argument (passed to phantomjs script)'
]

childProcess.execFile(binPath, childArgs, function(err, stdout, stderr) {
  // handle results
})

```

Versioning
----------

The major and minor number tracks the version of PhantomJS that will be
installed. The patch number is incremented when there is either an installer
update or a patch build of the phantom binary.

Deciding Where To Get PhantomJS
-------------------------------

I have changed the package to download phantomjs from `https://bitbucket.org/takuhii/phantomjs25-beta/downloads/` now, as I didn't want to be spanking Ariya's bandwidth. This should work fine for most people.

## Downloading from a custom URL

If bitbucket is down, or the Great Firewall is blocking bitbucket, you have another option:

```shell
npm install phantomjs --phantomjs_downloadurl=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-windows.zip
npm install phantomjs --phantomjs_downloadurl=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-macos.zip
npm install phantomjs --phantomjs_downloadurl=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-linux-ubuntu-xenial-x86_64.tar.gz
npm install phantomjs --phantomjs_downloadurl=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-linux-ubuntu-trusty-x86_64.tar.gz
```

or set the environment variable `PHANTOMJS_DOWNLOADURL`.
```shell
PHANTOMJS_DOWNLOADURL=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-windows.zip npm install PhantomJS25-Beta
PHANTOMJS_DOWNLOADURL=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-macos.zip npm install PhantomJS25-Beta
PHANTOMJS_DOWNLOADURL=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-linux-ubuntu-xenial-x86_64.tar.gz npm install PhantomJS25-Beta
PHANTOMJS_DOWNLOADURL=https://bitbucket.org/takuhii/phantomjs25-beta/downloads/phantomjs-2.5.0-beta-linux-ubuntu-trusty-x86_64.tar.gz npm install PhantomJS25-Beta
```


##### Using PhantomJS from disk

If you plan to install phantomjs many times on a single machine, you can
install the `phantomjs` binary on PATH. The installer will automatically detect
and use that for non-global installs.


A Note on PhantomJS
-------------------

PhantomJS is not a library for NodeJS.  It's a separate environment and code
written for node is unlikely to be compatible.  In particular PhantomJS does
not expose a Common JS package loader.

This is an _NPM wrapper_ and can be used to conveniently make Phantom available
It is not a Node JS wrapper.

I have had reasonable experiences writing standalone Phantom scripts which I
then drive from within a node program by spawning phantom in a child process.

Read the PhantomJS FAQ for more details: http://phantomjs.org/faq.html

### Linux Note

An extra note on Linux usage, from the PhantomJS download page:

 > This package is built on CentOS 5.8. It should run successfully on Lucid or
 > more modern systems (including other distributions). There is no requirement
 > to install Qt, WebKit, or any other libraries. It is however expected that
 > some base libraries necessary for rendering (FreeType, Fontconfig) and the
 > basic font files are available in the system.

Troubleshooting
---------------

##### Installation fails with `spawn ENOENT`

This is NPM's way of telling you that it was not able to start a process. It usually means:

- `node` is not on your PATH, or otherwise not correctly installed.
- `tar` is not on your PATH. This package expects `tar` on your PATH on Linux-based platforms.

Check your specific error message for more information.

##### Installation fails with `Error: EPERM` or `operation not permitted` or `permission denied`

This error means that NPM was not able to install phantomjs to the file system. There are three
major reasons why this could happen:

- You don't have write access to the installation directory.
- The permissions in the NPM cache got messed up, and you need to run `npm cache clean` to fix them.
- You have over-zealous anti-virus software installed, and it's blocking file system writes.

##### Installation fails with `Error: read ECONNRESET` or `Error: connect ETIMEDOUT`

This error means that something went wrong with your internet connection, and the installer
was not able to download the PhantomJS binary for your platform. Please try again.

##### I tried again, but I get `ECONNRESET` or `ETIMEDOUT` consistently.

Do you live in China, or a country with an authoritarian government? We've seen problems where
the GFW or local ISP blocks bitbucket, preventing the installer from downloading the binary.

Try visiting the [the download page](https://bitbucket.org/takuhii/phantomjs25-beta/downloads/) manually.
If that page is blocked, you can try using a different CDN with the `PHANTOMJS_CDNURL`
env variable described above.

##### I am behind a corporate proxy that uses self-signed SSL certificates to intercept encrypted traffic.

You can tell NPM and the PhantomJS installer to skip validation of ssl keys with NPM's
[strict-ssl](https://www.npmjs.org/doc/misc/npm-config.html#strict-ssl) setting:

```
npm set strict-ssl false
```

WARNING: Turning off `strict-ssl` leaves you vulnerable to attackers reading
your encrypted traffic, so run this at your own risk!

##### I tried everything, but my network is b0rked. What do I do?

If you install PhantomJS manually, and put it on PATH, the installer will try to
use the manually-installed binaries.

##### I'm on Debian or Ubuntu, and the installer failed because it couldn't find `node`

Some Linux distros tried to rename `node` to `nodejs` due to a package
conflict. This is a non-portable change, and we do not try to support this. The
[official documentation](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint-elementary-os)
recommends that you run `apt-get install nodejs-legacy` to symlink `node` to `nodejs`
on those platforms, or many NodeJS programs won't work properly.

Contributing
------------

Questions, comments, bug reports, and pull requests are all welcome.  Submit them at
[the project on GitHub](https://github.com/takuhii/PhantomJS-25_Beta/issues).

PhantomJS 2.5 Beta archives sourced from [Ariya's bitbucket repo](https://bitbucket.org/ariya/phantomjs/downloads/).

Install.js HEAVILY lifted from [Medium](https://github.com/Medium/phantomjs/)

Bug reports that include steps-to-reproduce (including code) are the
best. Even better, make them in the form of pull requests.

Author 
------

[Darren Mackintosh](https://github.com/takuhii).

License
-------

Copyright 2017 [Darren Mackintosh](https://github.com/takuhii).

Licensed under the MIT License.
See the top-level file `LICENSE.txt`.
