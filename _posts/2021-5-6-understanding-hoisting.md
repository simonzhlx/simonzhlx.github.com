---
layout: post
title: 理解JS中的hoisting(翻译)
permalink: /hoisting-javascript/
---

By Mabishi Wakio
原文地址：https://www.digitalocean.com/community/tutorials/understanding-hoisting-in-javascript
# 理解JS中的Hoisting

## Introduction
## 介绍
In this tutorial, we’ll investigate how the famed hoisting mechanism occurs in JavaScript. Before we dive in, let’s get to grips with what hoisting is.
本文将对javascript中著名的hoisting机制进行研究。在深入钱，我们先来看看hoisting到底是个啥。

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.
Hoisting就是js的一个机制，它会在代码真正执行前，先将函数与变量声明部分移至所在scope的顶部。

Inevitably, this means that no matter where functions and variables are declared, they are moved to the top of their scope regardless of whether their scope is global or local.
这就意味着不管函数或是变量声明在什么位置，相关scope是global还是local，这些声明都会被移至顶部。

Of note however, is the fact that the hoisting mechanism only moves the declaration. The assignments are left in place.
然而值得注意的是，hoisting机制只会移动声明部分，赋值部分还是会留在原地。

If you’ve ever wondered why you were able to call functions before you wrote them in your code, then read on!
想知道为什么你可以先写函数调用，后写函数声明，那请读下去。

## undefined vs ReferenceError
Before we begin in earnest, let’s deliberate on a few things.
正式开始介绍前，让我们先来仔细考虑一下几件事情。
```javascript
console.log(typeof variable); // Output: undefined
```
This brings us to our first point of note:
上述代码的运行结果引出了我们第一个需要注意的点：

In JavaScript, an undeclared variable is assigned the value undefined at execution and is also of type undefined.
JS中，一个未声明的变量会在执行时指定为undefined，同时，它的类型也是undefined。

Our second point is:
下面代码带来我们第二个需要注意的点：
```javascript
console.log(variable); // Output: ReferenceError: variable is not defined
```
In JavaScript, a ReferenceError is thrown when trying to access a previously undeclared variable.
JS这种，尝试去访问一个前面没有声明过的变量会收到一个引用错误(ReferenceError)。
The behaviour of JavaScript when handling variables becomes nuanced because of hoisting. We’ll look at this in depth in subsequent sections.
因为hoisting机制，JS中处理变量时的行为会有些许差异。接下来的部分我们来深入了解一下。

## Hoisting variables
The following is the JavaScript lifecycle and indicative of the sequence in which variable declaration and initialisation occurs.
下图中展示的就是JS生命周期中，变量声明与初始化的部分。

![variable-hoisting](/images/hoisting-flow.png)  

However, since JavaScript allows us to both declare and initialise our variables simultaneously, this is the most used pattern:
因为JS允许我们在声明的同时来初始化变量，因此下面是你最熟悉的方式：
```javascript
var a = 100;
```
It is however important to remember that in the background, JavaScript is religiously declaring then initialising our variables.
JS中很推崇声明后马上初始化变量的方式。

As we mentioned before, all variable and function declarations are hoisted to the top of their scope. I should also add that variable declarations are processed before any code is executed.
前面提到过，所有变量和函数都会在执行前移至scope的顶部。此处再增加一点，变量声明会被优先处理。

However, in contrast, undeclared variables do not exist until code assigning them is executed.
Therefore, assigning a value to an undeclared variable implicitly creates it as a global variable when the assignment is executed. This means that, all undeclared variables are global variables.
与声明的变量比起来，未声明变量在没有执行到赋值语句时根本不存在。因此，给一个未声明变量赋值实际上是隐式地把它作为一个全局变量创建了。也就是说，所有未声明变量都是全局变量。。。
To demonstrate this behaviour, have a look at the following:
下面代码演示了这种行为：
```javascript
function hoist() {
  a = 20;
  var b = 100;
}


hoist();

console.log(a);
/*
Accessible as a global variable outside hoist() function
Output: 20
*/

console.log(b);
/*
Since it was declared, it is confined to the hoist() function scope.
We can't print it out outside the confines of the hoist() function.
Output: ReferenceError: b is not defined
*/
```
Since this is one of the eccentricities of how JavaScript handles variables, it is recommended to always declare variables regardless of whether they are in a function or global scope. This clearly delineates how the interpreter should handle them at run time.
介于JS这种处理变量的特殊性，推荐大家不管你要使用的是函数还是一个全局变量，都先声明它。这样能清楚地让你的代码和解释器的行为一致。

## ES5
### var
The scope of a variable declared with the keyword var is its current execution context. This is either the enclosing function or for variables declared outside any function, global.
用var声明的变量可用范围要吗是某个function的scope要吗就是全局的。
Let’s look at a few examples to identify what this means:
来看几个例子：
### global variables
全局可用变量
```javascript
console.log(hoist); // Output: undefined

var hoist = 'The variable has been hoisted.';
```
We expected the result of the log to be: ReferenceError: hoist is not defined, but instead, its output is undefined.
这段代码本来预期得到"ReferenceError: hoist is not defined"这样的输出，然后输出只是undefined
Why has this happened?
为啥这样呢？

This discovery brings us closer to wrangling our prey.
这个发现让我们离我们想要探讨的目标又近了一点。
JavaScript has hoisted the variable declaration. 
JS将变量声明提前了。
This is what the code above looks like to the interpreter:
下面是解释器视角：
```javascript
var hoist;

console.log(hoist); // Output: undefined
hoist = 'The variable has been hoisted.';
```
Because of this, we can use variables before we declare them. However, we have to be careful because the hoisted variable is initialised with a value of undefined. The best option would be to declare and initialise our variable before use.
因为JS的这个机制，我们就可以在变量声明前使用它们。但是使用时我们还要小心，因为提前的变量声明只会将变量初始化为undefined。所以最好的方式还是在使用前就声明和初始化它。

### Function scoped variables
As we’ve seen above, variables within a global scope are hoisted to the top of the scope. Next, let’s look at how function scoped variables are hoisted.
前面看到的都是全局scope的变量提升，接下来看看函数scope的变量提升。
```javascript
function hoist() {
  console.log(message);
  var message='Hoisting is all the rage!'
}

hoist();
```
Take an educated guess as to what our output might be.
基于前一部分学到的内容，我们来猜猜上面代码的输出是什么。
If you guessed, undefined you’re right. If you didn’t, worry not, we’ll soon get to the bottom of this.
如果你的答案是：undefined，那你猜对了。如果猜错了也没有关系，我们很快就会明白为什么了。
This is how the interpreter views the above code:
以下为解释器视角：
```javascript
function hoist() {
  var message;
  console.log(message);
  message='Hoisting is all the rage!'
}

hoist(); // Ouput: undefined
```
The variable declaration, var message whose scope is the function hoist(), is hoisted to the top of the function.
变量message的声明（var message;）被提升到了其scope（function hoist())的最上面。
To avoid this pitfall, we would make sure to declare and initialise the variable before we use it:
为了避免这样陷阱的出现，我们需要确认在使用它之前声明和初始化它：
```javascript
function hoist() {
  var message='Hoisting is all the rage!'
  return (message);
}

hoist(); // Ouput: Hoisting is all the rage!
``` 
### Strict Mode
Thanks to a utility of the es5 version of JavaScript known as strict-mode, we can be more careful about how we declare our variables.
感谢ES5中引入的一个特性strict-mode,通过使用它能让我们在变量如何声明、使用上更谨慎些。
By enabling strict mode, we opt into a restricted variant of JavaScript that will not tolerate the usage of variables before they are declared.
通过启用strict-mode，JS将对于未声明就使用变量的行为零容忍。

Running our code in strict mode:

Eliminates some silent JavaScript errors by changing them to explicit throw errors which will be spit out by the interpreter.
运行进入strict-mode后的代码，对于原本被忽略的一些JS错误，解释器将会选择显式地抛出它们。
Fixes mistakes that make it difficult for JavaScript engines to perform optimisations.
修复这些可以优化JS引擎执行这些代码时的效率。
Prohibits some syntax likely to be defined in future versions of JavaScript.
未来JS版本可能会在语法上来避免将这样的代码交给解释器。
We enable strict mode by prefacing our file or function with
在文件内或函数内启用strict-mode的方式如下：
```javascript
'use strict';

// OR
"use strict";
```
Let’s test it out.
来测试一下。
```javascript
'use strict';

console.log(hoist); // Output: ReferenceError: hoist is not defined
hoist = 'Hoisted';
```
We can see that instead of assuming that we missed out on declaring our variable, use strict has stopped us in our tracks by explicitly throwing a Reference error. Try it out without use strict and see what happens.
strict-mode通过抛出引用错误的方式提醒我们重归正途，这样我们就不会忽略掉变量声明了（爸妈再也不用担心我的声明了：））再试试没有strict-mode启用下会发生什么吧。
Strict mode behaves differently in different browsers however, so it’s advisable to perform feature testing thoroughly before relying on it in production.
因为Strict-mode在不同浏览器中的表现是不同的，建议如果在生产环境上依赖它请先做完整的测试。

## ES6
ECMAScript 6, ECMAScript 2015 also known as ES6 is the latest version of the ECMAScript standard, as the writing of this article, Jan 2017 and introduces a few changes to es5.
ES6在本文写作时还是EMAScript的最新标准。（译者注：后续接连出了ES7--2016年，ES8--2017年，以及ES9--2018年）
Of interest to us is how changes in the standard affect the declaration and initialisation of JavaScript variables.
我们关注的还是在标准的不断更迭中，是如何影响JS变量的声明和初始化的。
### let
Before we start, to be noted is the fact that variables declared with the keyword let are block scoped and not function scoped. That’s significant, but it shouldn’t trouble us here. Briefly, however, it just means that the variable’s scope is bound to the block in which it is declared and not the function in which it is declared.
开始前请注意，用let声明的变量是块Scope的而不是函数scope的。这个区别很明显，但是目前不需要纠结于此。简言之，这个变量是绑定到声明它的块而非声明它的函数。

Let’s start by looking at the let keyword’s behaviour.
先来看看let关键字。
```javascript
console.log(hoist); // Output: ReferenceError: hoist is not defined ...
let hoist = 'The variable has been hoisted.';
```
Like before, for the var keyword, we expect the output of the log to be undefined. However, since the es6 let doesn’t take kindly on us using undeclared variables, the interpreter explicitly spits out a Reference error.
惯性思维我们预期此处的输出应该是undefined。然而，es6中引入的let关键字对于未声明的变量就没有这么友好了。解释器会直接给出一个引用错误。
This ensures that we always declare our variables first.
以此来确保我们总是先声明所需的变量。
However, we still have to be careful here. An implementation like the following will result in an ouput of undefined instead of a Reference error.
然而，使用let时还是需要小心。像下面的代码片段输出的内容就又变回了undefined而不是引用错误。
```javascript
let hoist;

console.log(hoist); // Output: undefined
hoist = 'Hoisted'
```
Hence, to err on the side of caution, we should declare then assign our variables to a value before using them.
因此，慎之又慎，我们还是应该在使用变量前，先声明然后给它赋值。
###　const
The const keyword was introduced in es6 to allow immutable variables. That is, variables whose value cannot be modified once assigned.
const关键字在es6中引入来帮助声明常量。常量就是那些一旦赋值就不能再修改的变量。
With const, just as with let, the variable is hoisted to the top of the block.
和let一样，用const修饰的常量也会被提升至块的顶部。
Let’s see what happens if we try to reassign the value attached to a const variable.
```javascript
const PI = 3.142;

PI = 22/7; // Let's reassign the value of PI

console.log(PI); // Output: TypeError: Assignment to constant variable.
``` 
How does const alter variable declaration? Let’s take a look.
我们来看看Const是如何改变变量声明的。
```javascript
console.log(hoist); // Output: ReferenceError: hoist is not defined
const hoist = 'The variable has been hoisted.';
``` 
Much like the let keyword, instead of silently exiting with an undefined, the interpreter saves us by explicitly throwing a Reference error.
解释器处理const声明的常量与let声明的变量是类似的，对于上述未声明直接使用，都是直接抛出一个引用错误。
The same occurs when using const within functions.
函数内的表现也一样
```javascript
function getCircumference(radius) {
  console.log(circumference)
  circumference = PI*radius*2;
  const PI = 22/7;
}

getCircumference(2) // ReferenceError: circumference is not defined
```  
With const , es6 goes further. The interpreter throws an error if we use a constant before declaring and initialising it.
Our linter is also quick to inform us of this felony:
通过const，ES6更近了一步，除了解释器会对未声明和初始化的变量检查并抛异常外，语言检查工具也能快速发现此类问题并通知我们类似下面的信息：
PI was used before it was declared, which is illegal for const variables.
PI在声明前就使用了，这种方式对于const变量是非法的。

Globally,
来看看全局scope的例子
```javascript
const PI;
console.log(PI); // Ouput: SyntaxError: Missing initializer in const declaration
PI=3.142;
``` 
Therefore, a constant variable must be both declared and initialised before use.
因此，一个常量使用前必须声明和初始化完。
As a prologue to this section, it’s important to note that indeed, JavaScript hoists variables declared with es6 let and const. The difference in this case is how it initialises them.
Variables declared with let and const remain uninitialised at the beginning of execution whilst variables declared with var are initialised with a value of undefined.
就如开篇所说，不同关键字声明的变量在提升时是有差异的。用let和const的变量声明确实也会被提升，但是不会为它们的变量做初始化。这也是不同于用var关键字声明的变量的地方。后者声明提升后还会初始化变量值为undefined。

## Hoisting functions
## 函数提升
JavaScript functions can be loosely classified as the following:
JS的函数可以简单的分为：
Function declarations
函数声明
Function expressions
函数表达式
We’ll investigate how hoisting is affected by both function types.
我们来看看函数提升是如何被这两种函数类型影响的。
### Function declarations
### 函数声明
These are of the following form and are hoisted completely to the top. Now, we can understand why JavaScript enable us to invoke a function seemingly before declaring it.
下面的函数声明将被完整的提升到scope顶部。现在我们就能理解为何JS支持函数调用写在函数声明之前了。
```javascript
hoisted(); // Output: "This function has been hoisted."

function hoisted() {
  console.log('This function has been hoisted.');
};
```  
### Function expressions
### 函数表达式
Function expressions, however are not hoisted.
函数表达式不会被提升。
```javascript
expression(); //Output: "TypeError: expression is not a function

var expression = function() {
  console.log('Will this work?');
};
```
Let’s try the combination of a function declaration and expression.
来尝试一下两者的结合。
```javascript
expression(); // Ouput: TypeError: expression is not a function

var expression = function hoisting() {
  console.log('Will this work?');
};
``` 
As we can see above, the variable declaration var expression is hoisted but it’s assignment to a function is not. Therefore, the intepreter throws a TypeError since it sees expression as a variable and not a function.
从上面结果可以看到，通过var声明的变量express被提升了，但是给它赋值为函数的行为没有。因此解释器直接抛出一个类型错误，它只能识别expression为一个普通变量而不是函数。
## Order of precedence
## 优先顺序
It’s important to keep a few things in mind when declaring JavaScript functions and variables.
每次声明函数和变量时记住下面几件事很重要。
Variable assignment takes precedence over function declaration
变量赋值优先与函数声明
Function declarations take precedence over variable declarations
函数声明优先于变量声明
Function declarations are hoisted over variable declarations but not over variable assignments.
函数声明会被提升至高于变量声明，但是低于变量赋值。
Let’s take a look at what implications this behaviour has.
让我们来看看这种行为有那些隐含意义。
Variable assignment over function declaration
变量赋值优先于函数声明
```javascript
var double = 22;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Output: number
```
Function declarations over variable declarations
函数声明优先于变量声明
```javascript
var double;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Output: function
```
Even if we reversed the position of the declarations, the JavaScript interpreter would still consider double a function.
即使我们颠倒一下函数和变量的声明顺序，JS解释器仍然会把double视作一个函数。
Hoisting classes
类提升
JavaScript classes too can be loosely classified either as:
一样的，JS类也可以简单分成下面两种：
Class declarations
类声明
Class expressions
类表达式

Class declarations
类声明
Much like their function counterparts, JavaScript class declarations are hoisted. However, they remain uninitialised until evaluation.
This effectively means that you have to declare a class before you can use it.
和函数声明一样，类声明也会被提升。然而会保持为未初始化状态直到求值。这也意味着你必须在使用一个类前，先声明它。
```javascript
var Frodo = new Hobbit();
Frodo.height = 100;
Frodo.weight = 300;
console.log(Frodo); // Output: ReferenceError: Hobbit is not defined

class Hobbit {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
}
``` 
I’m sure you’ve noticed that instead of getting an undefined we get a Reference error. That evidence lends claim to our position that class declarations are hoisted.
相信你已经注意到了这次得到的是一个引用错误而不是undefined。这可以作为一个类声明被提升的证据。
If you’re paying attention to your linter, it supplies us with a handy tip.
关注你的linter，它也会提示你如下类似信息的：
Hobbit was used before it is declared, which is illegal for class variables
Hobbit在声明前就使用了，这种用法是非法的
So, as far as class declarations go, to access the class declaration, you have to declare first.
所以，先声明类再使用它
```javascript
class Hobbit {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
}

var Frodo = new Hobbit();
Frodo.height = 100;
Frodo.weight = 300;
console.log(Frodo); // Output: { height: 100, weight: 300 }
``` 
Class expressions
类表达式
Much like their function counterparts, class expressions are not hoisted.
和函数表达式一样，类表达式也不会被提升。
Here’s an example with the un-named or anonymous variant of the class expression.
下面的例子是一个匿名类表达式
```javascript
var Square = new Polygon();
Square.height = 10;
Square.width = 10;
console.log(Square); // Output: TypeError: Polygon is not a constructor

var Polygon = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
``` 
Here’s an example with a named class expression.
接下来是一个命名的类表达式例子
```javascript
var Square = new Polygon();
Square.height = 10;
Square.width = 10;
console.log(Square); // Output: TypeError: Polygon is not a constructor


var Polygon = class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
``` 
The correct way to do it is like this:
下面为正确的方式：
```javascript
var Polygon = class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};

var Square = new Polygon();
Square.height = 10;
Square.width = 10;
console.log(Square);
```
## Caveat
## 警告
There’s a bit of an argument to be made as to whether Javascript es6 let, const variables and classes are actually hoisted, roughly hoisted or not hoisted. Some argue that they are actually hoisted but uninitialised whilst some argue that they are not hoisted at all.
关于let,const声明的变量以及类是否真的被提升了，目前还有一点争论。
## Conclusion
## 结论
Let’s summarise what we’ve learned so far:
总结一下我们了解到的内容：
While using es5 var, trying to use undeclared variables will lead to the variable being assigned a value of undefined upon hoisting.
使用var声明的变量，如果声明前就使用了会先导致声明提升紧接着被赋值为undefined。
While using es6 let and const, using undeclared variables will lead to a Reference Error because the variable remains uninitialised at execution.
而let和const声明的变量，如果声明前就使用了会引发一个引用错误，因为相关变量仍然未被初始化。
Therefore,
We should make it a habit to declare and initialise JavaScript variables before use.
因此我们应该养成先声明、初始化再使用的习惯。
Using strict mode in JavaScript es5 can help expose undeclared variables.
使用strict-mode可以帮助发现那些未声明的变量。