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


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NjgxNzI3MTYsLTEwOTM0NTM4MjAsLT
kwNDQzMTkyMCwtMTYwMDA5OTg4NSwtNzI2MzQ2NzE3LC0xMDAw
NTAyMTcxLDE0NDgzNjI2MTEsLTExODY1MTU1NzMsLTk3MTI3OD
U2OCwtMTUxOTMwMDU1MCwtMTEzMzU4MDAzMiwtMTY2OTYxMzQ4
MCwtNTM0Njg0NTgxXX0=
-->