
## React Fundamentals

### Prerequisites
- Read through [https://kentcdodds.com/blog/javascript-to-know-for-react](https://kentcdodds.com/blog/javascript-to-know-for-react)
- Read about [Javascript Closures](https://whatthefork.is/closure)
- Install React DevTools

### Helpful Emoji 🐨 💪 🏁 💰 💯 🦉 📜 💣 👨‍💼 🚨

Each exercise has comments in it to help you get through the exercise. These fun emoji characters are here to help you.

-   **Kody the Koala**  🐨 will tell you when there's something specific you should do
-   **Matthew the Muscle**  💪 will indicate what you're working with an exercise
-   **Chuck the Checkered Flag**  🏁 will indicate that you're working with a final version
-   **Marty the Money Bag**  💰 will give you specific tips (and sometimes code) along the way
-   **Hannah the Hundred**  💯 will give you extra challenges you can do if you finish the exercises early.
-   **Olivia the Owl**  🦉 will give you useful tidbits/best practice notes and a link for elaboration and feedback.
-   **Dominic the Document**  📜 will give you links to useful documentation
-   **Berry the Bomb**  💣 will be hanging around anywhere you need to blow stuff up (delete code)
-   **Peter the Product Manager**  👨‍💼 helps us know what our users want
-   **Alfred the Alert**  🚨 will occasionally show up in the test failures with potential explanations for why the tests are failing.

### Basic JS "Hello World"
Relevant resources:
 
[MDN - ParentNode append](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append)
[MDN - Intro to the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

### Intro to Raw React APIs

Core React can be used for both Native and DOM.

Useful resources:
* [Imperative vs Declarative Programming](https://ui.dev/imperative-vs-declarative-programming/)

### Using JSX
You can play around here with how [JSX gets compiled into Javascript](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=usage&spec=false&loose=false&code_lz=MYewdgzgLgBArgSxgXhgHgCYIG4D40QAOAhmLgBICmANtSGgPRGm7rNkDqIATtRo-3wMseAFBA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=true&targets=&version=7.13.14&externalPlugins=).

You can find the differences between JSX and HTML syntax in the [React Docs](https://reactjs.org/docs/dom-elements.html#differences-in-attributes).

Keep in mind that you can't do statements inside the curly braces like this:

```jsx
const element = (
  <div id="string" className={myClassName.toUppercase()}>
    {
      if(greeting !== 'hello'){
        // do something
      }
    }
  </div>
);
```

You can do ternaries however:
```jsx
const element = (
  <div id="string" className={myClassName.toUppercase()}>
      {greeting !== 'hello' ? greeting : '';}
  </div>
);
```

The order of attributes on an element matters:
```jsx
const className = 'container';
const children = 'Hello World';
const props = {children, className};

// className will be overriden to "container"
const element = <div id="something" className="default" {...props}/>;

// className will be overriden to "default"
const element = <div id="something" {...props} className="default"/>;
```

### Creating custom components

Useful links: 
* [](https://kentcdodds.com/blog/what-is-jsx)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwOTMxMDg2MSwxNzYyOTEwNjY0LDE1MT
Y0NDAyNzIsMTY5MDI2NTU5Niw1MjAwMjk4MTYsLTg4MzkxMTgz
MiwyMTAxMDE1ODA4LDE2MzA0ODY5NDksNjA0OTk5NjI1LDEzMD
AyODIyNDIsLTE1MTYwMjMzNzYsMTIyMjAyNTE2OCw2MTkxMTY2
MjEsMTUzMzU2ODA5NV19
-->