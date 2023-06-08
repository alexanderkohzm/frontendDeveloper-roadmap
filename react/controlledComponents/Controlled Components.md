# Controlled Components

[https://www.robinwieruch.de/react-controlled-components/](https://www.robinwieruch.de/react-controlled-components/)

So what’s an uncontrolled and controlled component?

This is an uncontrolled component. The issue with it is that the `input` HTML element is NOT controlled by React. While it might seem like you’re interacting with it and changing the value, you’re not really doing much as you’re just changing the HTML’s value.

This is because the HTML element handles its own state

**\*\***Uncontrolled Input**\*\***

```tsx
const App = () => {
  return (
    <div>
      <label>
        My uncontrolled Input: <input type="text" />
      </label>
    </div>
  );
};
```

Even if we try to add react states to it, it’s STILL not controlled. In fact, it’s even worse because it appears like it’s synchronised but in reality the input field still tracks its own internal state while the “output” we have from the React state is driven by the React state

- input field receives its value from internal DOM node state
- output paragraph receives tis value from React’s state

Having uncontrolled elements and components in your React application can lead to unwanted behaviour and bugs. You should really drive your UI from ONE source of truth - which is React’s props and state

**Still uncontrolled input**

```tsx
const App = () => {
	const [value, setValue] = useState('Hello React');

	const handleChange = event => setValue(event.target.value)

	return (
		<div>
			<label>
				My still uncontrolled Input:
				<input type="text" onChange={handleChange} />
			</label>

		<p>
			<strong> Output: </strong> {value}
		</p>
	)

}
```

What you need to do is to control the value of the input yourself. In this case, the input field offers the value attribute which you can pass the value state you have created. Now, the input does NOT rely on its internal state anymore but the state you provided it with React. Now things will be synchronised.

**Controlled input - note how value field is added**

```tsx
const App = () => {
	const [value, setValue] = useState('Hello React');

	const handleChange = event => setValue(event.target.value)

	return (
		<div>
			<label>
				My controlled Input:
				<input type="text" onChange={handleChange} value={value} />
			</label>

		<p>
			<strong> Output: </strong> {value}
		</p>
	)

}
```
