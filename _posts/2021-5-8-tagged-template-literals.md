---
layout: post
title: Tagged Template Literals(翻译)
permalink: /tagged-template-literals/
---

Tagged template literal is the advanced concept of template strings, it allows you to pass a string through a function. The first argument of the tag function contains an array of string values from your template literals, the rest arguments contain the expressions of your template literal.
标记的模板字符串（Tagged template literals)是模板字符串(template strings)的一个高级概念。通过它你可以给一个参数化的函数（标记函数）传递一个字符串。而标记函数接收到的第一个参数就是字符串数组(string[])，数组的每个成员就是模板中的字符串内容。第二个参数起，每个参数都对应一个模板中的表达式（${表达式}）。

So what, why should I know this thing? Well, the tag function can make your string become dynamic, it can then perform whatever operations on these arguments you wish, and return the manipulated string. It’s hard and obscure to envision without examples, this concept made me quite confused for a while. Now let’s make some examples to clarify this thing.
那这个与我何干呢？为啥我需要知道这个？通过标记函数字符串就被动态化了，你可以对传入的参数执行任何希望的操作，最后再返回处理后的字符串。我知道这个光听起来有点晦涩难懂，这个概念本身也让我困惑过。现在还是先来看几个栗子吧。

Firstly, we need to define some variables act as expressions inside the template literal:
首先，需要先定义几个作为表达式的变量以及包含它们的模板字符串：
```JavaScript
var a = 5;
var b = 7;
var c = 10;

var d = `Hello world ${a}, ${b}, ${c}`;
```
Now let’s define a tag function and add it to the front of our variable d:
接下来是标记函数的定义。我们把函数声明放在变量d的前面（也就是函数调用前面）：

```JavaScript
var a = 5;
var b = 7;
var c = 10;
function taggedTemplate(strings, ...values) {
    console.log(strings);
    console.log(values);
}
var d = taggedTemplate `Hello world ${a} ${b} ${c}`;
```
Let’s run this code real quick to see what we get:
直接来看运行结果吧：
```JavaScript
[ 'Hello world ', ' ', ' ', '' ]
[ 5, 7, 10 ]
```
Basically, the content inside the template literal will be passed in the taggedTemplate function. The first argument is the string values, which is an array contains all of the string inside the template literal. The rest argument contains the values of expressions inside the template string.
基本上模板字符串内的所有内容都被传递到标记函数内了。如前面讲的，第一个参数是模板字符串对应的字符串数组（可以看到，每个表达式构成了数组的分割点）。而剩下的参数则分别对应了模板字符串中引用的表达式。
so what exactly are string values we pass into the first argument of the taggedTemplate function?
那么我们传给标记函数模板字符串后，发生了啥呢？
Start from the beginning of the template, the taggedTemplate function searches for the string values, whenever it finds the expression, it stops and adds this string to the strings array, which is our first argument:
标记函数拿到模板字符串后，从头开始查找，一旦找到一个表达式它就停下来把前面找到的字符串放到函数接收到的第一个参数--字符串数组中。这样第一个参数就形成了。

>`Hello world ${a} ${b} ${c}`;
found an expression ${a} here, stops and add string "Hello world " to the array:
找到表达式${a}，把前面的"Hello world "添加到数组中：
```JavaScript
["Hello world "]
```
>Then it continues this process with the rest of the string to find the string values to add to the first argument of taggedTemplate function:
继续在余下的字符串上查找并重复操作：
>` ${b} ${c}`;found an expression ${b} here, stops add add string " " (just a space string) to the array:
这次找到表达式${b}，于是把前面的字符串（也就是一个空格）添加到数组中：
```JavaScript
["Hello world ", " "]
```
>Do it again:` ${c}` found an expression ${c} here, stop and add the string before this expression to the string array, it also adds the empty string after the expression ${c} to the string array:
继续查找又找到表达式${c}，同样一个空格被追加到了数组中：
```JavaScript
["Hello world ", " ", " ", ""]
```
>Next, we examine the values of the rest arguments in the taggedTemplate function, those values are the expressions inside the template literal, the result is stored in an array:
下面来看看标记函数接收到的其他参数。这些参数的值对应的就是模板中的表达式：
```JavaScript
${a}, ${b}, ${c}
[5, 7, 10]
```
So again we have those values are passed into the taggedTemplate function:
好了，参数全了。

```JavaScript
strings = ["Hello world ", " ", " ", ""]
values = [5, 7, 10]
```
However, why the second parameter is ...values and not values? Because we want to take an arbitrary number of the elements when we pass to the second argument, the ... 3 dots syntax is the rest operator, which we will learn more later. Basically it takes all the rest of the arguments we pass to the function and dense it to an array as we have in the values argument. But why don’t just use taggedTemplate(strings, a, b, c)? We don’t know how many numbers of expressions we have in the template literal, so when we have 4 expressions we have to change the parameters of the function again (taggedTemplate(strings, a, b, c, d)), the same thing happens with 5, 6, 100…expressions. With rest operator, problem solved when we store all the rest arguments to the one unite array.
但是有同学可能会问，为什么第二个参数声明前需要加上...？...是一个特殊的操作符，可以帮助我们将动态的参数列表变成数组。因为标记函数接收到的参数列表是与模板中的表达式数量有关的，写死它就失去了灵活性。因此，此处使用...操作符来接收动态数量的参数列表。
Let’s solidify this by do one more example:
再看个例子来巩固一下：
```JavaScript
var name = "Nam"
var age = 17;
function taggedTemplate(strings, ...values) {
    console.log(strings); // ["This person ", " is ", " years old."]
    console.log(values); // ["Nam", 17]
    var string = strings[0]; // "This person "
    var getAge = values[0]; // 17
    var isAdult;
    if (getAge >= 18) {
        isAdult = "an adult"
    } else {
        isAdult = "not an adult";
    }
    return `${string} ${name} is ${isAdult}`
}
var d = taggedTemplate `This person ${name} is ${age} years old.`;
console.log(d); // This person  Nam is not an adult
```
This example is quite the same as the thing we did above, we just added an extra step by logging out the value of d variable, and inside the taggedTemplate function we have 2 variables and we assign the values of the template literal to those variables, then underneath them, we check whether the age is equal or greater than 18 or not then store this value to the variable isAdult. Finally, we return the value with some variables we defined within this function. Note that we also can return a template literal.
这个例子和上面的例子类似，只是我们把保存标记函数处理结果的变量d的值打印出来了。此外，里面还有一个是否成年的逻辑检查，最终处理后的字符串被返回并赋值给了变量d