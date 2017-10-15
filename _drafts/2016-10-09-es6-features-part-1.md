---
layout: post
title: ES2015 features part 1 simple syntax
description: Many frontend JS frameworks like React or AngularJS 2 are taking advantage of new ES2015 features. Learn ES2015 would provide a better understanding about those frameworks.
date: 2016-10-09
tags: [javascript, es6]
comments: true
share: false
---

The evolution of Javascript for the last several years is incredible. From
frontend, backend to robotics, Javascript is everywhere. The language
moves so quickly that I barely keep up with the pace. And there is ES6. People
might think that ES6 has not been fully supported so you wouldn't need it, at
least for now. However, many new Javascript frameworks such as AngularJS 2 or
React are taking advantage of those ES6 features. I believe that learning ES6
would give me a solid foundation and better understanding about those
frameworks. I decided to learn ES6 after finishing a React introduction course.
Here are what I have learnt from ES6.

## Block-scoped declarations

Before ES6, Javascript has only function scope variables.

```javascript
(function() {
  for (var i = 0; i < 10; i++) {
    console.log(i);
  }
  console.log(i)      // 10, still defined outside the block
})();
```

ES6 introduces block-scoped declarations `let` and `const`.

### let

block-scoped variables are defined inside a pair of curly braces with the
keyword `let`.

```javascript
(function() {
  for (let i = 0; i < 10; i++) {
    console.log(i);
  }
  console.log(i)      // ReferenceError i is not defined
})();
```

### const

`const` declaration is similar to `let`, but the value is read-only and cannot be overwritten.

```javascript
const answer = 42;
answer = 54;      // TypeError: Assignment to constant variable.
```

`const` variable must be initialized at writing time.

```javascript
const answer;     // SyntaxError Missing initializer in const declaration
answer = 42;
```

## String interpolation

String interpolation is used to insert variables into strings and make it easier
for string concatenation. We wrap around the string by back tick ` ` and surround
  variables with `${}`.

```javascript
let firstName = 'James';
let lastName = 'Bond';
console.log(`My name is ${lastName}, ${firstName} ${lastName}`);
// My name is Bond, James Bond
```

ES6 string interpolation respects whitespaces so you could use it to
create string templates.

## Default Parameters

Before ES6, this is how we use default parameters in Javascript:

```javascript
function sayHi(first, last) {
  var firstName = first || 'John';
  var lastName = last || 'Doe';
  console.log('Hello ' + firstName + ' ' + lastName);
}
sayHi();  // Hello John Doe
```
Now we can use default parameters like other languages:

```javascript
function sayHi(first = 'John', last = 'Doe') {
  console.log(`Hello ${first} ${last}`);
}

sayHi('Quoc', 'Nguyen');  // Hello Quoc Nguyen
```

## Spread/Rest operator

`...` is a new operator in ES6. `...` can be spread or rest depend on the
context it is used.

### spread operator

Used with an array, it is spread operator because it spread out the array to
individual values.

```javascript
function foo(a, b, c) {
  console.log(a + b + c);
}
foo(...[1,2,3]);  // 6
```

### array concatenation

```javascript
let bar = [2,3,4];
let baz = [1, ...bar, 5];
console.log(baz);  // [1,2,3,4,5]
```

### rest operator

Oppositely, used with a variable, it is rest operator as it collects the rest
individual values into an array.

```javascript
let primes = [2,3,5,7,11];
let [first, ...rest] = primes;

console.log(first)  // 2
console.log(rest)  // [3,5,7,11]
```

### arguments object to array

One cool use of spread operator is to convert `arguments` object to array.

```javascript

// life before spread operator:
(function() {
 var argsArr = Array.prototype.slice.call(arguments);

 var newArr = argsArr.filter(function(e) {
     return e % 2 === 0;
 });
 console.log(newArr);
})(1,2,3,4,5);  // [2,4]

// now use spread operator
(function(...args) {
 let newArr = args.filter(function(e) {
     return e % 2 === 0;
  });
 console.log(newArr);
})(1,2,3,4,5);  // [2,4]

```

## Destructuring assignment

Destructuring assignment allows us to scope variables within an array or object
literal.

### array destructuring

```javascript
let arr = [1,2,3];
let [one, two, three] = arr;
console.log(one, two, three);  // 1 2 3

//skip some elements
let [un] = arr;
console.log(un);  // 1

let [uno,,tres] = arr;
console.log(uno, tres)  //1 3
```

### object destructuring

```javascript
let stuff = {
  camera: 'Nikon',
  watch: 'Seiko',
  glasses: 'Rayban',
  phone: 'LG'
};

let {camera, phone} = stuff;
console.log(camera, phone);  // Nikon LG

// dot notation replacement
function getStuff({camera, watch}) {
  console.log(`You are carrying ${camera} and ${watch} in your backpack`)
}

getStuff(stuff);  // You are carrying Nikon and Seiko in your backpack
```

## `for..of` Loops

Normally, we use `for` loop for array iteration by indices and `for..in` loop for object
iteration by keys. ES6 introduces `for..of` loop, which loops over the set of *values*
of the *iterable*.

Arrays are iterables out of the box, which means we can use `for..of` on arrays.

```javascript
let food = ['vermicelli', 'noodles', 'eggroll'];
for (let val of food) {
  console.log(val);  // 'vermicelli' 'noodles' 'eggroll'
}
```

Objects are not iterables unless we make them to be. (discuss later)

We check if an object is an iterable by checking whether its `Symbol.iterator` function is
defined or not.

```javascript
//objects are not iterables
let obj = {first: 'Jon', last: 'Snow' };
console.log(typeof obj[Symbol.iterator]); // undefined

// arrays are iterables
let houses = ['Stark', 'Tully', 'Lannister', 'Greyjoy', 'Baratheon'];
console.log(typeof houses[Symbol.iterator]);   // 'function'
```


## Arrow function expression

This is probably one of the coolest features in ES6. Arrow functions are
anonymous function expressions. The syntax is very short and concise which makes
it easy for oneliner function expression.

```javascript
let square = x => x*x   // so lit!
console.log(square(5));  // 25
```

### basic syntax

```javascript
(param1, param2, ..., paramN) => { // function body }
// if function body has only 1 statement, return keyword and {} could be omitted.
// if function has only one parameter, () could be omitted.
param => expression
// however, if function has no argument, () must be used.
() => { // function body }
```

### `this` context within arrow function

Arrow functions do not shadow `this`. So we do not have to do something like
`var that = this`.

```javascript
let zodiac = {
  signs: ['cancer', 'capricorn', 'leo', 'scorpio', 'taurus'],
  printSigns: function() {
    var that = this;  // cache this or it will be window obj
    setTimeout(function() {
      console.log(that.signs.join(', ');
    }, 1000);
  }
};

zodiac.printSigns(); // cancer, capricorn, leo, scorpio, taurus

// use arrow function syntax
let zodiac = {
  signs: ['cancer', 'capricorn', 'leo', 'scorpio', 'taurus'],
  printSigns: function() {
    setTimeout(() => {  // this now has proper context
      console.log(this.signs.join(', ');
    }, 1000);
  }
};

zodiac.printSigns(); // cancer, capricorn, leo, scorpio, taurus
```

*Note*: You might be tempted to replace the printSigns function with arrow syntax too.
But it would throw `TypeError` on `this` because `this` in this case is the
global object. How so? Remember that arrow functions inherit `this` from outer
scope. If you define printSigns function by arrow syntax, it would inherit
`this` from its closest outer scope, which is the window object.

