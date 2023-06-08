# LifeCycle

If you have used React Class Components before, you may be used to lifecycle methods such as `componentDidMount`, `componentWillUnmount`, and `shouldComponentUpdate`. We don’t have these in Function Components. So how do we implement them instead?

## Constructor? Or not…

First, there is **no constructor in a Function Component**. The constructor would have been used to allocate initial state but since we have useState we don’t need the constructor anymore.

## React Functional Component: Mount

Ok so what happens if we want something to happen when the React Component is mounted for the first time? Previously, we would use the `componentDidMount` method. However, now we can just use the `useEffect` method

```tsx
import React, { useState, useEffect } from "react";

const App = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = () => setCount((currentCount) => currentCount + 1);

  const handleDecrement = () => setCount((currentCount) => currentCount - 1);

  useEffect(() => {
    setCount((currentCount) => currentCount + 1);
  }, []);

  return (
    <div>
      <h1>{count}</h1>

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

export default App;
```

## React Functional Component: Update

Every time incoming props or state of the component changes, the component triggers a re-render to display the latest status quo (the status quo is often derived from props and state).

A render executes everything within the Function Component’s body.

In terms of our useEffect, we can get it to listen to a variable. Every time one of the variables changes, the effect hook runs.

```tsx
useEffect(() => {
  localStorage.setItem("storageCount", count);
}, [count]);
```

This useEffect stores localStorage every time there is a change to the variable “count”

## Pure React Function Component

React Class Components offered the possibility to decide whether a component has to re-render or not. This was through the `PureComponent` or `shouldComponentUpdate` functionalities to prevent performance bottlenecks in React to prevent re-rendering

Imagine this example

```tsx
import React, { useState } from "react";

const App = () => {
  const [greeting, setGreeting] = useState("Hello React!");
  const [count, setCount] = useState(0);

  const handleIncrement = () => setCount((currentCount) => currentCount + 1);
  const handleDecrement = () => setCount((currentCount) => currentCount - 1);

  const handleChange = (event) => setGreeting(event.target.value);

  return (
    <div>
      <input type="text" onChange={handleChange} />
      <Count count={count} />
      <button type="button" onClick={handleIncrement}>
        Increase
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrease
      </button>
    </div>
  );
};

const Count = ({ count }) => {
  console.log("Does this re-render");
  return <h1> {count}</h1>;
};
```

The issue is that the count re-renders every time the greeting changes. This is because the variable `greeting` changes in the `App` component, thus triggering the re-rendering of the entire component. We see that as Count’s component has a “Does this re-render” console.log every time we change something in the input.

A solution to this is the use the `memo` functionality provided by React

```tsx
const Count = memo(({ count }) => {
  console.log("DOes this re-render");
  return <h1> {count}</h1>;
});
```

Now the Count component does not update whenever things change in the input field. It only re-renders when the count changes. This is one way of optimising performance and it shouldn’t be used as a default

## React Function Component: Ref

A React Ref should only be used in rare cases such as accessing/manipulating the DOM manually (e.g. focus element), animations, and integrating third-party DOM libraries (e.g. D3). If you have to use a Ref in a Functional Component, you can define it within the component.

```tsx
const Input = () => {
  const ref = useRef();

  useEffect(() => ref.current.focus(), []);

  return <input ref={ref} />;
};
```

It’s important to note that you CANNOT pass refs - the ref will be assigned to the component instance but not to the actual DOM node.

If you want to pass a ref from a Parent Component to the Child Component, you will need to use `forwardRef` instead.
