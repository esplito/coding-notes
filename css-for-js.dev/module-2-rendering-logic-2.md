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

⚠️ Beware of iframes. If you want to use this on something that is rendered inside an iframe, you'll have to select the iframe's environ ⚠️



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODIzMDk4MDM0LDExNzgwMDgwMjMsMTkyMD
Q4MzMxLDMyNTQ0NTczOSwtOTk1NzcwMDAyLDY1MDA3ODIxMCwt
MTk3NjQ4Njk3NCwtMTY4ODY5MjI4MSwtNDQwNzU0Mzc5LC0xMT
cxNzE1Nzk5LDY2Mjg2OTA1OCwxMjM1ODQ4OTc3LDQxNDAyNjYy
MCwzNDQ4NjE2NjUsMTAzNzk5MjU3MCwtMTY1ODkyMzI0OSwxMT
U4MDQ1NDUxLDE0NzA1MjgwMDQsLTI5OTE0NDUxNywxNjAyOTQy
MzUzXX0=
-->