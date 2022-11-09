<div class="rich-link-card-container"><a class="rich-link-card" href="https://beta.reactjs.org/learn" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://beta.reactjs.org/logo-og.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Quick Start</h1>
		<p class="rich-link-card-description">
		A JavaScript library for building user interfaces
		</p>
		<p class="rich-link-href">
		https://beta.reactjs.org/learn
		</p>
	</div>
</a></div>

# JSX

- expressions in JSX use ==single curly braces==

```jsx
<h1>Hello, {user.name}</h1>
```

- wrap multi-line JSX in parentheses to avoid semicolon issues
- all tags must be closed

## Attributes

- attributes use quotes for string values or curly braces for expressions
    - React attribute names are camelCased

```jsx
<img tabIndex="0" src={user.avatarUrl}></img>
```

## Classes and Styles

- classes are added using `className`
- the `style` attribute accepts an object instead of a string
    - use double curly braces because it is an object inside a JSX expression
    - numeric values automatically have "px" appended where appropriate

```jsx
<img className="avatar" style={ { height: 10 } } />
```

# Rendering

- to render an element into a root DOM node use `ReactDOM.render(element, rootNode)`
    - React apps normally have a single root node, and most only call `ReactDOM.render` once

# Components

- component names **must** start with a capital letter
- the return value is what is rendered
- to render multiple elements wrap them in fragments:

```jsx
const Cafe = () => {
  return (
    <>
      <Cat name="Munkustrap" />
      <Cat name="Spot" />
    </>
  );
};
```

- can also render multiple elements by returning an array with keys on each element (see [[#Lists Keys]])

> [!important]
> Component rendering should be **pure**, meaning the same inputs will produce the same output. Any data the component depends on should be stored as [[#Props|props]] or [[#useState|state]].
>
> You can wrap your app in `<React.StrictMode>` to call each component twice during development to find impure components.

# Props

- props are read-only
- props are passed to function components as the *props* argument
    - can use destructuring to access them easier
- state which is shared by multiple components should be kept in their closest common ancestor, and passed down as props

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

const element = <Welcome name="Sara" />;
```

# Events

- DOM event names are written in camelCase
    - instead of custom events like Vue, React uses callbacks passed as props and called by the child component
- event handlers are passed as functions

```jsx
function MyButton() {
    function activateLasers() {
        alert('Lasers activated!')
    }

    return (
        <button onClick={activateLasers}>
            Activate Lasers
        </button>
    )
}
```

- to pass arguments to event handlers, either of the below will work (extra arguments to `bind` are prepended)

```jsx
render() {
    return (
        <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
        <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
    )
}
```

# Conditional Rendering

- React elements can be stored in variables

```jsx
const button = <LoginButton />;
return (
    <div>
        {button}
    </div>
)
```

- `&&` can be used in expressions to ==conditionally include elements==

```jsx
return (
    <div>
        <h1>Hello!</h1>
        {unreadMessages.length > 0 &&
            <h2>
                You have {unreadMessages.length} unread messages.
            </h2>
        }
    </div>
)
```

- can also use ternary conditional operator

```jsx
return (
    <div>
        {isLoggedIn
            ? <LogoutButton />
            : <LoginButton />
        }
    </div>
)
```

- to avoid rendering a prop, ==return null== from its `render` function
    - class component lifecycle methods like `componentDidUpdate` will still be called

```jsx
function WarningBanner(props) {
    if (!props.warn) {
        return null;
    }

    return (
        <div className="warning">
        Warning!
        </div>
    );
}

class Page extends React.Component {
    render() {
        return (
            <div>
            {/* will not be rendered if this.state.showWarning is falsy */}
                <WarningBanner warn={this.state.showWarning}>
            </div>
        )
    }
}
```

# Lists & Keys

- render arrays using a map function to create the corresponding elements
- list items should have a unique (among siblings) string key
    - do not use index as key if item order may change
    - key is not a prop and is not passed to the component as one

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map(number => 
    <li key={number.toString()}>
        {number}
    </li>
);

ReactDOM.render(
    <ul>{listItems}<ul>,
    document.getElementById('root')
);
```

- can inline the `map()` call:

```jsx
const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
    <ul>
        {numbers.map(number => 
            <ListItem key={number.toString()} value={number} />
        )}
    </ul>
);
```

# Hooks

- Hooks let you:
    - avoid writing classes
    - reuse stateful logic (ex. connecting to a store) without adding more components
    - group related logic in different lifecycle methods together
- Hooks must be imported from React

## Rules of Hooks

- ==Only call Hooks at the top level== - hooks need to be called in the same order every time
    - if you want to call a hook from a condition or loop, extract a new component and put it there
- Only call Hooks from React functions (components or custom Hooks), not standard JS functions

## Custom Hooks

- are normal functions with any signature, but must start with `use` and follow the [[#Rules of Hooks]]
- can call other Hooks

```jsx
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    /* ... */
  });

  return isOnline;
}

function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);
}
```

# useState

- use `useState` to preserve variables between renders
- the argument to `useState` is the initial state
    - can be any kind of value, including objects
- state should be treated as read-only - to update state objects or arrays, copy it to a new object/array and pass it to the `set` function
    - the `set` function always replaces, never merges
- to update the state from a child component, pass down the `set` function as a prop

```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

- if the same type of component is rendered in the tree same position, it will keep its state
    - to reset the state without changing the position, change the [[#Lists & Keys|key]]

```js
// Counter will keep its state when isFancy changes
return (
<div>
  {isFancy ? (
    <Counter isFancy={true} /> 
  ) : (
    <Counter isFancy={false} /> 
  )}
</div>
)
```

# useReducer

- reducers let you consolidate state update logic from multiple event handlers into one function
    - because reducers take the state as an argument, you can declare them outside of the component
- similar to Vue custom events or Vuex store mutators
- use reducers if you often encounter bugs due to incorrect state updates
- reducers must be pure
- each action describes a single user interaction - ex. if a user presses Reset on a form with five fields, dispatch one `reset_form` action instead of 5 separate `set_field` actions

```js
function tasksReducer(tasks, action) {
    // return next state
    switch (action.type) {
        case 'added':
            return [
                // remember that tasks.push won't work because you can't mutate state
                ...tasks,
                {
                    id: action.id,
                    text: action.text
                }
            ]
        case 'deleted':
            return tasks.filter(t => t.id !== action.id)
    }
}
```

- to use the reducer in a component:

```js
import { useReducer } from 'react'

// instead of
// const [tasks, setTasks] = useState(initialTasks)
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks)

function handleAddTask(text) {
  dispatch({
    type: 'added',
    id: nextId++,
    text: text,
  });
}

function handleDeleteTask(taskId) {
  dispatch({
    type: 'deleted',
    id: taskId,
  });
}
```

# useEffect

- lets you perform side effects from a function component - similar to `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` rolled into one
    - examples of side effects: data fetching, setting up a subscription, manually changing the DOM
- using `useEffect` should be a last resort
- the "effect" function runs on every render, including the first one, after React has updated the DOM
- can return a "cleanup" function that will run before re-running the "effect" function, as well as on component unmount
- pass an array as the second argument to `useEffect`, and the effect (including cleanup) will only run if any of the array items have changed
    - if you pass an empty array, the effect will only run once on mount (and cleanup on unmount)
        - code should still be resilient to effects running multiple times, both for Fast Refresh and future-proofing

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;

    return () => { /* cleanup can go here */ };
  }, [count]); // Only re-run the effect if count changes

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

# useRef

- `ref`s will hold onto information between renders, but aren't tracked by React and don't trigger new renders
- `ref`s should be used for information that is needed by **event handlers**, but not for rendering, for example:
    - timeout/interval IDs
    - DOM elements
- **don't read or write `ref.current` during render**, unless only doing it on the first render (ex. `if (!ref.current) ref.current = ...`)

```js
import { useRef } from 'react'
const ref = useRef(0) // can be any value, like useState

// ref looks like this
{
    current: 0
}

// ref.current is mutable
ref.current = ref.current + 1
```

## DOM element refs

- to get a DOM element ref, set the `ref` to `null` and set it as the `ref` attribute on an element
- example uses:
    - focusing text inputs
    - scrolling to elements
- avoid modifying the DOM manually

```jsx
const myRef = useRef(null)

<div ref={myRef}>
```

- you cannot put `ref`s on React components unless they are wrapped in `forwardRef`
    - should only be used on low-level components like custom buttons or inputs

```jsx
const myInput = forwardRef((props, ref) => {
    return <input {...props} ref={ref} />
})
```

# Context

#todo https://beta.reactjs.org/learn/passing-data-deeply-with-context

# --- old stuff below ---

This information hasn't been updated from the new tutorial yet :)

# Forms

- *controlled components* are form elements whose value is controlled by React, so that the React state is the single source of truth (similar to v-model)

```jsx
class NameForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: ''};

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value: event.target.value});
    }

    handleSubmit(event) {
        alert('A name was submitted: ' + this.state.value);
        event.preventDefault();
    }

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <label>
                    Name:
                    <input type="text" value={this.state.value} onChange={this.handleChange} />
                </label>
                <input type="submit" value="Submit" />
            </form>
        );
    }
}
```

- `<textarea>` uses a `value` attribute for its text: `<textarea value={this.state.value} />`
- `<select>` uses a `value` attribute instead of putting a `selected` attribute on the `<option>` element

```jsx
this.state = {value: 'coconut'};
    
/* ... */

return (
    <select value={this.state.value} onChange={this.handleChange}>
        <option value="grapefruit">Grapefruit</option>
        <option value="lime">Lime</option>
        <option value="coconut">Coconut</option>
        <option value="mango">Mango</option>
    </select>
);
```

- `<select>`'s `value` attribute can accept an array for multiple selections: `<select multiple={true} value={['B', 'C']}>`
- the `name` attribute can be used to distinguish multiple `<input>`s using the same handler

```jsx
class Reservation extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isGoing: true,
            numberOfGuests: 2
        };

        this.handleInputChange = this.handleInputChange.bind(this);
    }

    handleInputChange(event) {
        const target = event.target;
        const value = target.name === 'isGoing' ? target.checked : target.value;
        const name = target.name;

        this.setState({
            [name]: value
        });
    }

    render() {
        return (
            <form>
                <label>
                    Is going:
                    <input
                        name="isGoing" type="checkbox"
                        checked={this.state.isGoing}
                        onChange={this.handleInputChange} />
                </label>
                <br />
                <label>
                    Number of guests:
                    <input
                        name="numberOfGuests" type="number"
                        value={this.state.numberOfGuests}
                        onChange={this.handleInputChange} />
                </label>
            </form>
        );
    }
}
```

# Composition vs. Inheritance

- components can use the `children` prop to directly pass through child elements, or pass React elements through props similarly to Vue slots (since elements are objects)

```jsx
function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
            {props.children}
        </div>
    );
}

function SplitPane(props) {
    return (
        <div className="SplitPane">
            <div className="SplitPane-left>
                {props.left}
            </div>
            <div className="SplitPane-right>
                {props.right}
            </div>
        </div>
    );
}

function App() {
    return (
        <FancyBorder>
            <h1>Welcome</h1>
            <p>Thank you for visiting!</p>
        </FancyBorder>

        <SplitPane
            left={
                <Contacts />
            }
            right={
                <Chat />
            }
        />
    )
}
```

- components should favor composition (rendering more "generic" components with configured props to create more "specific" components) over inheritance

``` jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

# Render Props

- components that take a render function as a prop and call it in their own render function

```jsx
class Mouse extends React.Component {
  /* ... */

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

- lets you reuse functionality by creating "wrapper" components (like slots in Vue)
    - wrapper passes information to the child using arguments in the render prop (for example, a `<Mouse>` wrapper that passes pointer coordinates)
- mark prop as function type:

```jsx
Mouse.propTypes = {
  children: PropTypes.func.isRequired
};
```

- TypeScript function component example:

```tsx
function Example({ render }: { render: () => JSX.Element }) {
    /* ... */
}
```

# Higher Order Components

- function that takes a component and returns a new component
    - a component transforms props into UI, an HOC transforms components into other components
- another technique for reusing component logic
- names start with `with`

```jsx
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}

const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```

- HOC function can have as many arguments as needed
- HOCs do not modify the original component, or use inheritance - rather they compose it by wrapping it with additional behavior
    - HOCs should be pure functions with no side effects
    - do not mutate the input component's prototype
    - be sure to **pass through props** that aren't used by the HOC
    - refs are not passed through - use `React.forwardRef`
- **don't apply HOCs inside the render method** (will cause remounting on every render, which discards state)
- **[[#Hooks]] might be better?**

# Context

- lets you share values between components in a tree without having to pass props through every level (ex. locale preferences, UI themes)
    - similar to Vue provide/inject

```jsx
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

- *this.context* in class components holds the context value
- use `Context.Consumer` to subscribe to contexts in a function component, including consuming multiple contexts
    - child is a function that receives the context value and returns a React node
    - **or `useContext` hook**
- if lower level components need to update the context, pass an object as the `createContext` value that includes a function to update the component

```jsx
export const ThemeContext = React.createContext({
  theme: themes.dark,
  toggleTheme: () => {},
});

function ThemeTogglerButton() {
  return (
    <ThemeContext.Consumer>
      {({theme, toggleTheme}) => (
        <button
          onClick={toggleTheme}
          style={{backgroundColor: theme.background}}>
          Toggle Theme
        </button>
      )}
    </ThemeContext.Consumer>
  );
}
```

# Error Boundaries

- class components that catch errors from anything below them in the component tree
    - do not catch errors inside themselves
- must implement either (or both) `static getDerivedStateFromError(error)` or `componentDidCatch(error, info)`
    - `getDerivedStateFromError` returns an object that updates state, can be used to trigger a fallback UI (should not have side effects)
    - `componentDidCatch` can be used to log errors (can have side effects)
        - *info* contains the call stack
- if there are no error boundaries, the whole app will unmount if an error is thrown
- different parts of an application can have separate error boundaries, so that if one part breaks the rest will keep working (ex. the sidebar, conversation log, and message input in a messaging app)
- will not catch errors inside:
    - event handlers (which do not break rendering) - use try/catch for these
    - asynchronous code
    - SSR

# Class Components (deprecated)

```jsx
class Welcome extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}

const element = <Welcome name="Sara" />
```

## Pure Components

- extend `React.PureComponent`
- performs a **shallow** comparison of props and state and only re-renders if there are differences

## State and Lifecycle

```jsx
class Clock extends React.Component {
    constructor(props) {
        super(props); // must be called
        this.state = {date: new Date()};
    }

    componentDidMount() {
        // it is okay to add non-reactive fields directly to `this`
        this.timerID = setInterval(
            () => this.tick(),
            1000
        );
    }

    componentWillUnmount() {
        clearInterval(this.timerID);
    }

    tick() {
        // use `this.setState` to update the state
        this.setState({
            date: new Date()
        });
    }

    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
            </div>
        );
    }
}

ReactDOM.render(
    <Clock />,
    document.getElementById('root')
);
```

- ==non-reactive fields can be added directly to `this`==
- state may be passed to child components as props (by setting attribute on the component element)
- `setState` performs a shallow merge
- `props` and `state` may be updated asynchronously, so do not rely on their values in `setState` calls
    - instead pass a function to `setState`:

```jsx
// Wrong
this.setState({
    counter: this.state.counter + this.props.increment,
});

// Correct
this.setState((state, props) => ({
    counter: state.counter + props.increment
}));
```

- Is it passed in from a parent via props? If so, it probably isn’t state.
- Does it remain unchanged over time? If so, it probably isn’t state.

> [!tip]
> Can you compute it based on any other state or props in your component? If so, it isn’t state.

## Events

- class methods are not bound by default, so any methods which are passed to other components (such as event handlers) ==should be bound in the constructor==

```jsx
class Toggle extends React.Component {
    constructor(props) {
        super(props);
        this.state = {isToggleOn: true};

        // This binding is necessary to make `this` work in the callback
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        this.setState(state => ({
            isToggleOn: !state.isToggleOn
        }));
    }

    // can use public class fields syntax instead of binding in constructor
    // (experimental, but enabled by default in Create React App):
    handleClick = () => {
        console.log('this is:', this);
    }

    render() {
        return (
            <button onClick={this.handleClick}>
                {this.state.isToggleOn ? 'ON' : 'OFF'}
            </button>
        );
    }
}
```

## Refs

- created using `React.createRef()`
- node is accessible at the `current` property of the ref
    - `ref`s receive either the HTML element or mounted component instance as appropriate, just like Vue
    - ref updates happen before `componentDidMount` or `componentDidUpdate`

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  someFunction() {
    this.myRef.current.click()
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

- **cannot use `ref` on function components**, since they have no instances
    - you can use `ref` **inside** a function component, as long as it is not referring to a function component

### forwardRef

- lets you define a component that takes a ref and forwards it to a child
    - useful for "leaf" components like buttons or text inputs that wrap a DOM element and are used in place of it

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;

// ref.current will refer to the DOM <button> inside <FancyButton>
```

# See also

- [[React Native cheat sheet]]
