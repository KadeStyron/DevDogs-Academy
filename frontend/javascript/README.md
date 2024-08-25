# JavaScript in a Nutshell
JavaScript (JS for short) is a dynamically typed, interpreted, multi-paradigm language. It also happens to be the *lingua franca* of the web. All the major browsers can run it to make the webpages they display interactive, and you can even use [some tooling](https://en.wikipedia.org/wiki/Node.js) to run it on servers for back-end services. We'll be using JS extensively in this club to create our front-ends, so it'll serve you well to get familiar with it.

This article is meant to help you hit the ground running quickly. If you're here to review something specific, or you already feel pretty good about your JavaScript abilities, please feel free to aggressively utilize the table of contents (located in the top right corner) to skip to the parts you care about.

If you feel like messing around with the examples given in this article, you can open up a live JavaScript interpreter in your browser to see how things play out.  On Chrome, this can be done with Ctrl+Shift+J (Cmd+Opt+J for macOS). On Firefox, hit Ctrl+Shift+K (Cmd+Opt+K for macOS). Type things in at the bottom of the panel that pops up to run your JS code.

Finally, this is a living article. It probably contains some mistakes and certainly omits some details and edge cases that have been deemed unimportant for beginners. If you've spotted something that you feel should be changed, please don't hesitate to [open an issue](https://github.com/DevDogs-UGA/DevDogs-Academy/issues) or [a pull request](https://github.com/DevDogs-UGA/DevDogs-Academy/pulls) and we'll try to work it out.

That's it for introductions. Let's get in to it!

## Language overview
JavaScript's syntax is generally C-flavored, so if you have experience in languages like C, C++, C#, or Java, you'll feel right at home. That being said, there are some important differences that you should be aware of before you start writing code. Read on!

### Comments
Comments are portions of your source code that the interpreter won't run. They're typically used for documentation and other things that are intended for developers to read (not the computer). Single line comments start with two slashes (`//`) and run until the end of a line. Multi-line comments start with a slash-star (`/*`) and run until the next star-slash (`*/`).
```js
"something here will be run" // but this won't!
/*
multi-line comments are nice because they let you
break
things
up
*/
```

### Semicolons
Like most other C-family languages, JavaScript terminates statements with a semicolon (`;`). Sometimes, you're able to omit these semicolons in your source code and let the interpreter guess about where to put them. The catch is that the interpreter sometimes won't have enough context to guess correctly, leaving you with some really confusing errors. In this club, we've decided that it's in our best interest to explicitly include semicolons 100% of the time. Please do your best to follow this standard!
```js
// semicolon included
// ------------v
"this is valid";
// semicolon omitted
// -----------------------------------v
"this is also valid (but discouraged)"
```

### Primitives and common operations
JavaScript's most basic units of information are categorized into a few primitives[^other-primitives]: `boolean`, `number`, `string`, `undefined`, and `null`.

Booleans are values of `true` or `false`. They're used extensively in conditional statements, which we'll cover later.
```js
// true!!!
true
// false...
false
// logical or, yields true
true || false
// logical and, yields false
true && false
// logical not, yields false
!true
// use parenthesis to enforce an order of operations
// this yields true
(true || false) || (true && false)
```

Numbers are stored as double-precision floating point numbers (think `double` in your C-style language of choice). They look like:
```js
// plain integers yield their value
1
// you can add a point without anything after it
2.
// or with something
3.0
// scientific notation works - this translates to 400 * 10^(-2)
400e-2
// numbers prefixed with 0x are interpreted as base-16 (hexadecimal)
0x5
// a 0b prefix denotes a base-2 literal (binary)
0b110
// you can separate digits with an underscore (_) for readability
// this yields 1000000 (one million)
1_000_000
// addition, yields 3
1 + 2
// subtraction, yields -1
1 - 2
// multiplication, yields 2
1 * 2
// division, yields 0.5
1 / 2
// modulo, yields 1
1 % 2
// exponentiation (think a^b), yields 1
1 ** 2
```
Please keep in the back of your mind that these are floats, and come with all the usual float weirdness (imprecision, non-associativity, signed zeroes, infinities, NaNs). You should almost never have to worry about these things, but it's good to know that they exist just in case you ever do.

Strings look like this:
```js
// double quotes
"this is a string"
// single quotes
'this is also a string'
// backticks (that thing to the left of your 1 key)
`this is also also a string`
// you can use a backslash (\) to break long strings into multiple lines
// this yields "this string spans multiple lines!" (no new line)
"this string spans \
multiple lines!"
// you can use the usual escape sequences
"newline: \n, tab: \t, carriage return: \r, null: \0, etc"
// strings are arrays of individual characters. you can access these characters
// with a subscript ([]). we'll talk more about arrays later!
// this yields the string '3'
"012345"[3]
// + appends strings together. this yields "strings are nice".
"strings " + "are nice"
// other types are eagerly converted into strings when appended
// this yields "this should say false: false"
"this should say false: " + (false || (false && true))
// backtick strings allow you to inject any js expression into a string with
// `${expr}`
`this should say false: ${false || (false && true)}`
```

Undefined and null values are kind of special. Undefined is used to indicate that an identifier hasn't been given an explicit value. You'll see undefined values if you try to do something like use a variable that hasn't been assigned to. Null values, on the other hand, are used to indicate an intentional value of 'nothing'. Most of the time, you shouldn't go out of your way to use undefined. Null is maybe a little bit more common.
```js
undefined
null
// operations with null and undefined are weird. you really shouldn't ever have
// to think about them, but here are some quick examples just for fun:
// this yields NaN (the floating point value)
true + undefined
// this yields the number 1
true + null
// the typeof operator should yield a string containing a value's type (e.g.
// "string", "boolean", etc), but this yields "object" instead of "null" (more
// on objects later)
typeof null
```

[^other-primitives]: Okay, technically there's also [`bigint`](https://developer.mozilla.org/en-US/docs/Glossary/BigInt) and [`symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol), but most readers probably don't need to worry about those.

### Variables
You may from time to time want to store values for later use. You can do this using variables. Variables are declared using `const` and `let`.[^other-decls]
```js
// declares a new variable `variable0` with initial value `undefined`
let variable0;
// variables can be reassigned
variable0 = "hi";
// js is dynamically typed, so variables can also be reassigned to values with
// different types
variable0 = 0;
// const variables must be initialized at declaration and cannot be reassigned
// after declaration
// const variable1; <- not allowed!
const variable1 = 1;
// variable1 = 2; <- not allowed!

// variables are scoped to the block they're in. this means that they're only
// accessible inside the curly braces ({}) they were declared.
let outerScope = "outer!";
// outerScope now accessible
{
    // outerScope still accessible
    let innerScope = "inner!";
    // innerScope now accessible
}
// innerScope no longer accessible
// outerScope still accessible
```

Sometimes you'll want to do an operation on a variable's value and then store the result back into the variable. JavaScript has a shorthand for this:
```js
let foo = 100;
// instead of writing something like this
foo = foo + 1;
// you can instead write this
foo += 1;
```
You use this operation-then-assign syntax with any of the operations mentioned in the previous section.

There is another, even more terse way to do an operation-then-assign for the special cases of incrementing and decrementing by one. If you're familiar with increment and decrement from other C languages, they work pretty much the same in JS. You can use them postfix or prefix, and they act as expressions (i.e. they yield a value). These operators can be hard to read and reason with if used in overly creative ways though, so please be kind to your collaborators and give some thought to maintainability before using them.
```js
let baz = 0;

baz++;
baz === 1;

baz = 0;

baz--;
baz === -1;
```

[^other-decls]: There are also no-keyword declarations (which are always global-scope) and `var` declarations (which are function-scope). These are included in the language for legacy reasons, and generally should not be used today.

### Comparisons
Comparisons let you check whether data structures satisfy some sort of condition.

```js
// double-equals (==) compares values. this yields true:
0 == 0
// but beware! the interpreter will try and convert types to allow for
// comparison. this can lead to unexpected behavior like this (comparing string
// and number), which yields true
"0" == 0

// if you want to do an equals comparison while respecting type differences,
// you'd use a triple-equals (===) comparison.
0 === 0 // true
"0" === 0 // false

// a similar distinction exists for the not-equals comparisons. the following
// are equivalent, and yield false
!(0 == 0)
0 != "0"
// the following are equivalent, and yield true
!(0 === "0")
0 !== "0"

// finally, we have the ordering comparisons. these work as you would expect in
// any other language, with the caveat that types are always converted (like
// with ==)
0 > 0 // false
0 >= "0" // true
0 < "0" // false
0 <= 0 // true
```

JavaScript also has something called 'truthy' and 'falsy' values. Basically, all non-Boolean values will behave like `true` or `false` if you try to use them as if they were Boolean. The rule is pretty simple - all values behave like `true` except for the following, which behave like `false`:
- `0`
- `NaN` (a floating point not-a-number value)
- `""` (the empty string)
- `null`
- `undefined`
```js
// we'll use bang-bang (!!) to force a truthy/falsy value to turn into a
// boolean. it really just does a logical not (the first !) and then nots that
// again (the second !) to get the original boolean value.
// the following all yield `true`
false === !!0
false === !!""
false === !!null
true === !!1
true === !!"true"
true === !!"false"
```

### Conditionals
Conditionals let you make decisions about whether or not a bit of code should run. They come in the form of a Boolean condition to test and a block to execute (or not, depending on the condition).
```js
if (true) {
    // the condition is true, this will run
}

if (false) {
    // the condition is false, this won't run
}

// else statements are run when the previous if condition was false
if (false) {
    // won't run
} else {
    // will run
}

// you can omit the curly braces ({}) if you only use one statement
if (false)
    "won't run";
else
    "will run";

// omitting braces is usually bad style, but it's useful when chaining
// conditionals together. remember, conditionals are themselves a statement,
// so you can do something like this (called an else-if)
if (false) {
    // won't run
} else if (true) {
    // will run
}

// that was a brace-less else statement with an if as its single
// statement. cool, huh? you can then chain off of that single
// if statement with another else, and so on
if (false) {
    // won't run
} else if (true) {
    // will run
} else {
    // won't run (last condition was true)
}

// recall our previous overviews of logical operators and truthy values -
// they both apply here. the condition just has to be a boolean!
if (true && false) {
    // won't run
}

if ("foo") {
    // will run (nonempty strings are truthy)
}
```

JavaScript gives you a shorthand for simple conditionals called the ternary expression. They're broken up into three parts: a condition, a value to yield if the condition is true, and a value to yield if the condition is false.
```js
// ternaries look like this:
// condition ? leftHand : rightHand

let ifTrue = "left hand side!";
let ifFalse = "right hand side!";
// the condition is true, so the ternary yields the value on the left side of
// the colon - ifTrue, which is "left hand side!"
true ? ifTrue : ifFalse;
// now the condition is false, so the ternary yields the value on the right side
// of the colon - ifFalse, which is "right hand side!"
false ? ifTrue : ifFalse;

// we can use this to compress basic conditionals down to a single line
let condition = "some condition";
let message;

if (condition) {
    message = "nice, condition is true";
} else {
    message = "drats, condition is false";
}
message === "nice, condition is true";

// you could bring that down to something like this using a ternary
message = condition ? "nice, condition is true" : "drats, condition is false";
message === "nice, condition is true";
```

### Arrays
Arrays let you bundle multiple values into one data structure.
```js
// array literals are delimited by square brackets ([])
[0, 1, 2, 3]
// elements don't have to be the same type
let arr = ["value zero", "value one", 2, false];
// elements are accessed using the subscript ([]) operator
// arrays are zero-indexed, meaning they start counting their elements at zero
// yields "value zero"
arr[0]:
// yields "value one"
arr[1];
// yields 2
arr[2];
// you can change the contents at an index by reassigning with =
// after this next line, arr will look like ["value zero", "value one", 2, 3]
arr[3] = 3;
// you can even store other arrays inside of an array!
let nested = [["array 0 element 0"], ["array 1 element 0"], ["array 2 element 0"]];
// you access these values using chained subscripts. the first subscript yields
// the element stored at that index (which is another array) and the second one
// subscripts into that yielded array. this yields "array 1 element 0".
nested[1][0];
// you can get an array's length using the `.length` property (more on
// properties in the classes section). this yields 4.
arr.length;
// you can append a new element to the end of an array with the Array.push
// method (more on methods later)
arr.push(100);
```

### Loops
Loops let you repeat a block of code while a condition is true. They come in three primary varieties: `while`, `for`, and `for-of`.
```js
// while loops continue so long as the condition in their parenthesis (())
// yields true
let howMany = 0;
let message = "We looped ";
while (howMany < 5) {
    message += howMany + " ";
    howMany += 1;
}
message += "times";
message === "We looped 0 1 2 3 4 times";
```

For loops let you declare a variable scoped to the loop, usually for iteration.
```js
// for loops generally look like `for (initialization; condition; step)`.
message = "We looped ";
for (let howMuch = 0; howMuch < 5; howMuch += 1) {
    message += howMuch + " ";
}
message += "times";
message === "We looped 0 1 2 3 4 times";

// for loops can be thought of as just a while loop in a trench coat. the
// previous example could have instead been written equivalently as:
{
    let howMuch = 0;
    while (howMuch < 5) {
        message += howMuch + " ";
        howMuch += 1;
    }
    message += "times";
}
```

For-of loops are analogous to for-each loops in other languages. They provide a nicer way of looping through collections of things.
```js
message = "We've seen elements ";
let array = [0, 1, 2, 3, 4, 5];
// trying to change the variable you bind each element to will just change
// the variable, not the array, so we usually const-declare them for clarity
for (const element of array) {
    message += element + " "
}
message === "We've seen elements 0 1 2 3 4 5 "
```

### Functions
Functions let you bundle chunks of code into self-contained, parameterized units. They're essential to avoiding repetition and improving readability of code. Parameters are passed by value but referencing through that value may still make changes that are visible outside of a function. This is the same way Java works.
```js
// functions are declared using the function keyword.
function sayNi() {
    // ni!
}
// you call a function by using its name followed by parenthesis (())
sayNi();
```

Functions can also take paramters, values which are passed when the function is called and can be used inside the function's body. These parameters are 'pass by value' with similar rules to Java.
```js
// parameters are declared alongside a function inside its parenthesis. these
// parameters are untyped, like any other variable.
function useNum(num) {
    // you can use num like a variable in here
    num - 1;
    num += 1;
    num = 15;
}
// pass parameters when you call the function. here, num gets the value 1
// inside of useNum's body
useNum(1);
// if you omit a parameter when calling a function, its value is `undefined`
// inside the function. e.g., when we do this, num gets the value `undefined`
// in useNum's body
useNum();
// we can get around this with default parameters, which give a parameter a
// default value if it isn't passed (or is passed as `undefined`)
function useNumDefault(num = 0) {
    // ...
}
// default parameters act like any other parameter when passed
// here, num = 1 in the function body
useNumDefault(1);
// but when we don't pass a value, num gets the default value (in this case, 0)
useNumDefault();
useNumDefault(undefined);
// you can also use three dots (...) to accept a variable number of parameters
// (often called varargs). this must come at the end of the parameters list.
function useVariable(firstArg, ...restArgs) {
    // restArgs is a collection - you can pretty much treat it like an array
    restArgs[0];
    restArgs[1];
    restArgs[2];
    // etc
}
// here, firstArg is 0, restArgs[0] is 1, restArgs[1] is 2, etc
useVariable(0, 1, 2, 3);
```

Return statements let functions yield a value to the place where they were called.
```js
function returnOne() {
    let varToReturn = 1;
    varToReturn += 1;
    // return statement: this function yields
    // this computed value
    return varToReturn - 1;
}
// the value returned by a function is "slotted in" to the place where the
// function was called
// e.g. this is true, it looks like 1 === 1 to the interpreter since the
// returnOne function returned 1
returnOne() === 1;
```

There are also some other ways to declare functions, mainly unnamed functions and lambdas.
```js
// if the interpreter has enough information to name a function itself, you can
// omit the name from the initial declaration and later refer to it by the
// context-assigned name
let namedFunction = function() {
    // I'm named namedFunction
}
namedFunction();
let functionArray = [
    function() { /* I'm named functionArray[0] */ },
    function() { /* I'm named functionArray[1] */ },
];
functionArray[0]();
functionArray[1]();

// you can use arrow syntax functions (or 'lambdas') as more concise notation
let otherNamedFunction = () => { /* I'm named otherNamedFunction */ };
otherNamedFunction();
// lambdas can do anything functions can, including taking parameters and
// returning values
let lambdaSuccessor = (number) => { return number + 1; };
lambdaSuccessor(1) === 2;
// if a lambda is able to return in one expression, you can omit the braces and
// return statement as a shorthand. you can also leave out the parenthesis if
// the lambda only has one parameter. this function is equivalent to the last:
lambdaSuccessor = number => number + 1;
lambdaSuccessor(1) === 2;
```

### Objects
Objects let you bundle a bunch of named values into one unit.
```js
// object literals are delimited by curly braces ({})
let emptyObject = {};
// you define properties on an object by providing a name and a value
let greetingObject = {
    // properties define values on an object
    greeting: "Hello",
    // nested objects are valid
    nestedObject: {
        nestedGreeting: "Hello from the inside",
    },
    // they can store functions to emulate methods
    greet: function(name) {
        // access the current object's properties with the `this` keyword
        return `${this.greeting} ${user}.`;
    },
    // here's another way to define a method
    greetExcited(name) {
        return `${this.greeting} ${user}!`;
    },
    // getters and setters have special syntax that make them a bit nicer to use
    getSetProperty: 0,
    get getProperty() {
        return this.getSetProperty;
    },
    set setProperty(value) {
        this.getSetProperty = value;
    },
}
// properties are then accessed by writing the object's name, a dot (.), then
// the property's name. this is called dot notation.
greetingObject.greeting === "Hello";
greetingObject.nestedObject.nestedGreeting === "Hello from the inside";
greetingObject.greet("Alice") === "Hello Alice.";
// get and set methods can be accessed as if they were properties
greetingObject.getProperty === 0;
greetingObject.setProperty === 1;
greetingObject.getProperty === 1;
// you can modify an object outside of its declaration to change, remove, or add
// properties
greetingObject.greeting = "Goodbye";
greetingObject.greetExcited = undefined;
greetingObject.greetConfused = (name) => `${greetingObject.greeting} ${user}?`;
greetingObject.greetConfused("Bob") === "Goodbye Bob?";
```

### Classes
Classes let us prescribe properties and methods to an object. Think of it like a blueprint: any object that follows the blueprint provided by the class is considered an instance of that class.
```js
class Animal {
    // properties are declared with just a name
    name;
    // you can give properties a default value
    laysEggs = false;
    // static properties are shared across all instances of the class and are
    // referred to using the class name
    static defaultName = "Animal";
    // private properties start with a hash (#) and cannot be accessed outside
    // of the class
    #message;

    // methods are also defined with just a name
    sayMessage() {
        return `${name !== undefined ? this.name : Animal.defaultName} says: ${this.#message}`;
    }

    // private methods also start with a hash (#). they can also be static.
    #getNameOrDefault() {
        return name !== undefined ? this.name : Animal.defaultName;
    }

    // constructors give you a way of creating an object from the class
    // blueprint. the constructor method (there can only be one) must be named
    // `constructor`, and can take arguments like any other function.
    constructor(name, animalMessage, canSwim) {
        this.name = name;
        this.#message = animalMessage;
        // you can also define properties inside the constructor using `this`
        this.canSwim = canSwim;
    }
}
```

Using classes is pretty similar to other object-oriented languages.
```js
// you use the constructor to make a new object using the new keyword and the
// class name
let whale = new Animal("Whale", "Wooooo...", true);
// these objects are just like any other objects, except they have the
// properties we defined earlier
whale.name === "Whale";
whale.canSwim === true;
// whale.#message === "Wooooo..."; <- not allowed, #message is private
whale.sayMessage() === "Whale says: Wooooo...";
```

Classes can also extend other classes to represent a more specific type of blueprint.
```js
class Platypus extends Animal {
    // subclasses can add their own methods and properties
    layEggs() {
        // TODO lay eggs
    }

    constructor() {
        // you must call the parent class's constructor with the super keyword
        super("Platypus", "Chatter", true);
        // you can then access a parent class's properties and methods
        this.laysEggs = true;
        // not private ones though, those are still inaccessible
        // this.#message = "Growl"; <- not allowed
    }
}

let perry = new Platypus();
// you can use parent class properties and methods on the subclass object
perry.canSwim === true;
perry.laysEggs === true;
perry.sayMessage() === "Platypus says: Chatter"
// and also use the new ones defined by the subclass
perry.layEggs();
```

## Interacting with the DOM
An important part of using JavaScript is changing and responding to changes in the DOM. The DOM, short for **D**ocument **O**bject **M**odel, is the browser's internal data structure for the contents of a page. Web browsers expose a number of JavaScript objects and functions to let you interact with this model. Understanding this section requires a basic understanding of HTML and CSS. Check out [our article on HTML](https://github.com/DevDogs-UGA/DevDogs-Academy/tree/main/frontend/html) or [on CSS](https://github.com/DevDogs-UGA/DevDogs-Academy/tree/main/frontend/css) if you need a quick intro (or refresher!).
```js
// the `document` object has methods which let you interact with the DOM.
// often, the first thing you'll want to do is select an element. here are a
// few different ways to do this:
// get a single element by its ID
let uniqueElement = document.getElementById("unique");
// get a collection of elements that have a given tag name
let paragraphElements = document.getElementsByTagName("p");
// get a collection of elements that have a given class
let usernameElements = document.getElementsByClassName("username");
// get a collection of elements that match a CSS selector
let nestedParagraphElements = document.querySelectorAll("p p");

// you can essentially treat the 'collections' mentioned above like arrays
let firstParagraphElement = paragraphElements[0];

// the value returned by these selectors can be queried again to get more
// specific elements, if desired
let paragraphInUniqueElement = uniqueElement.querySelectorAll("p")[0];
```
Once you have a value that holds a selected element, you can bind to their events. Events are flags that get raised when something happens to an element. They happen when elements are clicked, moused over, dragged, loaded, etc. Binding to an event just means running some code when the event gets triggered.
```js
let button = document.getElementById("button-id");
// say we want to show a pop-up alert when someone clicks on the button
// we can bind to the click event using the `addEventListener` method
button.addEventListener("click", () => alert("You clicked the button!"));
// now whenever someone clicks the button, the click event is raised and
// our alert lambda is called.
```

## Promises and async
With JavaScript being the language of the web, you're bound to use it to request data over the internet pretty often. Let's look at a naive way of making such requests.[^cors]
```js
// XMLHttpRequest objects are one way to make HTTP requests
let x = new XMLHttpRequest();
// we want to make a GET request to the url 'https://example.com'. the false
// parameter tells the method to make the request synchronously - more on this
// later.
x.open("GET", "https://example.com", false);
// send the request to the server
x.send();
// success case: we got 200 OK from the server
if (x.status === 200) {
    // the responseText property contains the server's response
    let response = x.responseText;
    // print it to console with console.log
    console.log(response);
} else {
    // failure case: request failed
    console.log("Request failed!");
}
```
At first glance, this looks pretty nice. We get everything we need in only a few lines of code. Unfortunately there's a catch: making an HTTP request like this will block the main thread. This means that as soon as the program hits the `x.send()` line, everything freezes until the server responds. Depending on the user's internet speed and the status of the server, this could take anywhere from a few milliseconds to a few minutes. That's bad! Imagine you have a button on a page that does something when you click on it.
```js
// get the button element
let button = document.getElementById("button");
// bind a lambda to the click event
button.addEventListener("click", _ => {
    // let the user know they clicked the button
    alert("You clicked the button!");
});
```
Now imagine that elsewhere, you have to make a request to a server that takes a long time to respond. While this request is being made, nothing will happen when the user clicks on the button. This because the interpreter is being blocked by the long-running network request. The solution to this problem is something called a promise. Promises let you yield control back to the interpreter while you're waiting for a resource to load.
```js
let httpPromise = new Promise(function(success, failure) {
    // the success parameter is a function we need to call if things succeed
    // the failure parameter is a function we need to call if things fail

    let x = new XMLHttpRequest();
    x.open("GET", "https://example.com", false);
    x.send();
    if (x.status === 200) {
        success(x.responseText):
    } else {
        failure();
    }
});

// we can access the promise's return values with a call to the method `then()`
let usePromise = function(promiseText) {
    console.log("We got this from the promise:\n" + promiseText);
};
let handleFailure = function() {
    console.log("Promise failed!");
};
httpPromise.then(usePromise, handleFailure);
// if you know that the promise always succeeds, you can omit the failure function
let successfulPromise = new Promise(success => success("Success!"));
successfulPromise.then(usePromise);
```
Most things that deal with running IO-bounded, potentially slow code will return a promise. However, promises can be a bit cumbersome to write manually every single time. Thankfully, there are shortcuts for making functions that return promises and waiting on promises, called `async` and `await` (respectively). Async functions automatically turn a function's body into a promise. Awaiting a promise blocks execution until the promise resolves - i.e. succeeds or fails.
```js
function slowHello(name) {
    // the built-in setTimeout function waits a certain number of milliseconds
    // before calling a function. here we just do nothing 5 seconds (5000
    // milliseconds) and then return our greeting
    setTimeout(() => {}, 5000);
    return `Hello ${name}!`
}

// manually-made promise
let helloBobPromise = new Promise(success => {
    let bobGreeting = slowHello("Bob");
    success(bobGreeting);
});
// you can use await on a promise to immediately block execution until the
// promise resolves. this is usually used when you reach a portion of your code
// where you absolutely need a value in order to continue
let result = await helloBobPromise;

// you can alternatively define an equivalent async function.
// these functions get turned into a promise when they're called
async function helloBobAsync() {
    return slowHello("Bob");
}

// you can do promise things with the function's return value
helloBobAsync().then(greeting => console.log(greeting));
console.log(await helloBobAsync());
```
You can use async functions anywhere, but `await` only works in either the global scope of your program or in an async function. This is to avoid completely removing the benefits of promises - you still want to yield control when possible while using `await`.

[^cors]: We'll be ignoring [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) here for illustrative purposes, but in the real world you'll have to keep it in mind.

## Serialization with JSON
Before you store values to disk or send them over the network, you need some standard way of representing them. If one computer stores the value `0` as a 32-bit binary value and another one expects it to be stored as a string, things can get messy real fast when one tries to communicate with the other. This is where JSON comes in. JSON, which stands for **J**ava**S**cript **O**bject **N**otation, borrows the lions share of JS's type syntax and uses that to represent arbitrary data. JSON supports booleans, numbers, strings, arrays, objects, and `null` in roughly the same way that we talked about them earlier.

```js
// let's make some example values to encode
let objectToEncode = {
    message: "This incident will be reported.",
};

let arrayToEncode = [false, true, false, true, false, true];

// JSON.stringify turns a value into a JSON string
let objectStringValue = JSON.stringify(objectToEncode);
// curly braces ({}) denote objects, property names are put in double quotes (")
objectStringValue === '{"message":"This incident will be reported."}';

let arrayStringValue = JSON.stringify(arrayToEncode);
// square brackets ([]) denote arrays, elements are separated by commas (,)
arrayStringValue === "[false,true,false,true,false,true]";

// JSON.parse goes the other way, turning a JSON string into its JS value.
// whitespace is ignored, so you can make a JSON string as pretty as you want.
let parsed = JSON.parse(`{
    "sillyProperty": "wow this property is so silly",
    "absentProperty": null,
    "arrayProperty": [0]
}`);
// you can now use the parsed value as if it was declared in your code
parsed.sillyProperty === "wow this property is so silly";
parsed.absentProperty === null;
parsed.arrayProperty[0] === 0;
```

## Modules
As projects get larger, it becomes increasingly important to organize your code properly. JavaScript's solution to this is the module - a file-based bundle of code that exposes some (or all) of its variables, functions, and classes for other files to use. Making things available from a file is called `export`ing, while using things from another file is called `import`ing. If you have experience with C or C++, a module's exports are conceptually similar to a header file.
```js
// let's cook up a module that makes a cake for you (in javascript).
// consider the following to be in a file called 'cake.js':
function gatherIngredients() {
    return "Gathered ingredients.";
}

function mixIngredients() {
    return "Ingredients were mixed.";
}

function bakeMix() {
    return "Mix has been baked.";
}

// let's export a counter variable that keeps track of how many cakes we've
// made. exported variables are static to the module - that is, if two
// files A and B import this counter and A changes its value, B will see those
// changes.
export let cakesMade = 0;

// now let's make a function that does all the steps to bake a cake
export function makeACake() {
    // exported functions can use functions that aren't exported
    gatherIngredients();
    mixIngredients();
    bakeMix();
    cakesMade += 1;
    return "Cake made successfully!";
}

// say that we have another file, 'bakery.js', that wants to use the exports
// from 'cake.js'. our file tree now looks like this (with both files in the
// same directory):
// .
// +-- cake.js
// \-- bakery.js

// to import from cake.js, we list what we want to import in curly braces ({})
// and say where they live. the module name (that "./cake.js" bit) is a path
// relative to the current file (in this case, 'bakery.js').
import { cakesMade, makeACake } from "./cake.js"

// after importing, we can use the values just like if they were declared in
// this file
let cakeMessage = makeACake();
console.log(cakeMessage);
console.log(`Here's how many cakes we've made so far: ${cakesMade}`);
```
There are a few other ways to import and export things from modules. Consider checking out [MDN's articles on `import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) and [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) if you're hungry for more module-based content.

## Destructuring
JavaScript provides a very nice way to look inside composite data types called destructuring. Destructuring lets us unpack the contents of things like arrays, objects, and other collections into multiple named variables.
```js
// say we have an array like this
let myArray = ["foo", "bar", "baz"];
// we can peek into the contents of the array with a destructuring assignment
let [myFoo, myBar, myBaz] = myArray;
// myArray hasn't been changed at all, but now we can use myFoo instead of
// myArray[0], myBar instead of myArray[1], etc
myFoo === myArray[0];
myBar === myArray[1];
myBaz === myArray[2];

// you can also do this with objects. the unpacked variable names have to match
// the object property names, though.
let myObj = {
    message: "bingle",
    question: "what?",
};
let { message, question } = myObj;
message === "bingle";
question === "what?";
```

You can use collection destructuring in a number of places, but we mostly only care about using it in declarations (which we just saw), for-of loops, and function parameters. Here's how we destructure with function parameters.
```js
// say we have an array with some values in it
let array = [1, 2, 3, 4];
// and a function which uses some of these values
function addFirstTwo(one, two) {
    return one + two;
}
addFirstTwo(array[0], array[1]) === 3;

// we could instead use a function that destructures the array to get its first
// two elements
function addFirstTwoUnpack([ one, two ]) {
    return one + two;
}
addFirstTwoUnpack(array) === 3;

// this also works with objects
greetWithName(name, greeting) {
    return greeting + " " + name;
}
greetWithName("Alex", "Hi") === "Hi Alex";

function greetWithNameUnpack({ name, greeting }) {
    return greeting + " " + name;
}
greetWithNameUnpack({
    name: "Alex",
    greeting: "Hi",
}); === "Hi Alex";
```

And for for-of loops, we do something like this.
```js
let matrix = [
    [1, 0]
    [0, 1]
];

// this loop prints out:
// (1, 0)
// (0, 1)
for (const [firstCol, secondCol] of matrix) {
    console.log(`(${firstCol}, ${secondCol})`)
}
```

## Functional-style array manipulation
Working with arrays is a super common part of many programs. Given their ubiquity, developers have come up with some very nice ways to interact with arrays beyond your standard for loop. These take the form of higher order methods, methods that take in other functions as parameters. The most important ones for our purposes are `filter`, and `map`.[^other-array-funcs]
```js
// say we have a class that stores information about a person
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = name;
  }
}

// let's make an array of people
let people = [
    new Person("Alice", 20),
    new Person("Bob", 36),
    new Person("Harry", 44),
    new Person("Sue", 16),
    new Person("Alex", 10),
];

// now, imagine that we wanted to get all of the people who are able to vote.
// a naive approach might be to make an array with a loop and a conditional.
let votingPeople = [];
for (const person of people) {
    if (person.age >= 18) {
        votingPeople.push(person);
    }
}

// this works fine enough, but is a little long-winded. the Array.filter method
// lets us instead provide a 'checker' function which says either 'yes' (true)
// or 'no' (false) to each element of the array. the elements we say 'yes' to
// are added into the new array while the ones we say 'no' to are omitted.
let filterVotingPeople = people.filter(person => person >= 18);
// that did the same thing as that whole previous block! much nicer, huh?
```

Mapping lets us use an element's value to construct a new element.
```js
// assume we're using the same Person class from before
let people = [
    new Person("Alice", 20),
    new Person("Bob", 36),
    new Person("Harry", 44),
    new Person("Sue", 16),
    new Person("Alex", 10),
];

// now imagine that we only want to extract just the names from the array.
// instead of doing this
let peopleNames = [];
for (const person of people) {
    peopleName.push(person.name);
}

// we can instead do this to get the same result
let mapPeopleNames = people.map(person => person.name);
```

[^other-array-funcs]: There are several other very nice functional array methods out there, but these are the most important ones. See [`Array.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce), [`Array.every()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every), and [`Array.some()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some) for more.

## More resources
- https://learnxinyminutes.com/docs/javascript/
- https://developer.mozilla.org/en-US/docs/Web/JavaScript
- https://www.destroyallsoftware.com/talks/wat

## Meta
This article's content is available under the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) license. It's also one out of a number of other tutorials/quickstarts that you may also find useful, [available here](https://github.com/DevDogs-UGA/DevDogs-Academy).
