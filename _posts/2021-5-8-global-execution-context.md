---
layout: post
title: Global Execution Context
permalink: /global-execution-context/
---

In Javascript, an execution context is an abstract container that holds information about the environment that a piece of code is being executed in. For every function call, a new execution context is created and added to the execution stack; a container that keeps information about the different execution contexts.

![execution-stack](/images/execution_stack.png)  

The global execution context is the **default execution context** created before the JS engine starts to run through any code.
>On every function call in Javascript, a new execution context is created and added to the execution stack. It is the JS engine's job to configure it and prepare for the execution. 
You might have heard terms like scope, scope chain and this.

**Scope**: Every execution context needs to define its scope; it needs to decide which functions and variables should have direct access to it.

**Scope chain**: Aside from the individual scopes of the execution context, there are also references to an execution context's outer and global scopes. This chain of reference is known as the scope chain.

>You can access global variables from local scopes, but cannot access local variables from global scopes.

**this**: This (this keyword) is a special variable that allows access to an object to which the current code is being executed. In the global execution context, this allows access to the Window object.
