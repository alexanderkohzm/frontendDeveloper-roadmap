# Curried Functions

****************Curried Functions - Computerphile**************** [https://www.youtube.com/watch?v=psmu_VAuiag](https://www.youtube.com/watch?v=psmu_VAuiag)

**********************************************************************Understanding JavaScript currying:********************************************************************** [https://blog.logrocket.com/understanding-javascript-currying](https://blog.logrocket.com/understanding-javascript-currying) ****

<aside>
💡 **What actually is a function?**

A function simply takes in inputs (the arguments), processes it (within the function), and returns the output

</aside>

Currying is a function that takes ONE argument at a time and returns a new function expecting the next argument. For example:

```jsx
const regularAdd = (a, b, c) => {
	return a + b + c
} 

const curriedFunction = (a) => {
	return function(b) {
		return function(c) {
			return a + b + c
		}
	}	
}

console.log(regularAdd(1, 2, 3)) // should be 6 
console.log(curriedFunction(1)(2)(3)) // should be 6
```

```jsx
const regularAdd = (a, b, c) => {
	return a + b + c
} 

const curriedFunction = (a) => {
	return function(b) {
		return function(c) {
			return a + b + c
		}
	}	
}

console.log(regularAdd(1, 2, 3)) // should be 6 
console.log(curriedFunction(1)(2)(3)) // should be 6
```

Currying in JavaScript involves taking multiple arguments, decomposing them into a sequence of functions with a single argument. 
Some people would ask: Why would I use currying? What’s the point? 

- It can be used as a checking method (e.g. only proceed if everything is present)
- It also allows you to have purer functions as it divides your function into multiple smaller functions that can handle one responsibility. This makes your function pure and less prone to errors and side effects
- It is used in functional programming to create higher-order function

### Basic and Advanced Currying Techniques

A basic curry function involves:

```jsx
const getPancakeIngredients = (ingredient1) => {
	return (ingredient2) => {
		return (ingredient3) => {
				return `${ingredient1}, ${ingredient2}, ${ingredient3}`
				}
		}
}
```

This is basic because it simply takes in the ingredients and the function isn’t complete until it receives all parameters. This means that you’ll get unexpected results if you DON’T enter all ingredients (i.e. 3 of them) 

Here’s an example of more complex currying

```jsx
const curry = (fn) => {
	return curried = (...args) => {
		if (fn.length !== args.length) {
			return curried.bind(null, ...args)
		}
		return fn(...args)
	}
} 

const totalNumber = (x, y , z) => {
	return x + y + z
}
const curriedTotal = curry(totalNumber)
console.log(curriedTotal(10)(20)(30))
```

Our `curry` function receives an argument that is a function

This `curry` function is a wrapper function. It returns another function, `curried`, which receives an argument with the spread operator `(...args)`. 

The `curried` function compares the length of the function with the length of the args. If the lengths do not match, it returns the `curried` function with the args 

Once the function length matches args length, it returns the original function that we want to curry with all the args `fn(...args)`