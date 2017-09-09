---
title: npm
date: 2017-03-11 09:55:19
tags: [npm,publish]
categories: node
---
# NPM

node package manager

## update

* npm self
    So easy:
    ```bash
    npm install -g npm@...
    ```
<!-- more -->

## publish

You can check more like automatically release npm package on Travis CI.

### login

If you already have a token, then don't need to login again.  
You can check it at this directory `~/.npmrc`  
After login: `npm publish`, done.

## settings

These are some of `package.json` settings, usually could get basic settings by `npm init`. The **blod** means commands may work for <https://www.npmjs.com/> to show. *Italic* means automatically build site like Travis CI would use.

* **with global install**  
    If this package is npm command line pkg, maybe run at global. It need this: `"preferGlobal": true`

* command line  
    Always sets with global.
    ```js
    "bin": { "chr": "./index.js" }
    ```

* *scripts*  
    The command when use npm * to run.For example:
    ```js
    "scripts": {
        "test": "mocha"
    },
    ```
    means `npm test` is the same as `mocha`.  
    ~~So why i use more words to instead of less words.😒~~ Is that what I want? Look at the *Italic*.

* keywords  
    ~~nothing to say.~~

* ***repository***  
    First of settings that both npm&travis uses. But, actually nothing could introduct.
    ```js
    "repository": {
        "type": "git",
        "url": "https://github.com/Chreem/chr-mock"
    },
    ```

## trouble

### npm提示 XXX No repository field

好吧，提示都这么明显了，参考上文***repository***
