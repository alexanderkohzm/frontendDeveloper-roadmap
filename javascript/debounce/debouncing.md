# Debouncing

Debouncing is a technique to control how many times we allow a function to be executed over a period of time.

For example, if a JavaScript function is debounced with a wait time of X milliseconds, it must wait until the time has passed before it can be called again. 

[https://medium.com/@griffinmichl/implementing-debounce-in-javascript-eab51a12311e](https://medium.com/@griffinmichl/implementing-debounce-in-javascript-eab51a12311e) 

[https://css-tricks.com/debouncing-throttling-explained-examples/](https://css-tricks.com/debouncing-throttling-explained-examples/) 

Having a debounce/throttler or our function is especially useful when we are attaching the function to a DOM event. This is because we are giving ourselves a layer of control between the event and the execution of the function 

For example, if we attach an expensive function to the scrolling event, it can impact the UI (make things unresponsive) 

The Debounce technique can allow us to group together multiple sequential calls into one. This can be useful in many cases. For example, imagine you want to resize your window. Resizing it will trigger many resize events. Alternatively, you could group then all in a debounce and only care about a specific resize event, i.e. the last resize event as that indicates the final size of the window

**********************************This is how you can implement a debounce********************************** 

```jsx
function debounce(func, wait) {

	let timeoutID = null; 

	return function(...args) {

		const context = this; 
		// clear the timeout
		clearTimeout(timeoutID)		

		timeoutID = setTimeout(() => {
				timeoutID = null;
				func.apply(context, args)
			}, wait)
		}
}
```

There’s a couple of things you should understand 

First, the `setTimeout` function returns a timeoutID. This is a positive integer value which identifies the timer created by the call to `setTimeout()`. This value can be passed to a `clearTimeout()` to cancel the timeout 

We initialise a timeoutID variable and set it to null. 

The `debounce()` function returns an inner function that takes any amount of arguments `(...args)`. This inner function represents the debounced version of the original function. 

When the debounced function is called, it first stores the reference to the `this` value in the `context` variable. Then, it clears any existing timeout by calling `clearTimeout(timeoutID)`. This step is crucial as it cancels any existing pending invocation of the debounced function. 

We then set a new timeout using the `setTimeout` function. This timeout function will be executed after the specified `wait` duration. 

Inside the timeout function, the `timeoutID` is set to null to indicate that the debounce function has finished its delayed execution. 

The original function `func` is invoked using the `apply` method. The `apply` method ensures the function is called with the correct context (`this`) and the arguments (`args`) passed to the debounced function.