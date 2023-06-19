# Explain how ‘this’ works in JavaScript

Arnav Aggarwal - The Simple Rules to `this` in JavaScript 

[https://medium.com/codeburst/the-simple-rules-to-this-in-javascript-35d97f31bde3](https://medium.com/codeburst/the-simple-rules-to-this-in-javascript-35d97f31bde3) 

The overarching rule is that `this` is determined at the time a function is invoked by inspecting where it’s called (a.k.a the call site). 

There are certain rules with rule 1 taking precedent over rule 2 (e.g. if there are competing rules applied, rule 1 will be followed rather than rule 5) 

********************************************Rule 1: the word `new` is used** 

If the word `new` is used to call the function, `this` inside the function is a brand new object 

```jsx
function constructorExample() {
  console.log(this) // {}
	this.value = 5 
	console.log(this) // {value: 5}
}

new ConstructorExample() 
```

******************************Rule 2: If `apply`, `bind` or `call` are used to call the function, then `this` is the object that is passed into the function**

```jsx
function fn() {
	console.log(this)
}

var obj = {
	value: 5; 
}

var boundFunction = fn.bind(obj);
boundFunction(); // {value: 5}
fn.call(obj); // {value: 5}
fn.apply(obj); // {value: 5}
```

********Rule 3: If a function is called as a method -that is, if dot notation is used to invoke the function - `this` is the object that the function is a property of** 

```jsx
var obj = {
	value: 5;
	printThis: function() {
		console.log(This)	
	}
}

obj.printThis(); // {value: 5, printThis: function}
```

**************************************************************Rule 4: If a function is invoked as a free function invocation, then this is the global object. In the browser it’s `window`**

```jsx
function fn() {
	console.log(this);
}

// if called in the browser:
fn(); // -> Window {stop: function, open: function, alert: function ...}
```

****************Rule 5: If multiple of rules 1 - 4 are applied, the rule that is higher wins**************** 

********************************************************************Rule 6: If an arrow function is used, disregard rules 1-5. `this` will refer to the object in the surrounding scope at the time it is created**

```jsx
const object = {
		value: 'abc';
		createArrowFn: function () {
			return () => console.log(this);
  }
}

const arrowFn = obj.createArrowFn();
arrowFn(); // -> {value: 'abc', createArrowFn: function}
```

**************************************Applying the rules:**************************************

```jsx
var object = {
	value: 'hi';
	printThis: function() {
		console.log(this);
	} 
}

var print = obj.printThis;
obj.printThis(); // it'll be {value: 'hi', printThis: function} because it 
// was called with dot notation 
print() // this will be window / global 

```

**************************************Multiple Competing Rules**************************************

```jsx
var obj1 = {
	value: 'hi';
	print: function() {
			console.log(this)
		}
}

var obj2 = {value: 17}
obj1.print.call(obj2) // {value: 17} // this is because we passed obj2 into the function

new obj1.print() // -> {} // because we used new 
```

********************Why `this` is important** 

It’s important because libraries sometimes intentionally bind the value of `this` inside their functions. `this` is bound to the most useful value for use in the function. jQuery, for example, binds `this` to the DOM element triggering an event in the callback to that event. If a library has an unexpected value `this` value, it’ll be useful to check its documentation. It’s likely being bound with `bind`.