---
id: 21
title: "call, apply and bind methods in JavaScript"
date: "2020-01-03"
language: "en"
tags:
  - javascript
  - this
  - call
  - bind
  - apply
original_url: "https://developingschool.com/a/21/call-apply-and-bind-methods-in-javascript/"
source: "developingschool.com"
---

# call, apply and bind methods in JavaScript

Each of call, apply and bind methods in JavaScript are pretty similar, but there is a slight difference between them. If you knew one of them, it would be easy to know the other two. Let's begin with the call method. What is the call method and what is its importance?

### Call Method

The call method is a pre-defined method in JavaScript, and it belongs to Function object prototype like this "Function.prototype.call". So, when you define any function in JavaScript, this function will be able to access any property or method of the Function object. As you know; All functions in JavaScript are considered objects in the first place. So, We can use the "call" method with JavaScript functions.

The question is; what does the call method do? Simply, it invokes the functions. Let's take an example:-

```javascript
function printMyName(){
	console.log('My Name Is ....');
}

printMyName.call(); // "My Name Is ...."
```

As you see, the call method invoked the "printMyName" function. At this point nothing is new. As you know we can invoke any function in the routine way by using the parentheses without the call method.

Here is the start; The call method didn't come to only invoke the functions, no, It has a great role more than that.

Let's say; if you find call, apply or bind methods, look up to the "this" keyword or look forward the context. The main role of call method is that it changes the context of functions and methods. And as you know we point to the context via "this" keyword. Let's take an example:-

```javascript
var User_Mark = {
	id: 5,
	name: 'Mark',
	printUserName: function(){
		console.log('The User Name Is ' + this.name);
	}
};
User_Mark.printUserName(); // 'The User Name Is Mark'
```

If we talked about the previous code, we would say that the "this" keyword is pointing to the context of the executive object. Some people may say that the "this" keyword points to the context of the parent object which has the invoked method.

Anyway, in some cases we may need to change the context of the invoked methods or functions to another context. So, we need to make the "this" keyword to point to another objects rather than the default ones.

Here, the role of call, apply and bind methods comes. Let's define another object and invoke the User\_Mark.printUserName method but with a different context i.e. different object:-

```javascript
var User_Sarah = {
	id: 9,
	name: 'Sarah'
};
User_Mark.printUserName.call(User_Sarah); // 'The User Name Is Sarah
```

In the previous code, we've invoked the printUserName by using call method. It was expected that it will print "mark" but it printed "Sarah" instead. The reason behind that is the context has been changed. And since the context has been changed the "this" keyword become point to another object which is "User\_Sarah". And that's considered the core point of call method.

Before we go through the syntax, let's ask a question; what's about parameters? what if we need to pass parameters into the invoked function ? How would we do that?

```javascript
var User_Mark = {
	name: 'Mark',
	printUsername: function(upperCase){
		let name = this.name;
		if(upperCase === true){
			name = name.toUpperCase();
		}
		console.log('The User Name Is ' + name);
	}
};

var User_Sarah = {
	name: 'Sarah'
};

User_Mark.printUsername.call(User_Sarah, true); // The User Name Is SARAH
```

The call method takes many paramters. The first one is the important one. The first one is the context which "this" keyword will point to. The other paramters will be passed into the invoked function. As you see in the previous code; The call function has taken two paraments. The first one is the new context "User\_Sarah". The second one will be passed into printUsername.

All of the above is the same with "bind' and "apply" methods but with some differences. The main idea is the same. All of "call", "bind" and "apply" came to change the context. Or in another say; they came to make "this" keyword refer to different objects.

Let's have a look at the syntax of call method.

`function.call(contextualObj, param1, param2, ...)`

As we mentioned above; the call method takes many parameters. The most interesting one is the first one which is the new context "object" that "this" keyword will refer to. The rest of parameters will be passed to the invoked function.

### Difference between call and apply methods

The "apply" method is so similar to "call" method. The main difference is in the syntax. The apply method takes only two parameters. The first one is "context" object that "this" keyword will point to as in the "call" method. The second parameter is an array whose elements will be passed to the invoked function. Let's look at the syntax of the apply method;

`function.apply(contextualObj, paramsArray)`

As you see; the second parameter is an array whose elements will be passed into the function that we will invoke by "apply" method. Let's take an example;

```javascript
var context_A = {
	contextName: 'context_A',
	calc: function(a, b){
		let sumVal = a + b;
		console.log("output of " + this.contextName + " is " + sumVal);
	}
};
context_A.calc(2, 3); // output of context_A is 5

var context_B = {
	contextName: 'context_B'
};
// using apply 
context_A.calc.apply(context_B, [6, 4]); // output of context_B is 10
```

### Difference between call and bind methods

The "bind" method is too similar to "call" method. The main difference lies in the invocation time. Once you invoke any function by "call" method, it will be immediately invoked. On the other hand, when you try to invoke a function by "bind" method, it will not be invoked as the "call" method but it will return a bounded function for later use. Let's have a look at the syntax of bind method and take an example;

`boundedFunc = function.bind(contextualObj, param1, param2, ...)`

```javascript
var User_Mark = {
	name: 'Mark',
	printUserName: function(upperCase){
		let name = this.name;
		if(upperCase === true){
			name = name.toUpperCase();
		}
		console.log('The User Name Is ' + name);
	}
};

var User_Ahmed = {
	id: 9,
	name: 'Ahmed'
};
/*
* note)-> return a bounded function that 
* will be executed later
*/
var bf = User_Mark.printUserName.bind(User_Ahmed, true);
// execute the bounded function
bf();
```

As we've seen in the previous snippet, the "bind" method has returned a bounded function with the desirable context for later use. Many of use cases of "bind" method are using with callback functions whereas the different context and later invocation.

In the end, we can say that if you find any of these methods, look up the "this" keyword or the context. We can also say if you need to change the context or need to make the "this" keyword point to another object, think about call, apply or bind methods. All of them are responsible for that and the differences between them lies in the syntax or invocation time.
