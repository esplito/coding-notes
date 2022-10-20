# Module 2  - Rendering Logic II

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


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwMjQwNTAxOSwtMTQ0MTY0MjM4OCwtMj
E0MDI2MzAwLC0xNjU5NzIyMTA1LDExODIxMTU0ODEsLTY0Njg5
MjQ1MSwtNDYxMTEyMDg2LDE0Mzc1OTMzNjcsMTMxMjYzMzMxMF
19
-->