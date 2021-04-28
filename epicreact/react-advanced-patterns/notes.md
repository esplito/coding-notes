## Advanced React Patterns

### Context Module Functions

This is overkill, but it can in some situations help you reduce duplication, improve performance and help you avoid mistakes in the dependency lists.

>🦉 Tip: You may notice that the context provider/consumers in React DevTools just display as `Context.Provider` and `Context.Consumer`. That doesn’t do a good job differentiating itself from other contexts that may be in your app. Luckily, you can set the context `displayName` and it’ll display that name for the `Provider` and `Consumer`. Hopefully in the future this will happen automatically ([learn more](https://github.com/babel/babel/issues/11241)).

**Why do we even use this pattern?**
Dodds:
> We pretty much just moved something into another function. Why is this even a pattern?
> 
> **The benefit** of this pattern is that **we can take all of this stuff and put it into a separate function**, and then pass the things that are required. Each one of these is going to require the dispatch function.
> 
> **The benefit** of doing things this way is that **when we have multiple dispatches** that we're going to be calling, if we just leave that up to the user of our context, it's very **possible that they might miss a dispatch call**. It's better to pass that user dispatch to this context module function, so it can ensure that we're calling the dispatch in the right order.

### Compound Components

Think of it like how `<select>` and `option` works. They're useless by themselves, but great together. That's how Compound Components works.

>The `<select>` is the element responsible for managing the state of the UI, and the `<option>` elements are essentially more configuration for how the select should operate (specifically, which options are available and their values).

This pattern is used in [Reach UI](https://reach.tech/) and @reach/tooltip.

Used in this exercise: 
- [React.cloneElement](https://reactjs.org/docs/react-api.html#cloneelement)
- [React.Children.map](https://reactjs.org/docs/react-api.html#reactchildren)

> You can think of React.Children.map as basically an array.map, except it works especially for React.Children because the children prop can be a single element, or it can be an array of elements.

The exercise shows how you share implicit state for compound components.

You can restrict which type of children you want to allow in your compound component by specifying allowed types and then only use `React.cloneElement()` on those:

```js
const allowedTypes = [ToggleOn, ToggleOff, ToggleButton];

return  React.Children.map(children, child  => {
  if(allowedTypes.includes(child.type)){
    return  React.cloneElement(child, {on, toggle})
    // the props 'on' and 'toggle' will only be available
    // to children that are ToggleOn, ToggleOf or ToggleButton
  }
  return  child;
})
```

### Flexible Compound Components

The compound component that we created in the previous exercise doesn't work with nested children. So we need to make that possible.

We can make this possible with `React.createContext`.

This is probably the most important use case of `useContext` according to Dodds:
>This is probably the most important use case for context where we can share this implicit state for components that we expose to people for their use without them having to worry about the state that's being managed to make these things work together.

If you want to add custom hook validation that gives a better error when a component isn't used in within the required provider, you can do this:
```js
// example custom hook with validation
function useToggle() {
  const context = React.useContext(ToggleContext);
  if (context === undefined) {
    throw  new  Error('useToggle must be used within a <Toggle /> component');
  }
  return context;
}
```

### Prop Collections and Getters

These two patterns are used in:
* [downshift](https://github.com/downshift-js/downshift) (prop getters)
* [react-table](https://github.com/tannerlinsley/react-table) (prop getters)
* [@reach/tooltip](https://reacttraining.com/reach-ui/tooltip) (prop collections)

First part of the exercise uses Prop Collections and the extra credit uses Prop Getters.

Prop getters are more flexible. Dodds prefers using prop getters over prop collections.

In the extra credit a function called callAll is used and it looks like this:
```js
// Extra credit 1: prop getters
function  callAll(...functions) {
  return (...args) => {
    functions.forEach(fn  => {
      fn  &&  fn(...args)
    })
  }
}
````
Here's how it is explained:
>What I'm going to do is I'm going to make this fancy function called callAll(). This is going to take any number of functions, and then it's going to return a function that takes any number of arguments. I don't care what those arguments are. We'll take those functions. For each of those, we'll take that function.
>
>If that function exists, then we'll call that function with all the args. Basically, it's a function that I can call passing any number of functions that will return a function that calls all those functions. Definitely, you play around with this a little bit if it's a little confusing to you, but it's pretty fantastic.
>
>What it allows me to do here now is I can say, "Give me a function that will call all of the functions I pass you onClick and toggle." It'll call onClick, and then it'll call toggle. It'll only call onClick if that function actually exists.

### State Reducer
The state reducer pattern is an implementation of the computer science principle "inversion of control". This pattern translates even better to hooks than it did to regular components. [Source: Kent C. Dodds - The State Reducer Pattern with React Hooks](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks)

### Control Props

>Control props is an implementation of something that's supported in React by default for form elements. That is the idea of controlled components. - Kent C. Dodds

If you want to add warnings to your custom hook to make sure that the control props are being used as intended, you can use the library "warning" which React uses.

Simple example of how a warning can be written:
```js
warning(false, 'this will be logged');
```

Code example for extra credit 1 within our custom hook `useToggle`:
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



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDYxMzg2NTkyLC05MDI4NDM0OTgsMTc0ND
g3MTg4OSwxNzY3MDAzNzM3LC0xNzcwOTcwMDAxLC03NTY4MDQ5
NTcsLTI5NzM2NTEyMywtMjE0NjEzNDAyNywxNjM3NzI3Nzg4LD
ExMzg4MzM2NjEsLTE5NzQ0OTQwMDQsMTg4NTU3NjU2MiwxMTIy
NzIzMzI3LC02NTk2NjAwNDQsLTMxNjIxNDMxMSwtMTk2MDY0Nj
A4NCwtMTE3NDU5MjczOSw2OTM5MDgzMjUsLTEyMDUzOTI5Mzgs
LTE2NTk2ODU0Nl19
-->