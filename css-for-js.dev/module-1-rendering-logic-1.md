
# Module 1  - Rendering Logic I

## The Box Model

### Winter Layers
-   The  _content_  is the person themselves, the human being inside the coat.
    
-   The  _padding_  is the polyester stuffing in the coat. The more stuffing there is, the more poofed-up the coat will be, and the more space the person will take up.
    
-   The  _border_  is the material of the coat. It has a thickness and a color, and it affects the person's appearance.
    
-   The  _margin_  is the person's “personal space”. As we've learned in recent years, it's good to have 2 meters (6 feet) of space around us.

#### Box Sizing
| box-sizing | Behavior  |
|--|--|
|  content-box| width is only the content of the box. excluding padding and border. |
|  border-box| width includes also padding and border. |

#### A new default
You can use the following to default to `border-box`.

```css
*,
*::before,
*::after  {
  box-sizing: border-box;
}
```

### Padding

#### Units

> Many developers believe that pixels are bad for accessibility. This is true when it comes to _font size_, but I actually think pixels are the best unit to use for padding (and other box model properties like margin/border). - Josh W. Comeau

Shorthand padding visualized

![Illustration of a clock, with labeled cardinal directions](https://courses.joshwcomeau.com/cfj-mats/shorthand-clock.png)

This also applies for `border-style`:

border-style (`border-style: dotted solid dashed solid`)

### Border

> The only required field is `border-style`. Without it, no border will be shown!
```css
.not-good  {

/* 🙅‍♀️ Won't work – needs a style! */

border: 2px pink;

}

.good  {

/* 🙆‍♀️ Will produce a black, 3px-thick border */

border: solid;

}
```

>If we don't specify a border color, _it'll use the font's color by default_. This isn't well-known, but it can be useful in cases where those things should be synchronized!

> If you want to specify this behaviour explicitly, it can be done with the special `currentColor` keyword.

#### Border vs Outline

_outline doesn't affect layout_. -> _border does_.

>Outlines are stacked outside border, and can sometimes be used as a "second border".

>A couple more quick tidbits about outlines:

>-   Outlines will follow the curve set with  `border-radius`  in all browsers except Safari. This is a recent change; before September 2021, most browsers kept outlines straight and boxy.
    
>-   **Outlines have a special  `outline-offset`  property**. It allows you to add a bit of a gap between the element and its outline.

Never set the following due to a11y issues:
```css
button {
  outline: none;
}
```
You could however set your own focus style with `:focus`, but ensure that you have high enough contrast!!! 

### Margin

>  ℹ️ You can use negative margins both for **pulling an element outside its parent** AND **pulling an element's sibling closer**!

**Please note:** Negative margin can affect the position of _all siblings_.  This is different compared to `transform: translate` which only affects the element that the style is being applied to.

Article about negative margins: http://www.quirksmode.org/blog/archives/2020/02/negative_margin.html

Using the `margin-left` + `margin-right` `auto` trick, only works horizontally and the element needs an explicit width.

If you do it on `top` and `bottom` it will be set to `0px`

## Flow Layout

### Inline doesn't want to make a fuss

There's some inline elements that are special. They are called *replaced elements*:
1. `<img />`
2. `<video />`
3. `<canvas />`

They can actually *affect the block layout*, for example with `margin-top`.

> I like to pretend that it's a foreign object within an inline wrapper.

There's another one! `<button />`.

> They aren't _quite_ replaced elements, but they function the same way.

>All three elements (`img`, `span`, `button`) are inline elements, and they've all been given `margin-top`. In general, we don't expect this property to have any effect, and it doesn't on the `span`! But both the `img` and the `button` have the top margin applied, because they're special elements.

### Block elements don't share

A block level element greedily expands to the full width of the container.

>What if we force it to shrink down to the minimum size required for the letters? We can do this with the special width keyword `fit-content*:`

🚨 In Firefox, however, `fit-content` requires a vendor prefix: `-moz-fit-content` 🚨

The browser treats inline elements as typography and therefore adds some pixels (extra magic space 🧙‍♀️) below the element.

```html
<div class="outer">
	<img src="cat.jpg" />
	<!-- HERE IS SOME MAGIC SPACE 🧙‍♀️🪄 -->
</div>
```

This could for example happen when using images without adding `display: block`. Another way to solve it would be to add `line-height: 0` to the outer div.

### Space between inline elements

![image of img-elements with whitespace in the rendered html](https://github.com/esplito/coding-notes/blob/master/whitespace_images.png?raw=true)

>This space is caused by the **whitespace between elements**. If we squish our HTML so that there are no newlines or whitespace characters between images, this problem goes away

### Inline elements can line-wrap

>It's like a sushi roll. It starts as a long strip, but it's cut into slices before being presented.


####  Tweaking this default behaviour

Using horizontal padding (e.g. `padding-left , padding-right`) on inline elements only adds padding at the tips, for example at the beginning and at the end of a sentence.

This can be changed with the following css properties:
```css
 /* Our obscure magic trick: */
  -webkit-box-decoration-break: clone;
  box-decoration-break: clone;
```
It adds padding to each line instead of just at the tips. The lines are now treated as several boxes instead of just one long box!

> As with `fit-content`, we can rely on pre-processors to add the `-webkit` vendor prefix in most situations.

### The deal with inline-block

> Essentially, inline-block allows you to drop a block element into an inline context. It's a block in inline's clothing.

> Another way to phrase this: it's an element that _internally_ acts like a block element, but _externally_ acts like an inline element. The parent container will treat it as an inline element, since it's external. But the element itself can be styled like a block.

The catch with using it is that it **does not line-wrap**, which is not good when using it on links in paragraphs.


### Width algorithms

Block-elements have `width: auto`  **not** `width: 100%`.

> It's a subtle but important distinction: by default, block elements have _dynamic sizing_. They're context-aware.

> `auto` will let our element greedily consume the available space while respecting any constraints. 

#### Keyword values

We can specify two kinds of values for `width`:

1. Measurements
	2. 100%, 200px, 3rem...
2. Keywords
	3. auto, fit-content

#### min-content

- Intrinsic values 👼 => based on the child contents
- Extrinsic values  👩‍👦‍👦 => based on space made available by the parent

For text it takes every opportunity it can to wrap the text and uses the width of the longest word.

#### max-content

`max-content` does the _opposite_! It _never_ adds any line breaks.

Possible use case:
> Unlike with `auto`, `max-content` doesn't fill the available space. If we want to add a background color _only_ around the letters, this would be a neat way to do it!

#### fit-content

`fit-content` is the magic keyword. It uses the best of both worlds. It acts like `max-content`,  but adds line-breaks and also behaves like `auto` to not exceed the available space.

### Exercises

#### Max width wrapper

```html
<style>
  .max-width-wrapper {
    max-width: 350px; // constraint width on bigger devices
    margin-left: auto; // centering
    margin-right: auto; // centering
    padding-left: 16px; // add breathing room on smaller devices
    padding-right: 16px; // add breathing room on smaller devices
  }
  
</style>

<div class="max-width-wrapper">
	<div class="card">
	  <p>
	    Otters have long, slim bodies and relatively short limbs. Their most striking anatomical features are the powerful webbed feet used to swim, and their seal-like abilities holding breath underwater.
	  </p>
	</div>
</div>
```

#### Figures & captions

```html
<figure>
	<img src="..." />
	<figcaption>
		Some text here
	</figcaption>
</figure>
```

When you use `width: min-content` on `figure` (which in this case is the parent) it will take up the width of the widest child.

### Height algorithms

`height` wants to fill up the smallest possible space (think `min-content`) in comparison to `width` that wants to take up all the available space (`auto`).

To avoid having whitespace below content that doesn't fill up the entire viewport height we should do the following:

```css
html, body {
	height: 100%;
}
.wrapper {
	min-height: 100%;
}
``` 

> This way, the minimum size is equal to the viewport height, but it can overflow and take up more space if required by the content.

`height` behaves like a vacuum-sealed bag shrinking and wrapping around its content.

![Picture of vacuum-sealed nuts](https://marvel-b1-cdn.bc0a.com/f00000000114298/cdn11.bigcommerce.com/s-jsjjp2/images/stencil/1024x1024/products/486/1582/ZipperBags_2__06016.1454519777.jpg?c=2)

What about `vh`???

>When you scroll on a mobile device, the address bar and footer controls slide away, yielding their space to the content. This means that scrolling on a mobile device changes the viewport height.
>
>To avoid flickering UI issues, browsers like iOS Safari and Chrome Android will set  `vh`  equal to the  _maximum viewport height_, after scrolling. This means that when the page first loads,  `100vh`  will actually be quite a bit taller than the viewable area

#### What about the footer?

```css
html, body  {
  height: 100%;
}

.wrapper  {
  display: flex; /* this is also important */
  flex-direction: column; /* this is also important */
  min-height: 100%;
}

footer  {
  border: solid hotpink;
  padding: 8px;
  margin-top: auto; /* This makes all the difference in getting 
	the footer to stick to the bottom of the content.
*/
}
```

## Margin collapse

### Rules 🚨 
1. Only vertical margins collapse
	- 🤯 When block elements are horizontally stacked the rule flips 🛹, horizontal margins are now collapsing 🤯 
2. Margins _only_ collapse **in Flow layout**
	- ℹ️ It does not collapse when using for example `display: flex` on the parent.
3. Only adjacent elements collapse
	- 🚨 Adding a `<br />` between them will break the collapse.
4. The bigger margin wins 🏆
	>This one feels intuitive if you think of margin as "personal space". If one person needs 6 feet of personal space, and another requires 8 feet of personal space, the two people will keep 8 feet apart.
5. Nesting doesn't prevent collapsing

	**What's the meaning of margin?** 🤔
	> Margin is meant to increase the distance between siblings. It is _not_ meant to increase the gap between a child and its parent's bounding box; that's what padding is for.
	>
	>Margin will always try and increase distance between siblings, **even if it means  _transferring_  margin to the parent element!** In this case, the effect is the same as if we had applied the margin to the parent `<div>`, not the child `<p>`.
	>
	> Margins only collapse when they're _touching_.
	
	**What's it blocked by?** 🧱
	- Padding
	- Border
	- Gap (for example if the parent has a specified height which create more space than the child's margin)
	- Scroll container
	
6. Margins (parent and child) can collapse in the same direction
	- This means that if both the child and the parent have `margin-top`, the one with the biggest margin wins! 
	🤯 **0px margin is still a collapsible margin**, so if the parent has no margin specified, it still becomes a fight for the biggest margin 🤯
7. More than two margins can collapse -> Combination of previous rules:
	 - Siblings can combine adjacent margins (if the first element has margin-bottom, and the second one has margin-top)
	-   A parent and child can combine margins in the same direction
	(You can essentially have 2 parents and 2 children with each a separate margin, but the biggest margin wins)
8. Negative margins
	- They also compete -> If you `-75px` and `-100px`, `-100px` wins.
	- What about positive and negative? -> They add up, `+25px` and `-25px` = `0px`
9. Multiple positive and negative margins
	The algorithm works like this: 
	- Find the largest positive margin
	- Find the largest negative margin 
	- Add those two numbers together

## Using Margin Effectively

You can style layouts without even using margin. Max Stoiber, co-creator of styled-components explained how [margin is harmful.](https://mxstbr.com/thoughts/margin/).

💡 **Tip of the day:** Avoid putting margin on something at the component boundary 💡

> For reusable components, we want them to be as _unopinionated_ as possible. - Josh W. Comeau

A solution is to use _layout components_.  

> ...the basic idea is that components aren't allowed to have "external margin", margin that extends past the edge of the border box. Instead, components are grouped using layout components...  - Josh W. Comeau

## Workshop: Agency Page

Repo on my github: https://github.com/esplito/huckleberry-css-for-js-devs-workshop 

Time on solution video: 16:48

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzI1MjI5NTgxLDY3MDkxMjM4MCwtNzIzNz
M4NTM5LC0xOTY3NTc4NDM2LDE1NDI2NDY5NSwyMDYwNjY0NzE5
LDk2NTU0NjIyMiwtOTc1MDg2MzMyLC0xODk5OTg2MDYsLTIwNT
I3MjE2NjMsLTE1NjU0ODExOTEsLTEyNDI3NTE3NjUsLTEwMjkx
ODc5MjQsMTI1MjgyODUxMCwtMTg2MzIwODI2NCwxNDM4MjU3MD
U5LC0xMzIwNDgwMjAxLDE0ODAzMTQxOCwxNDgwMzE0MTgsLTI2
ODMxMjI2NV19
-->