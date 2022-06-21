
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


## Width algorithms

Block-elements have `width: auto`  **not** `width: 100%`.

> It's a subtle but important distinction: by default, block elements have _dynamic sizing_. They're context-aware.

> `auto` will let our element greedily consume the available space while respecting any constraints. 

### Keyword values

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

`fit-content` is the magic keyword. It uses the best of both worlds. It acts like `max-content`,  but adds linebreaks and also behaves like `auto` to fill the available space.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0MzAxMzk2NywtMjY4MzEyMjY1LC0xOD
I1MjgwNDk3LDE5MTQ1NTk3NzcsLTE2ODg2OTE0ODksMTE1ODc1
MTA1MywtMjEwNDgwMDY0NSwtMTU0NjY2ODk4Nyw0Njk4MTYwNz
ksMjAyMDg4NDc1NiwtMTIwOTQwOTQ3NSwtMTg3NjQxMzIxMCwt
MTkzMTMzMTIyMSwyMjAyMTY5NDksLTc2NzU1OTU4NSwxOTQzMD
I1NDgwLC0xOTc3NjI0MTYzLC0xNDY2MjExOTA0LDcyMzU4OTEx
LDQ2MTgwMTQxOV19
-->