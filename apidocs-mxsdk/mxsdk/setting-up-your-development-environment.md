---
title: "Setting Up Your Development Environment"
space: "API & SDK"
parent: "your-learning-path-for-the-mendix-sdk"
---
This tutorial will lead you through the process of setting up everything you need to start working with the Mendix Platform SDK. This includes setting up development tools and creating a first SDK script that automatically bootstraps a new Mendix app.

### Contents of this page

## Quick Installation

If you know what you are doing, the quick installation instructions below are for you. Otherwise, please skip this paragraph and continue with [Setting up your development tools](setting-up-your-development-environment).

**Quick installation instructions**

Set up a new `node` project and install the dependencies:

```bash
$ mkdir my-app-generator
$ cd my-app-generator
$ npm init --yes
$ npm install -g typescript tsd
$ npm install mendixmodelsdk mendixplatformsdk when --save
$ tsd install when --save
$ curl -o tsconfig.json -sL http://tinyurl.com/mxsdk-tsconfig
```

We use [curl](http://curl.haxx.se/download.html#Win32) to download a `tsconfig.json` file (see below) that we've already set up for you.

## Setting up your development tools

1.  Install [Node.js](https://nodejs.org/). Make sure that version 4.2._x_ is installed. If you need to download it, you can find it on [this page](https://nodejs.org/en/download/releases/).

2.  Open a terminal, on Windows e.g. [Command Prompt](http://windows.microsoft.com/en-us/windows/command-prompt-faq), and run the following command:

    ```text
    $ node --version
    v4.2.2
    ```

    For Debian-based Linux distributions such as Ubuntu, please refer to [this article](https://github.com/nodesource/distributions#user-content-installation-instructions) to properly set up your apt-get sources.

    In the rest of the tutorial, in blocks such as the above, lines starting with a `$` represent commands to type into a terminal. Sometimes a line follows without a $, represents output of the command.

3.  Install [Visual Studio Code](https://code.visualstudio.com/) - not to be confused with Visual Studio - a text editor/IDE with good support for [TypeScript](http://www.typescriptlang.org/). Make sure you have a recent version (v0.7.0+); check the version you are using through Help > About when you have Code opened.
4.  Install TypeScript 1.6.2 with [`npm`](https://www.npmjs.com/) , Node.js' package manager:

    ```text
    $ npm install -g typescript@1.6.2
    ```

5.  Use the following command to check the TypeScript compiler version on your PATH:

    ```text
    $ tsc --version
    message TS6029: Version 1.6.2
    ```

    If the version number is much lower, it could be that you also have an outdated TypeScript SDK on your system, left over from a previous installation. You can either uninstall the old TypeScript SDK, or bypass it by removing the old TypeScript SDK from your system's PATH environment variable.

6.  Use the following command to install the [TypeScript Definition Manager for DefinitelyTyped](http://definitelytyped.org/tsd/). From the tsd homepage: "TSD is a package manager to search and install TypeScript definition files directly from the community driven [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped) repository".

    ```text
    $ npm install -g tsd
    ```

7.  Use the following command to verify that a recent version is properly installed:

    ```text
    $ tsd --version
    >> tsd 0.6.5
    ```

    A lot of libraries have been written in JavaScript, and require [custom typings](http://www.typescriptlang.org/Handbook#writing-dts-files) so that the TypeScript compiler can check for type correctness of the code that uses those libraries. Fortunately, the TypeScript community has already created those custom typings for many open source JavaScript libraries. See the [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped) repository for typings for (at the time of writing) more than 1000 libraries.

    Sometimes, invoking `tsd` will fail because the GitHub API usage limit has been reached. To fix this, create a `.tsdrc` file in your user (home) directory or in your project directory that contains a `{ "token": "your_github_token" }` GitHub OAuth token.

    Please refer to the [tsd documentation](https://github.com/Definitelytyped/tsd#tsdrc) for further details.

## Setting up a working directory for your script

1.  First, create a new directory and initialize it for use with the Node.js package manager `npm`. Using `--yes` skips several unimportant questions. This creates a  [`package.json`](https://docs.npmjs.com/files/package.json)with default contents. Through this file you control your `npm` package. 

    ```java
    $ mkdir my-app-generator
    $ cd my-app-generator
    $ npm init --yes
    ```

    Visual Studio Code, other than Visual Studio, works with directories instead of project files.

2.  Start **Visual Studio Code** and open the directory you just created. You can open a new instance of Code from the command line with the directory you want to open as first argument. For example, if your current working directory in your terminal is the directory in which all your project files live, use the following command to open Code:

    ```java
    $ code .
    ```

3.  Add `mendixmodelsdk,` `mendixplatformsdk,` and `when.js` as dependencies. 
    Dependencies are stored in the `node_modules` directory (which will be automatically created by `npm` if necessary). Open the `package.json` you just created. Add a [`dependencies` block](https://docs.npmjs.com/files/package.json#dependencies) that looks like this:

    ```text
    "dependencies": {
      "mendixmodelsdk": "~2.0.0",
      "mendixplatformsdk": "~2.0.0",
      "when": "^3.7.3"
    }
    ```

    When a new major or minor version of the Mendix SDK is released (i.e. 1.0.0 to 2.0.0 or 1.0.0 to 1.1.0) and you run `npm update` in your project folder, the `~` in front of the version number makes sure that installed version of the SDK won't be upgraded automatically. Only patch releases (i.e. 1.0.1) of the SDK will be automatically upgraded, otherwise your script could inadvertently be broken. You may, of course, edit the dependency by hand yourself.

4.  Save your changes and then execute the following to install the dependencies:

    ```java
    $ npm install
    ```
    If you are using version control, make sure to ignore the `node_modules directory`, otherwise you end up committing dependencies.

5.  In Code, create a `[tsconfig.json](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json)` file next to your `package.json`. The `tsconfig.json` file is used by the TypeScript compiler to compile your code in the proper manner to a JS file. Create it with the following contents. 

    ```text
    {
    	"compilerOptions" : {
    		"module" : "commonjs",
    		"target" : "es5"
    	},
    	"files" : [
    		"script.ts"
    	]
    }
    ```

    The compiler options should be left as-is. The `files` directive is an array of strings with path names of all TypeScript files that make up your Node.js script or app. You can change it so that the compiler uses the right files.

    Create new files in your project directory with Visual Studio Code by hovering with the mouse cursor over the name of the working directory in the left side pane. A "new file" icon appears. Click it to create a new file. For more information on basic editing with Visual Studio Code, check the [manual](https://code.visualstudio.com/Docs/editor/codebasics).

6.  Install the typings for the when.js library.

    ```java
    $ tsd install when --save
    ```

    This creates a `typings` directory in your project directory, which will contain subdirectories with imported typings, and a `tsd.d.ts` which can be referenced from your script to import all those typings. Again, add this `typings` directory to your version control system's ignore list if you use a VCS. 

    These typings will be added automatically to the list in the `tsd.json` file created in the previous step.

    Use the following command to re-install configured typings, e.g. after checking out the repository from version control, or if you cleaned up/deleted the `typings` directory: `tsd install`

## Next step

Continue with [Getting started - Creating your first script](creating-your-first-script)
