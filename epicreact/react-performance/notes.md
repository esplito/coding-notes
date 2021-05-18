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

Each step in the React Profiler is a `commit`.

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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjkyNzc5ODA1LDExNDQ3Mzg1NDYsLTEwNT
QxNjA1MzgsMTgxOTAyMDQ0MywtMjExODIxOTcyMywxNjY5NDcx
ODc3LDgzNTA5NzQ2MywtMjAyNTQ4NTM2MSwtNzI4Nzk5MzA2LC
00NTc1NTM5MjUsMTU3NjE3MDYxNiwtMjEzNTExMjc3NSwtMTEx
MDA2ODQ4MiwtMTM4MjcyNzg5NywtOTQ2NTc4MjI1LC0xOTE2Nz
MzNTQyLC05NDE2MDA5MzMsMTA4MjEyODYxNywtMTQyNTQ3OTQ5
MSwxMDAyNzY0ODgzXX0=
-->