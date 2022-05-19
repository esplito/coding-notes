
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

#### Border

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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMyMzg0ODgwOSw0NjE4MDE0MTksMzA1ND
QyNTY2LC0yNzQwNDg3MjgsLTE4MDA1NzQzMzYsLTk0MzU0NDU4
NCwtMTcwOTgxMjk0MCwtMjY1MDAxMDkwXX0=
-->