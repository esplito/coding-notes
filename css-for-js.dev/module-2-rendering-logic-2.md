# Module 2  - Rendering Logic II
🔖 = where we are right now in the course.

> In this section, we're going to look at another layout mode: _Positioned layout_.
> >
> The defining feature of positioned layout is that items can overlap. The Flow Layout algorithm tries very hard to make sure that multiple elements never occupy the same pixels. With positioned layout, however, we can break out of the box.

Default value of `position` is `static`.

```css
.box  {
  /* Revert to its default value, which is ‘static’ */
  position: initial;
}
```

## Relative Positioning

`position: relative` does two things:
1.  Constrains certain children
2.  Enables additional CSS properties to be used.

When opting into _Positioned Layout_ we get access to the following properties:
- `top`
- `bottom`
- `left`
- `right`

_Relative positioning_ moves the element without affecting the surrounding elements, whereas _margin_ affects & moves the surrounding elements.
> The big difference is that **`position`  doesn't impact layout.**

> Finally, it's important to note that relative positioning can be applied to _both block and inline elements_. This allows us to nudge inline elements in a way that isn't really possible otherwise.

## Absolute Positioning 👻

> Absolutely-positioned elements are adjusted based on their container, not based on their in-flow position.

> If we don't give our absolute element an anchor, **it sits in its default in-flow position**. I think of it as "inheriting" its default position from Flow layout.
> It causes the absolute element to stack on top of the surrounding text.

Josh says the following about absolutely positioned elements 👻:
> you can stick your hand right through them, like a hologram or a ghost.

**When is it handy?**
>Being able to take elements out-of-flow is super handy. Any time you want an element to be "floating above" the content, like a tooltip or a dropdown or a modal, absolute positioning is your friend.

## Centering Trick

You can center an element with `position: absolute`.  
Example code:
```css
.box {
  position: absolute;
  top: 0px;
  left: 0px;
  right: 0px;
  bottom: 0px;
  width: 100px;
  height: 100px;
  margin: auto;
  background: deeppink;
}
```

>There are 4 important ingredients for this trick to work:
>-   absolute positioning (`position: absolute`)
>-   Equal distances from each edge (ideally  `0px`)
>-   A fixed size (defined  `width`  and  `height`  properties)
>-   Hungry margins (`margin: auto`)

It will not be centered if the above conditions are not met.

This trick can be useful when creating, modals, drawers or dialog boxes.

## Containing Blocks

> **Absolute elements can only be contained by  _other_  elements using Positioned layout.**

This is why absolute elements behave like rebellious teenagers and move outside of their containing block (parent).  If we want them to be contained by their parent then we need to add `position: relative` to the parent.

ℹ️ By "containing block" we mean the element that surrounds our element. Example `<div class="parent"><p class="child"></p></div>`

**But how does it work? 🤔**
>When deciding where to place an absolutely-positioned element, it crawls up through the tree, looking for a Positioned ancestor. The first one it finds will provide the containing block.

**And what if it does not find one? 🫢**
> In this case, the element will be positioned according to the **“initial containing block”**. This is a box the size of the viewport, right at the top of the document.

>It doesn't matter how many parent elements are wrapping the child, it will ignore all of them until it finds a `position`. It doesn't have to be `relative`, as seen here, but it has to use Positioned layout. `absolute`, `fixed`, and `sticky` will also work.

Margins affects the position of an absolute positioned element, while padding does not.

>When you place an absolute item _without_ specifying top/left/right/bottom, it sits where it would otherwise sit in its in-flow position, but it's incorporeal, it doesn't take up any space.

## Exercises

### Bubble Border

#### Our attempt

```html
<div class="box">
  <div class="big circle"></div>
  <div class="medium circle"></div>
  <div class="small circle"></div>
</div>
```

```css
.box {
  height: 150px;
  margin: 64px;
  border: 4px solid palevioletred;
  position: relative;
}
.circle { 
  border: 4px solid palevioletred; // Josh uses inherit here
  background-color: #fff;
  border-radius: 50%;
  position: absolute;
}
// Josh wrote a more specific selector for these ".big.circle" etc.
.big { 
  width: 60px;
  height: 60px;
  left: -20px;
  top: -30px;
}
.medium {
  width: 30px;
  height: 30px;
  left: -15px;
  top: 30px;
}
.small {
  width: 26px;
  height: 26px;
  top: -14px;
  left: 50px;
}
```

### Tooltip
#### Our attempt

```html
<!--
Tooltip should:
• Be at least 150px wide
• Have a black text and border color
• Use 400 font weight
• Use 0.875rem font size
-->

<style>
  .tooltip {
    /* Hide the tooltip by default */
    display: none;
    background: white;
    border: 1px solid;
  }
  
  /*
    Show the tooltip when hovering
    over the wrapper:
  */
  .tooltip-trigger:hover .tooltip {
    display: block;
  }
</style>

<p>
  The traditional Japanese tea ceremony centers on the preparation, serving, and drinking of
  <a href="/" class="tooltip-trigger">
    matcha
    <span class="tooltip">
      A concentrated, powdered form of green tea
    </span>
  </a>
  as hot tea.
</p>
```

```css
p {
  max-width: 300px;
}

.tooltip-trigger {
  font-weight: bold;
  color: seagreen;
  text-decoration: none;
  position: relative;
  /* Josh suggest to use display: inline-block here also -> otherwise we can get weird behaviour when the trigger text is line clipped and can also cause flickering issues in browsers. */
}

.tooltip {
  position: absolute;
  width: 150px;
  font-weight: 400;
  font-size: 0.875rem;
  color: black;
  border-color: black;
  padding: 10px;
  left: 0; /* This can be removed if we set display: inline-block on the containing block (parent) */
}
```

## Stacking contexts

**How does the browser decide what to render on top?**
It depends on the layout mode! 

In **flow layout** we rarely see it, but it can be done with negative margins. 

⚠️ **Watch out!** The content of the elements are rendered separately to their respective background. This means that any content (example: text) in an underlying element will still be displayed on top. ⚠️

> There is one catch, though: in Flow layout, content is painted separately from the background.

> The truth is that Flow layout isn't really built with layering in mind. Most of the time, when we want to layer elements, we'll want to use positioned layout.

🚩🚩 "As a general rule, **positioned elements will always render on top of non-positioned ones**." - Josh

Summary:
>-   When all siblings are rendered in Flow layout, the DOM order controls how the background elements overlap, but the content will always float to the front.
>-   If one sibling uses positioned layout, it will appear above its non-positioned sibling, no matter what the DOM order is.
>-   If both siblings use positioned layout, the DOM order controls which element will be on top. Unlike in Flow layout, the content does not float to the front.

### z-index

**When do you want to use `z-index`?** 
When we want the layered order to be different from the DOM order.

**When does it work?** 
> `z-index` **only works with positioned elements*.** It will have no effect on an element being rendered in Flow layout.
> *z-index can also be used with flex/grid children;

⚠️ `z-index` does allow **negative values**, but it usually just **adds unnecessary complexity,** so Josh advices us to avoid it ⚠️

### Introducing stacking contexts

In order for something to be considered a stacking context the following needs to be fulfilled:

- `position` must be set to anything but `static`
- `z-index` must be set

Stacking contexts that are not a child of another stacking context, are compared with each other. Else, if the stacking context is a child of another stacking context, it’s compared to other stacking context within that stacking context.

Example:
```css
header {
	position: relative
	z-index: 2 /* Semver: 2.0 */
}
main {
	position: relative
	z-index: 1 /* Think of semver: this is version 1.X */
}
.main-child {
	position: absolute;
	z-index: 999; /* Semver: 1.999 */ 
}
```
```html
<header />
<main>
	<div class="main-child"/>
</main>
```

**Please note:** You can create stacking contexts in other ways too.
>-   Setting  `opacity`  to a value less than  `1`
>-   Setting  `position`  to  `fixed`  or  `sticky`  (No z-index needed for these values!)
>-   Applying a  `mix-blend-mode`  other than  `normal`
>-   Adding a  `z-index`  to a child inside a  `display: flex`  or  `display: grid`  container
>-   Using  `transform`,  `filter`,  `clip-path`, or  `perspective`
> -   Explicitly creating a context with  `isolation: isolate`  (More on this soon!)

There's a whole [list of ways to create stacking contexts over at MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context).

Josh recommends the tool ["Stacking Contexts Inspector"](https://github.com/andreadev-it/stacking-contexts-inspector) for debugging stacking contexts. It is an extension for Chrome and Firefox.

The extension answers the following questions:
>-   Which stacking context does this element belong to?
>-   Does it create a stacking context of its own?
>-   If it uses the  `z-index`  property, is it having any effect? If not, why not?
>-   Where does it sit relative to other elements in the same stacking context?

### Managing z-index

#### Strategies for managing z-index

1. **Swapping DOM order**
	- ⚠️ Be careful with this strategy. It can affect the user experience for keyboard users if you use this strategy on interactive elements like `button`. However, as long as it is only non-interactive elements like `img` it should be fine. ⚠️
2. **Isolated stacking contexts**
	- Use the  `isolation` property to create an isolated stacking context.
		- This means that you don't have to set `position` or `z-index` to isolate some stacking contexts.
		- Example usage: 
			 ```css
			.parent-that-we-want-as-a-stacking-context {
				isolation: isolate;
			} 
			```

> Ever since discovering the `isolation` property, I've been using it a ton. Whenever a child within a component applies a `z-index` value, I add `isolation: isolate` to the component's parent element. This guarantees that we won't see weird "slip-in-between" bugs, like the one we saw with the sticky header. But it doesn't contribute at all to z-index inflation, or force me to pick a value. - Josh

💡 `z-index` works without setting `position` when you are using Flexbox or Grid. 

### Portals

> Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component. - [React.js docs](https://reactjs.org/docs/portals.html)

For example, when working with modals, you might end up in a situation where you have conflicting z-index values in different stacking context. This often happen when you render a modal inside the root element created by React.
```jsx
<!-- Structure when you might have issues with stacking context -->
<html>
 <div id="root">
	 <header>My beautiful header</header>
	 <main>
		 <div id="modal" />
	 </main>
 </div>
</html>
```

```jsx
<!-- Structure when using portals. Portals are added to the bottom of the DOM structure -->
<html>
 <div id="root">
	 <header>My beautiful header</header>
	 <main>
		 <!-- Stuff here -->
	 </main>
 </div>
 <div id="modal" />
</html>
```
```css
#root {
	/* If we use this, any z-index defined inside our react app, will not affect the stacking context of the modal/portal */
	isolation: isolate; 
}
``` 

## Fixed Positioning

`position: fixed` works like `position: absolute`, but it sticks to the viewport so that it is always in the same position in the view, even when scrolling. 

ℹ️ The [centering trick](#centering-trick) that we mentioned previously, still works when using `postion: fixed`.

### Incompatibility with certain CSS properties
⚠️ If any ancestor to our "fixed" element has the `transform` , `will-change` or `filter` property defined, `position: fixed` will not work. ⚠️ 

> By applying a transform to `.container`, it becomes the containing block for this fixed-position child. As a result, it functions like an absolutely-positioned child. - Josh

Josh refers to [an article by Eric Meyer](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).  

Josh has created a snippet for detecting these culprits. Just change the selector to your fixed element and then paste the script in the console of your browser:
```js
// Replace “.the-fixed-child” for a CSS selector
// that matches the fixed-position element:
const selector = '.the-fixed-child';

function findCulprits(elem) {
    if (!elem) {
        throw new Error(
            'Could not find element with that selector'
        );
    }

    let parent = elem.parentElement;

    while (parent) {
        const {
            transform,
            willChange,
            filter,
        } = getComputedStyle(parent);

        if (
            transform !== 'none' ||
            willChange === 'transform' ||
            filter !== 'none'
        ) {
            console.warn(
                '🚨 Found a culprit! 🚨\n',
                parent, {
                    transform,
                    willChange,
                    filter
                }
            );
        }
        parent = parent.parentElement;
    }
}

findCulprits(document.querySelector(selector));
```

> Once you find the element(s) in question, there are two things you can do:
>1.  You can try to remove or replace the CSS property (eg. for  `filter: drop-shadow`, you can use  `box-shadow`  instead).
>  
>2.  If you can't change the CSS, you can  [use a portal](https://courses.joshwcomeau.com/css-for-js/02-rendering-logic-2/10-portals), like we saw in the previous lesson, or otherwise find a way to move the fixed element to a different container.

⚠️ Beware of iframes. If you want to use this on something that is rendered inside an iframe, you'll have to select the iframe's environment inside your browser. ⚠️

### Exercises

Use the snippet to find issues! 🥳

Codepen: https://codepen.io/joshwcomeau/full/KKgBmYL 

## Overflow

### Accepted values
> `overflow` defaults to `visible`, which allows an element's content to extend beyond its bounds.

#### Scroll

⚠️ `overflow: scroll` behaves differently on MacOS! By default on Mac, it will only show the scrollbar when you scroll. However on Windows & Linux, it is always shown. ⚠️

If you want to always display scrollbars on MacOS to see how it looks on Windows & Linux:
1. Go to System Preferences
2. General
3. Set "Show scroll bars" to "Always"

`overflow` is a shorthand for:
- `overflow-x`
- `overflow-y`

#### Auto

> `auto` is a smart value that adds a scrollbar when one is required.

**But why would I even use `scroll` then? 🤔** 
Because using `auto` can cause content layout shift when the scrollbar is rendered in the viewport (all content is pushed a few pixels to the side) for example when pressing a "Load more" button that triggers the scrollbar to show. It would then make more sense to always show the scrollbar (by using `scroll`) because we know that it might show up for the user.

#### Hidden

When would it be feasible to use this?
1. When truncating text with an ellipsis (...)
2. When we have decorative elements that we want to partially show.

 🚨**Josh's rule of thumb:** Always add a comment when employing this declaration. Future-you will thank you. 🚨

Example comment:
```css
.wrapper {
  /*
    On mobile, we shift the lesson-content 300px to the right,
    off-screen. I want it to be truncated, so that we only see
    a sliver of the content when the menu is open.
  */
  overflow: hidden;
}
```

### Scroll Containers

As soon as you set a value for `overflow` you create a scroll container, regardless of the value set.

> ...an element with `overflow: hidden` is literally a scroll container without scrollbars.

This means that you can tab through the overflowing content and it will scroll. (You won't see any scrollbars though and can't scroll with the mouse)

> When a child is placed in a scroll container, it _guarantees_ that the child will never spill outside of it. It's on the other side of the portal! Either a child _is_ or _isn't_ in a scroll container. We can't mix and match for vertical/horizontal.

(Josh references the TARDIS in the show *Dr. Who* when referring to "portal")

### Overflow: clip

`overflow: clip` works like `overflow: hidden`, but **it does not create a scroll container**! 🥳

> Essentially, it acts the way most developers think `overflow: hidden`  _should_ work. It trims off any overflow, in one or both directions. - Josh

🚨Does not work consistently across browsers and only arrived in Safari September 2022! 🚨 

- ⚠️ `border-radius` breaks it in Chrome (that we know of)
- ⚠️ Many iOS/MacOS users are still on older versions of Safari (in Nov 2022 20% had the needed browser version)

> `overflow: hidden` has built-in guardrails: interactive elements like links, buttons, and form inputs will be made visible if they're focused, but we lose these guardrails with `overflow: clip`. - Josh

>...scroll containers only start to scroll when the _inner size_ exceeds the _outer size_. As long as the outer size can keep on growing, that doesn't happen. - Josh

### Horizontal Overflow

To achieve horizontally scrollable elements (for example when we have a few images of cats) we can use `white-space: nowrap` and `overflow: auto`. 

> `white-space` is a property that lets us tweak how words and other inline/inline-block elements wrap. By setting it to `nowrap`, we instruct the container to never break lines. - Josh

### Positioned Layout

#### Overflow and containing blocks

> In general, absolute positioning is ignored by standard layout algorithms, and yet `overflow: auto` treats it just like any other element! - Josh

Josh recommends to have a look [at an article about how you can use this knowledge to your advantage, "Popping out Of Hidden Overflow"](https://css-tricks.com/popping-hidden-overflow/). 

#### Fixed positioning

> In order for a child to "trigger" the overflow, it needs to be contained by it. Setting `position: relative` is enough to contain an absolute child, but _fixed_ children are only ever contained by the “initial containing block”, a box that exists outside the DOM structure.

This also means that fixed-position elements are **immune** from being hidden with `overflow: hidden`. 

## Sticky Positioning

When you scroll it sticks. 🍝

> In addition to setting `position: sticky`, you also need to pick at least one edge to stick to (top, left, right, bottom). Most commonly, this is done with `top: 0px` (or `top: 0`; the unit is optional when it's zero). - Josh

### Stays in their box
> An often-overlooked aspect of `position: sticky` is that the element will never follow the scroll outside of its parent container. Sticky elements only stick while their container is in view. - Josh

Josh also showcases an interesting way of having content headings that sticks as long as the adjacent content is in view.

### Offset

-   With **relative positioning**, the element is shifted from its natural, in-flow position
    
-   With **absolute positioning**, the element is distanced from its containing block's edge
    
-   With **fixed positioning**, the element is adjusted based on the viewport
    
- With **sticky positioning**, the value controls the  **minimum gap between the element and the edge of the viewport**  while the container is in-frame.

For sticky positioning: `top` has no effect until we start scrolling.

###  Not incorporeal

Sticky positioning behaves a bit like static & relative, in the way that it takes up space **in-flow**. 

### Horizontal stickiness
You can also use `sticky` to stick an element when horizontally scrolling.

### Sticky positioning and browser support
🥳🥳🥳🥳 Works across all major browsers on both mobile and desktop 🥳🥳🥳🥳 

### Exercises

#### 1. Sticky header
In this exercise we got the task to create a sticky header that has a bit of a cushion before it sticks to the top of the page on scroll.

##### Solution
```html
<style>
  header {
    height: 66px;
    background: slateblue;
    color: white;
    opacity: 0.96;
    position: sticky;
    top: -16px;
    padding-top: 16px;
  }
</style>

<header>
  <ul>
    <li>Home</li>
    <li>About</li>
    <li>Contact</li>
  </ul>
</header>

<main>
  <p>Hello world!</p>
</main>
```

#### 2. Fix the bug
In this task there's a pink box that should stick to the top on scroll, but it does not. We have to fix it! 🛠

##### Solution
```html
<header>
  <div class="sticky"></div>
</header>
<main>
  <p>
    Lorem ipsum dolor sit amet,
    consectetur adipiscing elit, sed do
    eiusmod tempor incididunt ut labore
    et dolore magna aliqua. Ut enim ad
    minim veniam, quis nostrud
    exercitation ullamco laboris nisi ut
    aliquip ex ea commodo consequat.
    Duis aute irure dolor in
    reprehenderit in voluptate velit
    esse cillum dolore eu fugiat nulla
    pariatur. Excepteur sint occaecat
    cupidatat non proident, sunt in
    culpa qui officia deserunt mollit
    anim id est laborum.
  </p>
</main>
```

```css
/* Baseline decorative styles: */
html {
  height: 100%;
}
body {
  height: 150%;
  padding: 0;
}
header {
  position: sticky;
  top: -16px;
}
header,
main {
  padding: 16px;
}
.sticky {
  width: 50px;
  height: 50px;
  background: deeppink;
  border-radius: 4px;
}
```
### Troubleshooting
> Unfortunately, it's very common to apply `position: sticky` to an element, only for nothing to happen; the element won't stick! - Josh

#### Common pitfalls
- A parent is hiding/managing overflow
	- For example when it creates its own scroll container
	- It does not have to be a direct parent.
	- > Here's how to think about it: `position: sticky` can only stick in one "context". If it's within a scroll container, it can only stick within that container. - Josh
- 

#### Script for detecting sticky-issue culprits 
Paste it in the browser devTools!
```js
// Replace “.the-sticky-child” for a CSS selector
// that matches the sticky-position element:
const selector = '.the-sticky-child';

function findCulprits(elem) {
    if (!elem) {
        throw new Error(
            'Could not find element with that selector'
        );
    }

    let parent = elem.parentElement;

    while (parent) {
        const {
            overflow
        } = getComputedStyle(parent);

        if (['auto', 'scroll', 'hidden'].includes(overflow)) {
            console.log(overflow, parent);
        }

        parent = parent.parentElement;
    }
}

findCulprits(document.querySelector(selector));
```
**How do we fix it?**
> -   If the culprit uses  `overflow: hidden`, we can switch to  `overflow: clip`. Because  `overflow: clip`  doesn't create a scroll container, it doesn't have this problem!
>    
>-   If the culprit uses  `auto`  or  `scroll`, you  _might_  be able to remove this property, or push it lower in the DOM. This is a tricky problem, often without a quick solution. We saw an example of this sort of restructuring in the  [solution to the last exercise](https://courses.joshwcomeau.com/css-for-js/02-rendering-logic-2/16-sticky-exercises#digit-fix-the-bug). - Josh





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzNTkyNzE0NSwxMTMyMDQ3NTIyLC0xOT
IyMDAzNjM2LC0zMjkyNDYxNTQsODk4OTU3Nzk3LDcyODk1ODQ0
MywtNTU0Mzc4MTk5LC0xNjEyNzAwMjQ2LC01NTI1OTc2ODUsMT
k5ODA0NDg0MSw4Mjg0NTY2NTUsMTYwMDg5OTM4Nyw5MTY1NTU4
ODQsNTU0NDk3MzcsMTAwODMyMjkzNSwtMTY5NDgwMjI5Nyw2Nz
cwMjUwMzAsLTc0OTQ2MTgzNSwyMDM2MjE2ODIxLC04Mzc1MDc4
MDVdfQ==
-->