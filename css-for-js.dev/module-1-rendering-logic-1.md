
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

There's some inline elements that are special. They are called


>All three elements (`img`, `span`, `button`) are inline elements, and they've all been given `margin-top`. In general, we don't expect this property to have any effect, and it doesn't on the `span`! But both the `img` and the `button` have the top margin applied, because they're special elements.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NjUwNjkxNzUsLTc2NzU1OTU4NSwxOT
QzMDI1NDgwLC0xOTc3NjI0MTYzLC0xNDY2MjExOTA0LDcyMzU4
OTExLDQ2MTgwMTQxOSwzMDU0NDI1NjYsLTI3NDA0ODcyOCwtMT
gwMDU3NDMzNiwtOTQzNTQ0NTg0LC0xNzA5ODEyOTQwLC0yNjUw
MDEwOTBdfQ==
-->