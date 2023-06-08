# Functional and Class Components

[https://legacy.reactjs.org/docs/components-and-props.html#function-and-class-components](https://legacy.reactjs.org/docs/components-and-props.html#function-and-class-components)

[https://www.robinwieruch.de/react-function-component/](https://www.robinwieruch.de/react-function-component/)

## How would you write a functional and class component?

```tsx
// Functional Component
function MyFirstFunctionalComponent(props) {
  return(
  <div>Hello! {props.name}</div>
  )
}

// Class Component

Class FirstClassComponent extends React.Component {

  constructor(props) {
    super(props)
  }
  return () {
    return <div>Hello! {this.props.name}</div>
  }
}
```

## How would you render the element?

```tsx
function Welcome(props) {
  return <div>Hello! {props.name}</div>;
}

const root = ReactDOM.createRoot(document.getElementById("root"));
const element = <Welcome name={"Alex"} />;
root.render(element);
```

## Composing Components

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components

```tsx
function Welcome(props) {
  return <div> Hello {props.name} </div>;
}

function App() {
  return (
    <div>
      <Welcome name={"Alex"} />
      <Welcome name={"James"} />
      <Welcome name={"Tim"} />
    </div>
  );
}
```

In this case, we’ve reused the “Welcome” component many times

## Extracting Components

You should think about splitting components into smaller components. You especially get the reward of reusable components when it comes to larger apps. A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar) or is complex enough on its own (App, FeedStory, Comment), it is a good candidate to be extracted to a separate component

**For example: How would you split this large comment component into smaller reusable ones?**

```tsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img
          className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">{props.author.name}</div>
      </div>
      <div className="Comment-text">{props.text}</div>
      <div className="Comment-date">{formatDate(props.date)}</div>
    </div>
  );
}
```

- Answer
  You could think about splitting up the `UserInfo` part into a separate `Avatar` component
  ```tsx
  function Avatar(props) {
    const { author } = props;
    const { avatarUrl, name } = author;
    return (
      <div className="UserInfo">
        <img className="Avatar" src={avatarUrl} alt={name} />
        <div className="UserInfo-name">{name}</div>
      </div>
    );
  }

  function Comment(props) {
    return (
      <div className="Comment">
        <Avatar props={props.author} />
        <div className="Comment-text">{props.text}</div>
        <div className="Comment-date">{formatDate(props.date)}</div>
      </div>
    );
  }
  ```

## Props are Read-Only

When you declare a component as a function or a class, it must never modify its own props

**All React Components must act like pure functions with respect to their props**

Instead of changing props directly, we use **State**instead. State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating the pure function rule

## The evolution of Functional Components

Initially, we had to use Class Components because we needed them to have state. Our Functional Components were simply used to render things (e.g. by passing it props) and it was stateless. This changed, however, when React Hooks were introduced. Our Functional Components could now have state.

## Event Handlers

In React, we have several React event handlers such as onClick, onMouseDown, and onBlur.

```tsx
const HeadLine = () => {

  const [greeting, setGreeting] = useState('Hello! Functional Component')

	const handleChange = event => setGreeting(event.target.value)

	return (
			<div>
				<h1> {greeting}
				<input type="text" value={greeting} onChange={handleChange} />
			</div>
    )

}
```

You can create or add as many functions inside the Functional Component as you want to act as explicit event handlers or to encapsulate other business logic

## Callback Functions

It’s possible to pass a function to a component as prop and handle what happens in the Parent Component. For example, you can pass a setState or handleChange function down from the parent Functional Component to a child Functional Component

In this case, we’re passing down the handleChange function from the parent component to the child component.

```tsx
import React, { useState } from "react";

const App = () => {
  const [greeting, setGreeting] = useState("Hello Function Component!");

  const handleChange = (event) => setGreeting(event.target.value);

  return (
    <div>
      <Headline headline={greeting} />

      <Input value={greeting} onChangeInput={handleChange}>
        Set Greeting:
      </Input>
    </div>
  );
};

const Headline = ({ headline }) => <h1>{headline}</h1>;

const Input = ({ value, onChangeInput, children }) => (
  <label>
    {children}
    <input type="text" value={value} onChange={onChangeInput} />
  </label>
);

export default App;
```
