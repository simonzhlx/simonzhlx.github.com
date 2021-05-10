---
layout: post
title: Javascript Generator Function
permalink: /js-generator/
---

## Key Facts About Generator Functions
A generator function is quite different from normal functions. To create a generator, we will have to use “function*” as the syntax construct.

Providing an “*” after a function keyword indicates you’re creating a generator function. Here's a generator function example:
``` JavaScript
function* generateNumber(){
    yield 1;
    yield 2;
    yield 3;
    return 4;
}
```

The code snippet above displays what the definition of a generator function will look like. The “yield” keyword pauses/resumes the generator function and returns the value of the expression to the generator’s caller. Whenever a generator function is called to execute and we try to log its type, it will return the output like this:

``` JavaScript
function* generateNumber(){
    yield 1;
    yield 2;
    yield 3;
    return 4;
}

const generator = generateNumber();
console.log(generator);// {}
console.log(typeof (generator)) // object
```

A generator function returns a special object called “generator object,” which is a type of iterators. An iterator is an object that defines an iteration sequence (like an Array in JavaScript) and potentially returns a value upon its termination.

## How a Generator Function Works

Calling generator functions is not similar to calling normal regular functions. To call a generator function, we will need to call it using the “next()” method. When we call a generator function with the next() method, it will run the execution until the nearest yield “<value>” statement. The value can be omitted, and it will return “undefined.” The function execution is paused, and the yielded value returns to the generator’s caller.

The result of next() is always an object with two properties:

- Value: the yielded value.
- Done: true if the function code is finished else will return false.
Let’s see what the same snippet returns when called with the next() method:

``` JavaScript
function* generateNumber(){
    yield 1;
    yield 2;
    yield 3;
    return 4;
}

const generator = generateNumber();
console.log(generator.next());// {value: 1, done: false}
console.log(generator.next());// {value: 2, done: false}
console.log(generator.next());// {value: 3, done: false}
console.log(generator.next());// {value: 4, done: true}
```

In the code snippet above, the value and done are returned for each “yield” and “return” keyword. With the last statement of function (the return keyword) changing from false to true to indicate that it reached the last statement of a function, the generator function execution must now stop. This is how the generator function is called and values are retrieved from it.

## Generator without any return statement:

The generator function will stop the execution without having a return statement in the function. This is how the code will look:

``` JavaScript
function* generateNumber(){
    yield 1;
    yield 2;
    yield 3;
}

const generator = generateNumber();
console.log(generator.next());// {value: 1, done: false}
console.log(generator.next());// {value: 2, done: false}
console.log(generator.next());// {value: 3, done: false}
console.log(generator.next());// {value: undefined, done: true}
```

In the code snippet above, we haven’t returned any values from a generator. Once all yield <value> is resolved, and we call on the next() method, it will then return a value as “undefined” and stops the generator statement execution, and finishes with the value “true.” 

## Browser Compatibility

![browser-compatibility](/images/browser-compatibility-generator.png)

Check out the other methods available for generator functions besides the next() method:

+ Return(): Returns the given value and finishes the execution of the generator.
+ Throw(): Throws an error to a generator and finishes the execution of the generator.
