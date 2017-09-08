---
title: typescript
date: 2017-03-25 13:50:54
tags: [typescript]
categories: js
---
# TypeScript

Some introduction by object:

```js
{
    "use":"provided strong type for javascript",
    "site":"http://www.typescriptlang.org",
    "github":"https://github.com/Microsoft/TypeScript",
    "how-to-start":"npm i -g typescript"
}
```

## config file

ts' config file was named `tsconfig.json` which by following this command created: `tsc --init`, watch more about this file:  

### compiler option

* include files exclude  
    These three options was introducted by ts official website:

    > If the "files" and "include" are both left unspecified...(some other words)

    A simple summary: `include` < `exclude` < `files`
* outDir  
    How to use watches this name. There are talking about include & exclude.  
    Default action of using `tsc` is exclude outDir directory unless using `files`.