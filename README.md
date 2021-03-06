# Node-Webkit Sample with Native DLL

This is a sample app for [node-webkit](https://github.com/rogerwang/node-webkit) that calls a Native DLL.

node-webkit is a excellent framework for writing a cross platform GUI app. But there are some cases when you want to call a Native DLL.

You can use [node-ffi](https://github.com/rbranson/node-ffi) to call Native DLLs from JavaScript. But the JS engine in node-webkit is V8 based and requires slightly different setup.

This is a minimum app showing how to call Native DLLs from JavaScript code in node-webkit.

* Based on skelton code generated by generator-node-webkit.
* Single set of source codes runs on Win/Mac and [calls Native  DLLs](https://github.com/essa/nw_native_dll_sample/blob/master/app/js/index.js) ( *.dylib, *.DLL )
* Different sources for DLLs on each platform, with same [API](https://github.com/essa/nw_native_dll_sample/blob/master/dlls/macosx/sample1.h).

# 32bit issue

I'm not clear about this, but node-webkit ( at least some version ) requires DLLs to be 32bit version.

So, in this document, I recomend 32bit version of node.js.

# usage

## on Mac

### test DLL with node.js cli

Install node.js [32bit version](http://nodejs.org/dist/v0.10.26/node-v0.10.26-darwin-x86.tar.gz) from [Download page of nodejs.org ](http://nodejs.org/download/).

Set NODE_PATH environmente variable.

    $ export NODE_PATH=/usr/local/lib/node_modules

Install [node-ffi](https://github.com/rbranson/node-ffi) and [ref](https://github.com/TooTallNate/ref)

    $ npm install -g ffi
    $ npm install -g ref

Compile sample1.dll and test it with node.js cli.

    $ git clone https://github.com/essa/nw_native_dll_sample.git
    $ cd nw_native_dll_sample
    $ cd dlls/macosx
    $ make
    node ../test_sample.js
    # If you see this message, the sample is working.
    <Buffer@0x22591c0 05 00 00 00 01 00 00 00 6d 61 63 00 00 00 00 00 00 00 00 00 00 00 00 00>
    { sum: 5, difference: 1, platform: 'mac' }

You can see the sources at [nw_native_dll_sample/dlls](https://github.com/essa/nw_native_dll_sample/tree/master/dlls)

### Run the sample app with node-webkit

Install node-webkit and grunt-cli

    $ npm install -g nodewebkit grunt-cli nw-gyp

Install ffi

    $ grunt shell:install_ref shell:install_ffi

This task will compile a native module for node-webkit by commands like this.

    $ rm -rf ffi
    $ npm install ffi
    $ cd node_modules/ffi
    $ nw-gyp rebuild --target=0.8.5'

Modules made by nw-gyp is not compatible with node.js. If you want to run the sample with node.js cli, you must remove node_modules/ffi and node_modules/ref.

Run the app
    $ nodewebkit app

You will see this.

![Screen Shot on Mac](https://raw.github.com/essa/nw_native_dll_sample/master/screenshots/nw_dll_mac.png)

The source is [app/js/index.js](https://github.com/essa/nw_native_dll_sample/blob/master/app/js/index.js)

### packaging

    $ grunt dist-mac

You can copy dist/nw-dll-sample.app/ to other machine and run it.

## on Windows

### Install dependencies

You need to install these products to run this app.

* Visual Studio 2013 express
* MinGW (Base, Msys and C++ Compiler)
* Python 2.7.X
* node.js [32bit version](http://nodejs.org/dist/v0.10.26/node-v0.10.26-x86.msi) from [Download page of nodejs.org ](http://nodejs.org/download/).

### test DLL with node.js cli

    $ git clone https://github.com/essa/nw_native_dll_sample.git
    $ cd nw_native_dll_sample
    $ npm install ffi ref
    $ cd nw_dll_sample/windows
    $ make  
    <Buffer@0x2a2fb98 05 00 00 00 01 00 00 00 77 69 6e 64 6f 77 73 00 00 00 00 00 00 00 00 00>
    { sum: 5, difference: 1, platform: 'windows' }

### Run the sample app with node-webkit

    $ npm install -g nodewebkit grunt-cli nw-gyp
    $ grunt shell:install_ref shell:install_ffi
    $ nodewebkit app

You will see this.

![Screen Shot on Windows](https://raw.github.com/essa/nw_native_dll_sample/master/screenshots/nw_dll_win.png)

The source is [app/js/index.js](https://github.com/essa/nw_native_dll_sample/blob/master/app/js/index.js)

## on Linux

(under construction)

## create a skelton app source by yourself.

Install node-webkit and yeoman generator

    $ npm install -g nodewebkit yo generator-node-webkit
    $ yo node-webkit

Then copy Gruntfile.js, app/js/index.js, dlls/* from this repository and modify them.



