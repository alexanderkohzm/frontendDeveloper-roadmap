# Scope

[https://developer.mozilla.org/en-US/docs/Glossary/Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope) 

[https://dmitripavlutin.com/javascript-scope/](https://dmitripavlutin.com/javascript-scope/)

## What is Scope?

Scope is the current context of execution in which values and expressions are “visible” or can be referenced. 

If a variable or expression is not in the current scope, it will NOT be available for use. 

JavaScript has three scopes:

- ****************************Global Scope:**************************** The default scope of all code running in script mode
- ************************Module Scope:************************ Scope for code running in module mode
- ********************************Function Scope:******************************** Scope for code running in a function

There is an additional scope - **********************Block Scope**********************, where variables cannot be referenced when it’s inside curly braces `{}` 

******************Examples:******************

```jsx
function exampleFunction1() {
	const name = 'John'
	console.log(`hello ${name}`)
}

exampleFunction1() // 'hello John'
console.log(name) // throws an error

function exampleFunction2() {
	var name2 = 'Smith'
	console.log(`hello ${name2}`)
}

exampleFunction2() // 'Hello Smith'
console.log(name2) // 'Smith'
```

## Block Scope

****************Important:**************** `var` is NOT block scoped 

```jsx
if (true) {
	const hello = 'hello'
	console.log(hello) // 'hello'
}
console.log(hello) // reference error

for (const color of ['green', 'red', 'blue']) {
	const message = 'hi' 
	console.log(color, message) // 'green hi', 'red hi'
}
console.log(message) // reference error
console.log(color) // reference error
```

## Lexical Scope

Example:

```jsx
function outerFunction() {
	
	const outerVariable = 'I am outside'
	
	function innerFunction() {
		console.log(outerVariable) // I am outside
	}
	return innerFunc
}

const callFunction = outFunction();
callFunction()
```

JavaScript implements a scoping mechanism named lexical scoping (or static scoping)

Essentially, JavaScript determines the accessibility of variables depending on its position within functions. 

In this case, `innerFunction` has access to `outerVariable` 

<aside>
💡 ******************Closure:****************** Function that accesses variables in its lexical scope (i.e. a function that accesses a variable in a parent scope)

</aside>

## Variable Isolation

Scoping isolates the variable - this means that you can declare the same named variable in different scopes

```jsx
function foo() {
	const count = 0
	console.log(count) // 0 
}

function bar() {
	const count = 200
	console.log(count) // 200
}
```