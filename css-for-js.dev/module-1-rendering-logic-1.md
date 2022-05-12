
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
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMzY4MjQ2MjIsLTk0MzU0NDU4NCwtMT
cwOTgxMjk0MCwtMjY1MDAxMDkwXX0=
-->