---
layout: post
title:  "JavaScript debugging tips"
categories: JavaScript
tags: JavaScript,Debugging
author: moai
---

출처 : [https://raygun.com/javascript-debugging-tips][출처]  

## 1. debugger;
After console.log, 'debugger;' is my favorite quick and dirty debugging tool. Once it's in your code, Chrome will automatically stop there when executing. You can even wrap it in conditionals, so it only runs when you need it.

```javascript
if (thisThing) {
    debugger;
}
```
## 2. Display objects as a table
Sometimes, you have a complex set of objects that you want to view. You can either console.log them and scroll through the list, or break out the console.table helper. Makes it easier to see what you’re dealing with!

```javascript
var animals = [
    { animal: 'Horse', name: 'Henry', age: 43 },
    { animal: 'Dog', name: 'Fred', age: 13 },
    { animal: 'Cat', name: 'Frodo', age: 18 }
];
 
console.table(animals);
```




Will output:  
![jd1](/assets/images/2019-08-02-javascript-debugging-1.png){: .center}   

## 3. How to find your DOM elements quickly
Mark a DOM element in the elements panel and use it in your console. Chrome Inspector keeps the last five elements in its history so that the final marked element displays with $0, the second to last marked element $1 and so on.

If you mark following items in order ‘item-4′, ‘item-3’, ‘item-2’, ‘item-1’, ‘item-0’ then you can access the DOM nodes like this in the console:  
![jd2](/assets/images/2019-08-02-javascript-debugging-2.png){: .center}

## 4. Benchmark loops using console.time() and console.timeEnd()
It can be super useful to know exactly how long something has taken to execute, especially when debugging slow loops. You can even set up multiple timers by assigning a label to the method. Let’s see how it works:
```javascript
console.time('Timer1');
 
var items = [];
 
for(var i = 0; i < 100000; i++){
   items.push({index: i});
}
 
console.timeEnd('Timer1');
```

This has produced the following result:  
![jd3](/assets/images/2019-08-02-javascript-debugging-3.png){: .center}

## 5. Quick-find a function to debug
Let’s say you want to set a breakpoint in a function.

The two most common ways to do that is:

1. Find the line in your inspector and add a breakpoint
2. Add a debugger in your script

In both of these solutions, you have to click around in your files to find the particular line you want to debug

What’s probably less common is to use the console. Use debug(funcName) in the console and the script will stop when it reaches the function you passed in.

It’s quick, but the downside is it doesn’t work on private or anonymous functions. But if that’s not the case, it’s probably the fastest way to find a function to debug. (Note: there’s a function called console.debug which is not the same thing.)
```javascript
var func1 = function() {
	func2();
};
 
var Car = function() {
	this.funcX = function() {
		this.funcY();
	}
 
	this.funcY = function() {
		this.funcZ();
	}
}
 
var car = new Car();
```
Type debug(car.funcY) in the console and the script will stop in debug mode when it gets a function call to car.funcY:  
![jd4](/assets/images/2019-08-02-javascript-debugging-4.png){: .center}

## 6.  Black box scripts that are NOT relevant
Today we often have a few libraries and frameworks on our web apps. Most of them are well tested and relatively bug-free. But, the debugger still steps into all the files that have no relevance for this debugging task. The solution is to black box the script you don’t need to debug. This could also include your own scripts. [Read more about debugging black box in this article.](https://developer.chrome.com/devtools/docs/blackboxing)

## 7. Find the important things in complex debugging
In more complex debugging we sometimes want to output many lines. One thing you can do to keep a better structure of your outputs is to use more console functions, for example, Console.log, console.debug, console.warn, console.info, console.error and so on. You can then filter them in your inspector. But sometimes this is not really what you want when you need to debug JavaScript. It’s now that YOU can get creative and style your messages. Use CSS and make your own structured console messages when you want to debug JavaScript:

```javascript
console.todo = function(msg) {
	console.log(‘ % c % s % s % s‘, ‘color: yellow; background - color: black;’, ‘–‘, msg, ‘–‘);
}
 
console.important = function(msg) {
	console.log(‘ % c % s % s % s’, ‘color: brown; font - weight: bold; text - decoration: underline;’, ‘–‘, msg, ‘–‘);
}
 
console.todo(“This is something that’ s need to be fixed”);
console.important(‘This is an important message’);
```
Will output:   
![jd5](/assets/images/2019-08-02-javascript-debugging-5.png){: .center}

For example:

In the console.log() you can set %s for a string, %i for integers and %c for custom style. You can probably find better ways to use this. If you use a single page framework, you maybe want to have one style for view message and another for models, collections, controllers and so on. Maybe also name the shorter like wlog, clog and mlog use your imagination!

## 8. Watch specific function calls and its arguments
In the Chrome console, you can keep an eye on specific functions. Every time the function is called, it will be logged with the values that it was passed in.
```javascript
var func1 = function(x, y, z) {
//....
};
```
Will output:   
![jd6](/assets/images/2019-08-02-javascript-debugging-6.png){: .center}

This is a great way to see which arguments are passed into a function. But I must say it would be good if the console could tell how many arguments to expect. In the above example, func1 expect 3 arguments, but only 2 is passed in. If that’s not handled in the code it could lead to a possible bug.

## 9. Quickly access elements in the console
A faster way to do a querySelector in the console is with the dollar sign. $(‘css-selector’) will return the first match of CSS selector. $$(‘css-selector’) will return all of them. If you are using an element more than once, it’s worth saving it as a variable.
![jd7](/assets/images/2019-08-02-javascript-debugging-7.png){: .center}

## 10. Postman is great (but Firefox is faster)
Many developers are using Postman to play around with ajax requests. Postman is excellent, but it can be a bit annoying to open up a new browser window, write new request objects and then test them.

Sometimes it’s easier to use your browser.

When you do, you no longer need to worry about authentication cookies if you are sending to a password-secure page. This is how you would edit and resend requests in Firefox.

Open up the inspector and go to the network tab. Right-click on the desired request and choose Edit and Resend. Now you can change anything you want. Change the header and edit your parameters and hit resend.

Below I present a request twice with different properties:  
![jd8](/assets/images/2019-08-02-javascript-debugging-8.png){: .center}

## 11. Break on node change
The DOM can be a funny thing. Sometimes things change and you don’t know why. However, when you need to debug JavaScript, Chrome lets you pause when a DOM element changes. You can even monitor its attributes. In Chrome Inspector, right click on the element and pick a break on setting to use:  
![jd9](/assets/images/2019-08-02-javascript-debugging-9.png){: .center}

[출처]: https://raygun.com/javascript-debugging-tips