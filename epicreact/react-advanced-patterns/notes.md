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
The state reducer pattern is an implementation of the computer science principle "inversion of control". This pa


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MDU5OTgxMSwtMjk3MzY1MTIzLC0yMT
Q2MTM0MDI3LDE2Mzc3Mjc3ODgsMTEzODgzMzY2MSwtMTk3NDQ5
NDAwNCwxODg1NTc2NTYyLDExMjI3MjMzMjcsLTY1OTY2MDA0NC
wtMzE2MjE0MzExLC0xOTYwNjQ2MDg0LC0xMTc0NTkyNzM5LDY5
MzkwODMyNSwtMTIwNTM5MjkzOCwtMTY1OTY4NTQ2LC03MDkyOD
UzMDcsMTU3NTY4ODAzNSwtMTI1OTk0MDI0Niw2NDI0NDE4NjEs
LTQyMTMyMDYyXX0=
-->