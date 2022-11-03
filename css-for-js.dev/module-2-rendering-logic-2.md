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

🚩🚩 "As a general rule, **positioned elements will always render on top of non-positioned ones**." - Josh  🔖🔖🔖



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzNzk5MjU3MCwtMTY1ODkyMzI0OSwxMT
U4MDQ1NDUxLDE0NzA1MjgwMDQsLTI5OTE0NDUxNywxNjAyOTQy
MzUzLDEyMjYxODExNzEsLTE4MzkwMzA1MTMsLTEwMjUxNzc2ND
MsMjMzMjUwMDEzLC0xNDQxNjQyMzg4LC0yMTQwMjYzMDAsLTE2
NTk3MjIxMDUsMTE4MjExNTQ4MSwtNjQ2ODkyNDUxLC00NjExMT
IwODYsMTQzNzU5MzM2NywxMzEyNjMzMzEwXX0=
-->