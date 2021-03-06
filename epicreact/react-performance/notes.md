## React Performance

### Welcome

> There are a couple of examples that will be useful to you in here, to get an idea of how a different files work together, when we're doing lazy loading or code splitting, lazy loading of this code. You'll be playing around in there a little bit, but most of your time will be spent in these exercises.
> 
> Actually, lots of these exercises don't require a lot of code changes. What you'll actually be doing is spending a lot of your time in the Network tab, the Profiler, and the Performance tab. You may or may not be very familiar with these tools and that's totally fine. - Dodds

### Code Splitting

The thinking behind code splitting is that loading less code will speed up your application.

> The idea behind the code splitting is that it allows you to lazily load your code at the time that it's needed. - Dodds

Example: your application always loads some massive chart that uses an external library and this chart is only displayed on a specific page when you navigate to it. -> With code splitting this wouldn't have to be loaded on first load of the app and would instead be lazily loaded in the browser when the user navigates to the page that uses this chart.

Lazy loading something can be done with "dynamic imports" which is a built-in way of lazy loading Javascript. More reading about dynamic imports: https://kentcdodds.com/blog/super-simple-start-to-es-modules-in-the-browser

When importing a javascript module using ESModules, we have to specify a path to the javascript file that we want to import (with the .js-extension, unless it's on like unpkg or another external url). So we can't omit the extension (when we use it directly in plain javascript + html) like we are used to in React, etc. 

> 🦉 One great way to analyze your app to determine the need/benefit of code splitting for a certain feature/page/interaction, is to use [the “Coverage” feature of the developer tools](https://developers.google.com/web/tools/chrome-devtools/coverage).

**Exercise one:** The test is passing from the beginning when it shouldn't. I created a PR to fix this: https://github.com/kentcdodds/react-performance/pull/83/commits/d0ebad574e0528ae46483a2b73560eaa1319dcf8

**Extra credit 1: Eager loading**
We can do "eager loading", which in the exercise means that we will start loading the content even before the user has clicked the checkbox "show globe". We do this by creating a function that we can use to lazy load our component when a user focuses the checkbox or when the user hovers the label. 

```js
// Extra credit 1. Eager loading
const loadGlobe = () => import("../globe"); 
const Globe = React.lazy(loadGlobe);
```
```jsx
// Label and input in our React component
<label
  style={{marginBottom: '1rem'}}
  onMouseEnter={loadGlobe}
  onFocus={loadGlobe}
>
  <input
    type="checkbox"
    checked={showGlobe}
    onChange={e => setShowGlobe(e.target.checked)}
  />
  {' show globe'}
</label>
```

**Extra credit 2: webpack magic comments**
>`webpackPrefetch`: Tells the browser that the resource is probably needed for some navigation in the future. 
>Source: [Webpack Docs - Module Methods](https://webpack.js.org/api/module-methods/#magic-comments)

We can use `webpackPrefetch` (which is a webpack magic comment) to make the javascript file automatically load it into the browser cache when the browser is idle.

More about prefetching here: https://webpack.js.org/guides/code-splitting/#prefetchingpreloading-modules

**Suspense Position / DevTools**
> If you've used the error boundary before, that's exactly the same way that the error boundary behaves is the entire children is managed by that suspense-boundary.
> 
> The nice thing about this is that if any of these other components suspend like the globe is, they will also be managed by the same suspense-boundary. We probably want to locate that suspense-boundary closer to where it's relevant to provide a more useful fallback. - Dodds

There's a way to check the fallback UI of `React.Suspense`, by  selecting the Suspense-component in the "Components"-DevTools and pressing the stopwatch.

**Coverage Tool**

You either use the command tool in Google Chrome to open up the "Coverage"-tab or open up the console and press the three dots and choose coverage.

The red bar is all of the unused code that we have in the different files.

> It's important when you're looking at actual numbers and trying to compare and contrast that you're doing that comparison with all of the production optimizations that you have set up. I should also note that you can do this all locally as well. There's no reason that you have to do it in production. You could run your build and then serve it locally.
> 
> Just try to get what you're actually testing to be as close to the end user's experience as possible so you can get more real numbers.  - Dodds

### useMemo for Expensive Calculations

> If you've got an expensive calculation to make in the function body of your component, then useMemo is the hero you need. - Dodds

**Profiling**
We are using the "Performance"-tab in Chrome. Set the CPU to 6x slowdown -> Hit Record -> Do something -> Press "Stop". Then you can se some stats and screenshots.

You can see "Timings" which is executed by React.

**Wrapping a function in useMemo**
If we have an expensive calculation (some kind of function) inside our component, it can lead to performance bottlenecks. This is because the calculation will be triggered on every render. So any time the component re-renders (like when a parent re-renders or some state changes) the function will be called. 

Example from the course:
```jsx
function Distance({x, y}) {
  const distance = calculateDistance(x, y)
  return (
    <div>
      The distance between {x} and {y} is {distance}.
    </div>
  )
}
```

We can then wrap the function with `useMemo` so that the function only will be called when the dependencies change. (This would be if `x` or `y` changes in the example)

Example from the course:
```jsx
function Distance({x, y}) {
  const distance = React.useMemo(() => calculateDistance(x, y), [x, y])
  return (
    <div>
      The distance between {x} and {y} is {distance}.
    </div>
  )
}
```

**Note: You can no longer check timings from React in the Performance tab. You have to use the Profiler to see React-specific stuff or check inside "main" in dev-mode.** 
Sources:  
 - https://stackoverflow.com/questions/52364829/no-timing-breakdown-in-user-timing-section-in-performance-tab-in-chrome-dev-tool
 -  https://github.com/facebook/react/pull/18417

**Extra credit 1: Production Mode**
> If you want to have 60 frames a second, which is what we're going for so we can have a nice smooth experience for the human eye, then you need to nail 16 frames a second.
> 
> That 1,000 divided by 60 frames a second. That's going to be 16 milliseconds, thereabouts, for your JavaScript to run so that the browser can keep up and not drop any frames and resulting in a janky experience. - Dodds

**Extra credit 2: Put getItems into a Web Worker**

Javascript is single-threaded. You want it on another thread? -> Use Web Workers!

You can for example use the [workerize-loader](https://github.com/developit/workerize-loader) in webpack. Note: The communication between the main thread and web workers is **asynchronous**. 

However, workerize-loader doesn't support Webpack 5 yet: https://github.com/developit/workerize-loader/issues/77 

Here's a comparison of the fps and execution time when only using `useMemo` and when using a web worker. (in prod build with 6x CPU)

|  | useMemo | Web Worker |
|--|--|--|
| **Execution time** | 23.5 ms | 12.4 ms |
| **FPS** | 43 fps | 81 fps |

Screenshot useMemo:
![Screenshot of the performance tab when clicking "force re-render" in example app](https://github.com/esplito/coding-notes/blob/master/with_usememo_no_web_worker_6x-slowdown.png?raw=true)

Screenshot using Web Worker:
![Screenshot of the performance tab when clicking "force re-render" in example app](https://github.com/esplito/coding-notes/blob/master/with_web_worker_6x-slowdown.png?raw=true)

### React.memo for Reducing re-renders

```
Lifecycle of react app:

→  render → reconciliation → commit
         ↖                   ↙
              state change
```

>-   The “render” phase: create React elements React.createElement
>-   The “reconciliation” phase: compare previous elements with the new ones
>-   The “commit” phase: update the DOM (if needed).

>A React Component can re-render for any of the following reasons:
>1.  Its props change
>2.  Its internal state changes
>3.  It is consuming context values which have changed
>4.  Its parent re-renders

> We want to give little hints to React to say, "Hey, I actually don't need to re-render."
> 
> The reason that's necessary is that sometimes there's some state change that happens that triggers a re-render of a component when that component actually doesn't have any updates to the DOM to make. That's what we call an unnecessary re-render. - Dodds

Each step in the [React Profiler](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html) is a `commit`.

Remember that sometimes the re-rendering isn't the actual performance problem. It can be something else that is causing the issue, so don't just throw `React.memo` on everything that you see. More about this in Kent's post here: https://kentcdodds.com/blog/fix-the-slow-render-before-you-fix-the-re-render

> Again, I want to mention that people can make the mistake of wrapping _everything_ in `React.memo` which can actually slow down your app in some cases and in all cases it makes your code more complex. - Dodds

If you want to see why a component rendered, you can enable `"Record why each component rendered while profiling"` in `Profiler -> Settings -> Profiler`. When you have profiled something and click on a component you'll see "Why did this render?" and an explanation to why it was rendered.

When you use `React.memo` on a component it will look like this if the component wasn't re-rendered at a certain commit:

![Screenshot of React Profiler where the NameInput component is memoized and not re-rendered](https://github.com/esplito/coding-notes/blob/master/react-memo_component_not_rerendering.png?raw=true)

In the example above, we clicked the `CountButton` and since we had used `React.memo` on `NameInput` this didn't re-rendered. (It re-rendered before, because the parent re-renders when we press the `CountButton`)

**Exercise**

**CPU Throttling:** 6x slowdown
Press "force rerender" while profiling.

We try and optimize this by putting the `Menu`-component and the `ListItem`-component in `React.memo`.

| Optimization -> | Before | After |
|--|--|--|
|**Render duration** | 56.9 ms | 9.5 ms |

**Extra credit 1: Use a custom comparator function**
Sometimes you want to decide when rendering the component again is unnecessary. You can do this by passing a second argument to `React.memo`. This argument is a custom compare function where we can compare props and return either `true` or `false`:
 
`true` = rendering component again is **unnecessary**
`false` = rendering component again is **necessary**

Example solution from the exercise:
```js
ListItem = React.memo(ListItem, (prevProps, nextProps) => {
  // true means do NOT rerender
  // false means DO rerender
  
  // these ones are easy if any of these changed, we should re-render
  if (prevProps.getItemProps !== nextProps.getItemProps) return false;
  if (prevProps.item !== nextProps.item) return false;
  if (prevProps.index !== nextProps.index) return false;
  if (prevProps.selectedItem !== nextProps.selectedItem) return false; 
  
  // this is trickier. We should only re-render if this list item:
  // 1. was highlighted before and now it's not
  // 2. was not highlighted before and now it is
  if (prevProps.highlightedIndex !== nextProps.highlightedIndex) {
    const wasPrevHighlighted = prevProps.highlightedIndex === prevProps.index;
    const isNowHighlighted = nextProps.highlightedIndex === nextProps.index;
    return wasPrevHighlighted === isNowHighlighted;
  }

  return true;
});
```

> You want to **be careful when you're doing these kinds of optimizations** and **ensure that you're reaping the benefits** that you're looking for out of this, which is why we're bringing up this performance profiler in production mode would be handy so you can compare the before and after and **see if it's worth the complexity that you've added to your codebase**. - Dodds

**Extra credit 2: pass only primitive values**
> Wouldn’t it be even better to not have to provide our custom memoization comparator function and still get the perf gains? Definitely! - Dodds

To do this we have to pass some values that are pre-computed. In the example exercise we will instead figure out `isSelected` and `isHighlighted` when we pass them as props to `<ListItem>`. 

Example code:
```jsx
{items.map((item, index) => (
  <ListItem
    key={item.id}
    getItemProps={getItemProps}
    item={item}
    index={index}
    isSelected={selectedItem?.id === item.id} // Extra credit 2: pass only primitive values
    isHighlighted={highlightedIndex === index} // Extra credit 2: pass only primitive values
  >
    {item.name}
  </ListItem>
))}
```
This means that they now are primitive values inside `<ListItem>` and React's built-in comparison function will kick in. This also means that only the selected item needs a re-render when you select an item.

>The takeaway here is if you have a particular component and you're rendering a ton of instances of those, try to do calculations a little higher in the tree, so you're only passing primitive values that when changed, will trigger a DOM update. That way, you don't worry about breaking memoization or having to create a custom comparator. - Dodds

### Window Large Lists with react-virtual

> Mostly, what I want you to get out of this exercise is the understanding that just because you have a ton of stuff that you need to display to the user doesn't mean that you have to render all of the stuff at the same time if it's not all actually in the visible space for the user to interact with. - Dodds

In this exercise we use a library called [react-virtual](https://github.com/tannerlinsley/react-virtual) to be able to do "windowing". This means that we will only render the items visible on the screen + some additional items so that when a user scrolls, they will not notice that we are loading in more items as we scroll through the list.

See an example here: https://react-performance.netlify.app/4

### Optimize context value

> The way that context works is that whenever the provided value changes from one render to another, it triggers a re-render of all the consuming components (which will re-render whether or not they’re memoized). - Dodds

More info about this here: https://github.com/kentcdodds/kentcdodds.com/blob/319db97260078ea4c263e75166f05e2cea21ccd1/content/blog/how-to-optimize-your-context-value/index.md 

**Exercise**

Stats when clicking "force rerender"-button before and after optimization with `useMemo`. (No throttling)
| | Before | After |
|--|--|--|
|Click duration|70.7 ms|6.2 ms|
|Click fps|14 fps| 160 fps|
|Render duration|115.9 ms|0.3 ms|

> Why are we getting a re-render when the context value has values that don't change? As it happens, the value prop that you pass to the provider is going to be compared shallowly with object.is or basically a ===.
> When this app provider is re-rendered, then this value gets re-created. It's a brand-new array. When React compares the value we're passing to this provider between the last render and the new one, it says, "Hey, you gave me a different array last time." - Dodds

**Extra credit 1:  separate the contexts**
When we click on a cell in the grid, the whole grid re-renders even though the click doesn't really affect the whole grid and even though we have memoized the grid (it is not dependent on the state change and only needs the dispatch). We'll have to separate the contexts so that we have one provider for the state and one provider for the dispatch.

Render duration stats before/after optimization.
| | Before | After |
|--|--|--|
|Render duration|61.3 ms|59.4 ms|

Code before: https://github.com/kentcdodds/react-performance/blob/main/src/exercise/05.js  

Final solution here: https://github.com/kentcdodds/react-performance/blob/main/src/final/05.extra-1.js 

### Fix Perf Death by a Thousand Cuts
> Whatever state management solution you’re using, often you can run into a problem that I call "perf death by a thousand cuts" which basically means that so many components are updated when state changes that it becomes a performance bottleneck. - Dodds

Perf death by a thousand cuts doesn't give you an obvious place to solve the performance problem. This is because the problem occurs when lots of components need to run because of a state update. The components are not slow in isolation.

**How can we solve this?**
* Every performance problem is solved by less code.
* We often have components responding to a state change when they don't need to.
* Should we memoize the components with `React.memo`? **No.**
	* Why not?
		* Increases complexity of our application. (because we need to use `useCallback` and `useMemo` for basically everything to get some performance boost from this)
		* React will still have to do lots of work to check whether the components should be re-rendered.

**Solution**:  *State colocation!*
This means that we put **less state in the global store** and instead put it where it is actually used. 

**Benefits**
✅ Improved performance
✅ Improved maintenance

More on state colocation in Kent's blog post about the topic: [State Colocation will make your React app faster](https://kcd.im/colocate-state).

**Extra credit 1: separate contexts**
If we want to have the `dogName` accessible globally, but still want the same performance as we had with the colocated state, we would have to create a separate context for dog. (Like we did the in the extra credit 1 of the previous exercise.)

Code example here: https://github.com/kentcdodds/react-performance/blob/main/src/final/06.extra-1.js

**Extra credit 2: consuming components**
Another issue with the grid in this exercise is that the cells are re-rendering when we only click one of the cells. This affects performance so we would want to minimize this. One way of doing it, is to create a "middle-man" component which will handle the state changes of a cell. By having this component we'll only re-render the "middle-man" components instead of re-rendering all the Cell-components. This leads to an improvement in performance. 

Example code here: https://github.com/kentcdodds/react-performance/blob/main/src/final/06.extra-2.js

**Extra credit 3: write an HOC to get a slice of app state**
However, we can make the solution more generic by creating a Higher Order Component (HOC). 

>NOTE: this is effectively what react-redux’s `connect` higher order component does.

This makes it easier to do the previous optimization in more places!

Example code here: https://github.com/kentcdodds/react-performance/blob/main/src/final/06.extra-3.js 

> To take this a step further, what if somebody wanted to pass a ref to this wrapper component or to the underlying cell component, and we wanted to be able to accept that ref and do something with it?
> 
> In that case, we're going to need to use React.forwardRef, and then we'll be able to accept that ref, and we can forward that ref right along. This is a pretty nice higher-order component for which we can apply this kind of pattern anytime we want to get a specific slice of the state, and we can take advantage of memoization. 
> 
> Now, if I hit the Profiler and go, then we're solid. We are only rendering the components that actually care about this update. - Dodds

After this, the cell only re-renders when the slice of state that it cares about, is changing!

More reading about HOC and forwarding refs:
* https://reactjs.org/docs/higher-order-components.html
* https://reactjs.org/docs/forwarding-refs.html

**Extra credit 4: Use recoil**
Situations were you need to apply these types of optimizations are rare!

The state-management library, [recoil](https://recoiljs.org/) is used in this extra credit.

About this extra credit:
> The point is to teach you what’s possible with recoil and the kinds of problems it’s well suited for so you know when to reach for it in your own applications (definitely don’t use it for everything). - Dodds

### Production Performance Monitoring

There's an API for monitoring the performance of a React application in production. It is called Profiler API. It also has an API for tracing interactions but it is still experimental.

It isn't enabled by default because it gives our app a slight performance penalty when monitoring. 

> Companies like Facebook built their application both with profilingEnabled and with profilingDisabled, and then only serve the profilingEnabled build to a small portion of their users to get this performance monitoring, and you could absolutely do something like that yourself. - Dodds

We can use this to send info to Grafana or similar tools.

More info about the api here: 
* https://gist.github.com/bvaughn/8de925562903afd2e7a12554adcdda16 
* https://reactjs.org/docs/profiler.html

To see an example of how you could pass something to the Profiler-component's `onRender` prop, check out `reportProfile` in this file: https://github.com/kentcdodds/react-performance/blob/main/src/report-profile.js

More about this topic in Kent's blog posts:
* [React Production Performance Monitoring](https://kentcdodds.com/blog/react-production-performance-monitoring)
* [Profile a React App for Performance](https://kentcdodds.com/blog/profile-a-react-app-for-performance)

Check the console after clicking the "profiled counter" to see what we pass along from our profiler in the following examples:
* https://react-performance.netlify.app/isolated/final/07.js
* https://react-performance.netlify.app/isolated/final/07.extra-1.js (with tracing of interactions enabled)

> Written with [StackEdit](https://stackedit.io/).

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyNDYxMDU3NCwyNzA3MzI5NDEsLTEwMz
UzNTY1NDksLTU0MzQ2NTExNywtMTcyMDQ2ODkzLC00NzU3NDcy
MDEsMTM2MDIwMDQ4MiwtODY4MzM5MjIyLC0zNzIyNTQ0MjMsOD
Y5ODU5NDU0LC05MDM1OTk4NTIsMTYyNTMyNTEzMSwtOTY1MDk0
MTgxLC0zNjcxMTU0MDEsLTkxMDgzMjAxNCwtMTkzNjA3MDUzMC
wxMzE3MTE1MzM2LDIwNDQ3ODgyMjcsLTE1OTY3ODQzMTgsMjA0
Mjg5MjAyNF19
-->