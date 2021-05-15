---
layout: post
title: Require VS Import
permalink: /require-vs-import/
---

## require() in JavaScript
This is an in-built node.js statement and is most commonly used to include modules from other separate files. It works by reading the JS file, executing it, and returning the object being exported. It is very popular for its ability to incorporate core node modules, community-based, and even local modules.

We can use require() to include an in-built module in JavaScript.
```JavaScript
const express = require ("express");
```
It is also easy to include a local module.
```JavaScript
require("local_module");
```
As illustrated above, node.js will try to find the local_module.js in the paths specified. The output will defer according to what node finds: if the file exists in the path specified, the output will display the contents of the module. If it does not, you may receive an error such as the one below:
```JavaScript
Error: Cannot find module 'local_module'
    at Function.Module._resolveFilename (module.js:470:15)
    at Function.Module._load (module.js:418:25)
    at Module.require (module.js:498:17)
    at require (internal/module.js:20:19)
    at repl:1:1
    at ContextifyScript.Script.runInThisContext (vm.js:23:33)
    at REPLServer.defaultEval (repl.js:336:29)
    at bound (domain.js:280:14)
    at REPLServer.runBound [as eval] (domain.js:293:12)
    at REPLServer.onLine (repl.js:533:10)
```

## import() in JavaScript
import() is an ES6 module. Together with export(), they are commonly referred to as ES6 import and export. This essentially means that the statements can not be used with other file types outside the ES modules.

The import() statement lets the user import his modules and use them in the program. The syntax is:
```JavaScript
import './this-module.js';
```
The user specifies the file path to the module being imported.

## Major Differences Between require and import in JavaScript
While require() is a node.js statement that uses CommonJS, import() is used only with ES6.
**require() remains where it has been put in the file (non-lexical), and import() always moves to the top**.
require() can be called for use at any point in the program, but import() can only be run at the beginning of the file.

Most people run code with the require() statement directly but prefer to use the experimental module feature flag when working with import().

Finally, all files using the require() statement are saved as .js files, while those with import() can only be saved as .mjs files.

They cannot be used in a program at the same time.

Generally, import() is more preferred because it allows the user to choose and load only the pieces of the module that they need. The statement also performs better than require() and saves some memory.