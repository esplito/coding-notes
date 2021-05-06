# Advanced React Patterns
The following patterns are brought up in the "Advanced React Patterns" course module at epicreact.dev, by Kent C. Dodds.  

**Please Note: This summary was written before the course was re-written in TypeScript.**

Github repo: [https://github.com/kentcdodds/advanced-react-patterns](https://github.com/kentcdodds/advanced-react-patterns)

The course content with background information for each pattern, can be found here: [https://advanced-react-patterns.netlify.app/](https://advanced-react-patterns.netlify.app/)

## Context Module Functions

What does this even mean? Well, the idea with this pattern is that you **create an importable "helper"-function** that accepts **dispatch**.

### Pros
✅ Can help reduce duplication
✅ May have performance benefits → "You only pay for what you use, where you use it." - Dan Abramov ([https://twitter.com/dan_abramov/status/1125774170154065920](https://twitter.com/dan_abramov/status/1125774170154065920))
✅ Helps avoid mistakes in dependency lists

### Example code
Let's say that you have a form with a function, 'handleSubmit' that you call on submit. This function currently lives within our component.

**Parts of the UserSettings component**
```js
// Inside the UserSettings-component
function handleSubmit(event) {
  event.preventDefault()
  userDispatch({type: 'start update', updates: formState})
  userClient.updateUser(user, formState).then(
    updatedUser => userDispatch({type: 'finish update', updatedUser}),
    error => userDispatch({type: 'fail update', error}),
  )
}
```
But if we would want to follow the Context Module Functions-pattern then we have to move most of the logic that it contains, to a function that we instead will import and use in our component. We want our 'handleSubmit'-function to look like this instead:

**New 'handleSubmit' in UserSettings component**
```js
import {updateUser} from './context/user-context'
function handleSubmit(event) {
  event.preventDefault()
  updateUser(userDispatch, user, formState).catch(() => {
    /* ignore the error */
  })
}
```
  
Our importable helper function will look like this:

**updateUser**
```js
// inside our context-file
async function updateUser(dispatch, user, updates) {
  dispatch({type: 'start update', updates})
  try {
    const updatedUser = await userClient.updateUser(user, updates)
    dispatch({type: 'finish update', updatedUser})
    return updatedUser
  } catch (error) {
    dispatch({type: 'fail update', error})
    return Promise.reject(error)
  }
}
export {updateUser}
```
  
If you want to look at the complete files go to [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/01.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/01.js) and [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/01.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/01.js).

## Compound Components

What do we mean with Compound Components? We mean **components that work together to form a complete UI**. Dodds mentions a simple example of a compound component in HTML, which is a select element with option elements.  
 
 **Simple Compound Component in HTML** 
```html
<select>
  <option value="1">Option 1</option>
  <option value="2">Option 2</option>
</select>
```
  
The **select** is responsible for managing the UI state, meanwhile the **option** elements act as configuration for how the select should operate (which options are available and what are their values). We can use this pattern for our React Components as well.

More background on this can be found here: [https://advanced-react-patterns.netlify.app/2](https://advanced-react-patterns.netlify.app/2)

### Pros
✅ Good for creating more flexible and capable components.

### Cons
❌ We can only clone and pass props to immediate children. (To be able to share the state to nested children we have to use the Context API. Check out the ["Flexible Compound Components"](#ReactPatterns-ReactPatterns-FlexibleCompoundComponents) pattern.)

### Example code
Original exercise code here: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/02.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/02.js)

The toggle component shares some state implicitly with each one of the child components.

Source code: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/02.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/02.js)

**Compound Components**
```js
import * as React from 'react'
import {Switch} from '../switch'

function Toggle({children}) {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)
  return React.Children.map(children, child =>
    React.cloneElement(child, {on, toggle}),
  )
}

function ToggleOn({on, children}) {
  return on ? children : null
}

function ToggleOff({on, children}) {
  return on ? null : children
}

function ToggleButton({on, toggle, ...props}) {
  return <Switch on={on} onClick={toggle} {...props} />
}

function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <ToggleButton />
      </Toggle>
    </div>
  )
}

export default App
```

## Flexible Compound Components

As the name says, this pattern revolves around making compound components even more flexible. This is done by using the [Context API](https://reactjs.org/docs/context.html) so that we can share the state with deeply nested children.

> This is probably the most important use case for context where we can share this implicit state for components that we expose to people for their use without them having to worry about the state that's being managed to make these things work together. - Kent C. Dodds

More background info on the subject can be found here: [https://advanced-react-patterns.netlify.app/3](https://advanced-react-patterns.netlify.app/3)

### Pros
✅ Allows people to render the compound components wherever they like in the render tree

### Example code

Source code: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/03.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/03.js)

**Code before pattern is applied**
```js
import * as React from 'react'
import {Switch} from '../switch'

function Toggle({children}) {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  return React.Children.map(children, child => {
    return typeof child.type === 'string'
      ? child
      : React.cloneElement(child, {on, toggle})
  })
}

function ToggleOn({on, children}) {
  return on ? children : null
}

function ToggleOff({on, children}) {
  return on ? null : children
}

function ToggleButton({on, toggle, ...props}) {
  return <Switch on={on} onClick={toggle} {...props} />
}

function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <div>
          <ToggleButton />
        </div>
      </Toggle>
    </div>
  )
}

export default App
```
 
Source: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/03.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/03.js)

**Code after applying the "Flexible Compound Components" pattern**

```js
import * as React from 'react'
import {Switch} from '../switch'

const ToggleContext = React.createContext()
ToggleContext.displayName = 'ToggleContext'

function Toggle({children}) {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  return (
    <ToggleContext.Provider value={{on, toggle}}>
      {children}
    </ToggleContext.Provider>
  )
}

function useToggle() {
  return React.useContext(ToggleContext)
}

function ToggleOn({children}) {
  const {on} = useToggle()
  return on ? children : null
}

function ToggleOff({children}) {
  const {on} = useToggle()
  return on ? null : children
}

function ToggleButton({...props}) {
  const {on, toggle} = useToggle()
  return <Switch on={on} onClick={toggle} {...props} />
}

function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <div>
          <ToggleButton />
        </div>
      </Toggle>
    </div>
  )
}

export default App
```

## Prop Collections & Prop Getters

The **prop collections** pattern means that you return an object with the props that is needed when using a custom hook and this object (could be called **togglerProps**) can be passed into the relevant component with the [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax). Let's say that we have a custom hook called **useToggle,** that we want to use for toggling the active state of component. The object returned from this hook will have all the props that we would typically apply for this component, e.g. **onClick**-function, **aria-pressed**-attribute etc.

The **prop getters** pattern is different to the **prop collections** pattern in terms of flexibility. Instead of our users being locked to the logic that we have specified for the **togglerProps**-object in our hook, they can use their own logic and anything else that they want to have and still get the enhanced capabilities from the props in our **prop getters.**  
  
These two patterns are used in the following open source projects:

-   [downshift](https://github.com/downshift-js/downshift) (prop getters)
-   [react-table](https://github.com/tannerlinsley/react-table) (prop getters)
-   [@reach/tooltip](https://reacttraining.com/reach-ui/tooltip) (prop collections)

Dodds mostly use prop getters:

> I don't typically use prop collections. I defer to prop getters because they're more flexible in that they allow me to compose things together. - Kent C. Dodds

More background info can be found here: https://advanced-react-patterns.netlify.app/4

### Pros

✅ Makes it easier to use our components and hooks the correct way without requiring the user of the components to wire things up for common use cases.

✅ With **prop getters** we can also make it more flexible and let the users have their own logic, while still getting the benefits of the props provided in our **prop getter**.

### Example code

Here's two examples from [Kent C. Dodds course 'Advanced React Patterns'](https://github.com/kentcdodds/advanced-react-patterns).  
  
**Prop Collections**
Source code: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/04.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/04.js)
```js
import * as React from 'react'
import {Switch} from '../switch'

function useToggle() {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  return {
    on,
    toggle,
    togglerProps: {
      'aria-pressed': on,
      onClick: toggle,
    },
  }
}

function App() {
  const {on, togglerProps} = useToggle()
  return (
    <div>
      <Switch on={on} {...togglerProps} />
      <hr />
      <button aria-label="custom-button" {...togglerProps}>
        {on ? 'on' : 'off'}
      </button>
    </div>
  )
}

export default App
```
**Prop Getters**
Source code: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/04.extra-1.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/04.extra-1.js)

```js
import * as React from 'react'
import {Switch} from '../switch'

const callAll = (...fns) => (...args) => fns.forEach(fn => fn?.(...args))

function useToggle() {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  function getTogglerProps({onClick, ...props} = {}) {
    return {
      'aria-pressed': on,
      onClick: callAll(onClick, toggle),
      ...props,
    }
  }

  return {
    on,
    toggle,
    getTogglerProps,
  }
}

function App() {
  const {on, getTogglerProps} = useToggle()
  return (
    <div>
      <Switch {...getTogglerProps({on})} />
      <hr />
      <button
        {...getTogglerProps({
          'aria-label': 'custom-button',
          onClick: () => console.info('onButtonClick'),
          id: 'custom-button-id',
        })}
      >
        {on ? 'on' : 'off'}
      </button>
    </div>
  )
}

export default App
```
## State Reducer
The state reducer pattern is an implementation of the computer science principle "inversion of control". This pattern translates even better to hooks than it did to regular components. [Source: Kent C. Dodds - The State Reducer Pattern with React Hooks](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks)

> All right. State reducer's probably one of my very favorite patterns. I love the state reducer pattern. It is super powerful, an excellent implementation of inversion of control. - Kent C. Dodds

More background info can be found here: https://advanced-react-patterns.netlify.app/5

### Pros

✅ Enhances the power and flexibility of your hooks

### Cons
❌ If not done properly, we may end up giving the users of our custom hook or custom component, too much control.

### Example code
According to Dodds, this is a pretty contrived example of just a toggle component. You probably shouldn't use this pattern unless you have a more complicated problem to solve.

***Code before implementing the pattern***
Source code: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/05.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/05.js)
```js
// State Reducer
// http://localhost:3000/isolated/exercise/05.js

import * as React from "react";
import { Switch } from "../switch";

const callAll = (...fns) => (...args) => fns.forEach((fn) => fn?.(...args));

function toggleReducer(state, { type, initialState }) {
  switch (type) {
    case "toggle": {
      return { on: !state.on };
    }
    case "reset": {
      return initialState;
    }
    default: {
      throw new Error(`Unsupported type: ${type}`);
    }
  }
}

// 🐨 add a new option called `reducer` that defaults to `toggleReducer`
function useToggle({ initialOn = false } = {}) {
  const { current: initialState } = React.useRef({ on: initialOn });
  // 🐨 instead of passing `toggleReducer` here, pass the `reducer` that's
  // provided as an option
  // ... and that's it! Don't forget to check the 💯 extra credit!
  const [state, dispatch] = React.useReducer(toggleReducer, initialState);
  const { on } = state;

  const toggle = () => dispatch({ type: "toggle" });
  const reset = () => dispatch({ type: "reset", initialState });

  function getTogglerProps({ onClick, ...props } = {}) {
    return {
      "aria-pressed": on,
      onClick: callAll(onClick, toggle),
      ...props,
    };
  }

  function getResetterProps({ onClick, ...props } = {}) {
    return {
      onClick: callAll(onClick, reset),
      ...props,
    };
  }

  return {
    on,
    reset,
    toggle,
    getTogglerProps,
    getResetterProps,
  };
}

function App() {
  const [timesClicked, setTimesClicked] = React.useState(0);
  const clickedTooMuch = timesClicked >= 4;

  function toggleStateReducer(state, action) {
    switch (action.type) {
      case "toggle": {
        if (clickedTooMuch) {
          return { on: state.on };
        }
        return { on: !state.on };
      }
      case "reset": {
        return { on: false };
      }
      default: {
        throw new Error(`Unsupported type: ${action.type}`);
      }
    }
  }

  const { on, getTogglerProps, getResetterProps } = useToggle({
    reducer: toggleStateReducer,
  });

  return (
    <div>
      <Switch
        {...getTogglerProps({
          disabled: clickedTooMuch,
          on: on,
          onClick: () => setTimesClicked((count) => count + 1),
        })}
      />
      {clickedTooMuch ? (
        <div data-testid="notice">
          Whoa, you clicked too much!
          <br />
        </div>
      ) : timesClicked > 0 ? (
        <div data-testid="click-count">Click count: {timesClicked}</div>
      ) : null}
      <button {...getResetterProps({ onClick: () => setTimesClicked(0) })}>
        Reset
      </button>
    </div>
  );
}

export default App;

/*
eslint
  no-unused-vars: "off",
*/
```

**Code after implementing the 'State Reducer'-pattern**

Source code: [https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/05.extra-2.js](https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/05.extra-2.js)

```js
// state reducer
// 💯 state reducer action types
// http://localhost:3000/isolated/final/05.extra-2.js

import * as React from "react";
import { Switch } from "../switch";

const callAll = (...fns) => (...args) => fns.forEach((fn) => fn?.(...args));

const actionTypes = {
  toggle: "toggle",
  reset: "reset",
};

function toggleReducer(state, { type, initialState }) {
  switch (type) {
    case actionTypes.toggle: {
      return { on: !state.on };
    }
    case actionTypes.reset: {
      return initialState;
    }
    default: {
      throw new Error(`Unsupported type: ${type}`);
    }
  }
}

function useToggle({ initialOn = false, reducer = toggleReducer } = {}) {
  const { current: initialState } = React.useRef({ on: initialOn });
  const [state, dispatch] = React.useReducer(reducer, initialState);
  const { on } = state;

  const toggle = () => dispatch({ type: actionTypes.toggle });
  const reset = () => dispatch({ type: actionTypes.reset, initialState });

  function getTogglerProps({ onClick, ...props } = {}) {
    return {
      "aria-pressed": on,
      onClick: callAll(onClick, toggle),
      ...props,
    };
  }

  function getResetterProps({ onClick, ...props } = {}) {
    return {
      onClick: callAll(onClick, reset),
      ...props,
    };
  }

  return {
    on,
    reset,
    toggle,
    getTogglerProps,
    getResetterProps,
  };
}
// export {useToggle, toggleReducer, actionTypes}

// import {useToggle, toggleReducer, actionTypes} from './use-toggle'

function App() {
  const [timesClicked, setTimesClicked] = React.useState(0);
  const clickedTooMuch = timesClicked >= 4;

  function toggleStateReducer(state, action) {
    if (action.type === actionTypes.toggle && clickedTooMuch) {
      return { on: state.on };
    }
    return toggleReducer(state, action);
  }

  const { on, getTogglerProps, getResetterProps } = useToggle({
    reducer: toggleStateReducer,
  });

  return (
    <div>
      <Switch
        {...getTogglerProps({
          disabled: clickedTooMuch,
          on: on,
          onClick: () => setTimesClicked((count) => count + 1),
        })}
      />
      {clickedTooMuch ? (
        <div data-testid="notice">
          Whoa, you clicked too much!
          <br />
        </div>
      ) : timesClicked > 0 ? (
        <div data-testid="click-count">Click count: {timesClicked}</div>
      ) : null}
      <button {...getResetterProps({ onClick: () => setTimesClicked(0) })}>
        Reset
      </button>
    </div>
  );
}

export default App;

```

## Control Props
**Control Props** allows users of our components to handle a component's internal state from the "outside".

>Control props is an implementation of something that's supported in React by default for form elements. That is the idea of controlled components. - Kent C. Dodds

More background info can be found here: [https://advanced-react-patterns.netlify.app/6](https://advanced-react-patterns.netlify.app/6)

### Pros

✅ We can customize the different warnings that is displayed when someone uses our components incorrectly. → Easier for other developers to see what they did wrong.

### Example code

Code before implementing the pattern can be found here: https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/exercise/06.js

Code after implementing the pattern (but not customizing warnings etc.): https://github.com/kentcdodds/advanced-react-patterns/blob/main/src/final/06.js


If you want to add warnings to your custom hook to make sure that the control props are being used as intended, you can use the package "warning" which React uses.

Simple example of how a warning can be written:
```js
warning(false, 'this will be logged');
```

**Code example for extra credit 1 within our custom hook `useToggle`:**
```js
// Extra credit 1: add read only warning
// Passing on without onChange
const  hasOnChange  =  Boolean(onChange)
React.useEffect(() => {
  warning(
  !(!hasOnChange  &&  onIsControlled  &&  !readOnly),
  `An \`on\` prop was provided to useToggle without an \`onChange\` handler. This will render a read-only toggle. If you want it to be mutable, use \`initialOn\`. Otherwise, set either \`onChange\` or \`readOnly\`.`,
  )
}, [hasOnChange, onIsControlled, readOnly])
```
>Now at any time if the user of our hook forgets to pass an onChange but is controlling it, then they're going to get a nice helpful warning indicating to them that they've got a readOnly toggle value. - Dodds

Extra credit 2: add a controlled state warning.
This one has two cases:
* Uncontrolled -> controlled
* Controlled -> uncontrolled

Example:
```js
// Extra credit 2: add a controlled state warning
const {current: onWasControlled} =  React.useRef(onIsControlled)
  React.useEffect(() => {
	// case 1: uncontrolled to controlled
    warning(
      !(onIsControlled  &&  !onWasControlled),
      `\`useToggle\` is changing from uncontrolled to be controlled. Components should not switch from uncontrolled to controlled (or vice versa). Decide between using a controlled or uncontrolled \`useToggle\` for the lifetime of the component. Check the \`on\` prop.`,
    )
    // case 2: controlled to uncontrolled
    warning(
      !(!onIsControlled  &&  onWasControlled),
      `\`useToggle\` is changing from controlled to be uncontrolled. Components should not switch from controlled to uncontrolled (or vice versa). Decide between using a controlled or uncontrolled \`useToggle\` for the lifetime of the component. Check the \`on\` prop.`,
    )
}, [onIsControlled, onWasControlled])
```

Differences in the app component listed below.
Case 1:
```js
const [bothOn, setBothOn] =  React.useState() // <- Check this!
..
..
function  handleToggleChange(state, action) {
  if (action.type  === actionTypes.toggle  &&  timesClicked  >  4) {
    return
  }
  setBothOn(state.on) // <- Check this!
  setTimesClicked(c  =>  c  +  1)
}
```
Case 2:
```js
const [bothOn, setBothOn] =  React.useState(false) // <- Check this!
..
..
function  handleToggleChange(state, action) {
  if (action.type  === actionTypes.toggle  &&  timesClicked  >  4) {
    return
  }
  setBothOn() // <- Check this!
  setTimesClicked(c  =>  c  +  1)
}
```

Extra credit 3: extract warnings to a custom hook
> Both of those warnings could be useful anywhere so let’s go ahead and make a custom hook for them.
> Shout out to the Reach UI team for  [the implementation of the  `useControlledSwitchWarning`](https://github.com/reach/reach-ui/blob/a376daec462ccb53d33f4471306dff35383a03a5/packages/utils/src/index.tsx#L407-L443) - Dodds

Extra credit 4: don’t warn in production
```js
// Extra credit 4: don’t warn in production
// process.env.NODE_ENV will never change when running the app so it is okay to ignore the rules of hooks that we shouldn't run hooks conditionally.
if (process.env.NODE_ENV  !==  'production') {
  // eslint-disable-next-line react-hooks/rules-of-hooks
  useControlledSwitchWarning(controlledOn, 'on', 'useToggle')
  // eslint-disable-next-line react-hooks/rules-of-hooks
  useOnChangeReadOnlyWarning(
    controlledOn,
    'on',
    'useToggle',
    Boolean(onChange),
    readOnly,
    'readOnly',
    'initialOn',
    'onChange',
  )
}
```
>Depending on the situation, there could actually be bugs, so that's why it's important for people to listen to our warnings. The nice thing here is that when we build for production, not only do we not call those warnings but thanks to Dead Code Elimination, the entire function definition for all of this code is actually going to get wiped away. We're not even sending this code to production. - Dodds


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ0Nzc0ODI1NywxOTI1OTcxMzM4LDI4OT
YxNzY4NCwxMjU1ODUyMTgwLDY5NTY3MzI3LC0xMDkyMTkxMTIx
LDcyNzAwMDAwNV19
-->