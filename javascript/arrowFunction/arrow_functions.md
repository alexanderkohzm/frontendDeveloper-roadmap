# Arrow Functions

https://medium.com/@mahendrjy/es6-arrow-functions-syntax-and-lexical-scoping-d061732071e7

It’s important to remember that the arrow function ********************************************\*\*********************************************is not a replacement for the function keyword********************************************\*\*********************************************

### Syntax

```jsx

// Arrow Functions
(param1, param2) => {
	// statement
}

param => expression

// ES5 equivalent functions
function(param1, param2){
	// statement
}

function (param) {
	return expression
}
```

In ES6, we can assign an anonymous function to a variable

```jsx
const functionToCall = (param) => {
  // statement
};
```

### Using Arrow Functions with Map

```jsx
// Previous way

const numbers = [1, 2, 3, 4, 5];

const timesTwo = numbers.map(function (number) {
  return number * 2;
});

console.log(timesTwo); // [2, 4, 6, 8, 10]

// Arrow Function way

const timesTwoArrowFunction = numbers.map((number) => number * 2);

console.log(timesTwoArrowFunction); // [4, 8, 12, 16, 20]
```

Arrow functions also provide some syntax sugar by allowing us to omit the parenthesis around `number` if there is only one parameter

For example:

```jsx
const timesTwoArrowFunctionNoParanthesis = numbers.map((number) => number * 2);
```

However, this can be problematic when people forget syntaxes or if more junior developers don’t know how to add the parenthesis back for functions that require more than one parameter.

### Functionality: Lexical Scoping `this`

The benefits of arrow functions does not simply lie in syntax sugar.

It provides a convenient way to access the `this` keyword of the scope in which the arrow function has been called in.

Typically, in ES5, you’ll need to use `function.prototype.bind` to grab the value from another scope

```jsx
// This will fail

function FooControl(FooService) {
  this.foo = "hello";
  FooService.doSomething(function (response) {
    this.foo = response;
  });
}

// this will work
function FooControl(FooService) {
  this.foo = "hello";
  FooService.doSomething(
    function (response) {
      this.food = reponse;
    }.bind(this)
  );
}

// Or instead of using bind, we could
// assign this to a variable
function FooControl(FooService) {
  var context = this;
  context.foo = "hello";
  FooService.doSomething(function (response) {
    that.foo = response;
  });
}
```

Arrow functions allow us to inherit the scope we’re in. This means we don’t need to bind `this` or assign `this` to a variable

```jsx
function FooControl(FooService) {
  this.foo = "hello";
  FooService.doSomething((response) => {
    this.foo = response;
  });
}
```

Which can be refactored into a single line expression

```jsx
function FooControl(FooService) {
  this.foo = "hello";
  FooService.doSomething((response) => (this.foo = response));
}
```

The `this` value (internally) is not actually bound to the arrow function. Normal functions in JavaScript bind their own `this` value however the `this` value used in arrow functions is actually fetched lexically from the scope it sits inside. It has no `this`, so when you use `this` it automatically means you’re talking to the outer scope.
