
# Manual Accessibility Testing

Github repo: https://github.com/testing-accessibility/workshop-manual-a11y-testing

**Table of contents**
- [Section 1 - Intro & overview](#section-1---intro--overview)
- [Useful resources](#useful-resources)

## Section 1 - Intro & overview

### What will I get in this module?
- A repeatable accessibility testing process that can be applied to any web application. ♲
- Gain experience with different tools and learn their limitations and when to look beyond them

🚨 **Remember to think about how you would apply this to your own applications!** 🚨

This summarizes what this module is all about:
> 📝Exercise
>**You are a developer for CampSpots, whose product is a web app that will allow users to browse and reserve places to go out and camp.**
>Another member of the dev team has opened a Pull Request with an initial prototype build of the site. It is your responsibility to test the accessibility of the application. As you find issues with the application, share details about the issues along with how you would implement the fixes.
>The fixes you make to the application should meet or exceed WCAG 2.1 AA success criteria. For example, the app should be keyboard accessible and provide a good screen reader experience.

Regarding WCAG:

>If at any point you are confused about a guideline, search for related content on the web or ask in digital accessibility communities such as the [WebAIM mailing list](https://webaim.org/discussion/). The official [“Understanding”](https://www.w3.org/WAI/WCAG21/Understanding/) and [“How to Meet”](https://www.w3.org/WAI/WCAG21/quickref/) documents are also useful for providing an additional context where success criteria are a bit vague.

I've tried to summarize the workflow that Marcy suggests and you can find it in the [workflow-template.md file](./workflow-template.md)

## Section 2 - Keyboard Testing

### Lesson 1 - Identify issues by hitting Tab

>_The first step in testing keyboard accessibility is to start hitting the Tab key to see what interactive items you can and can't reach._

_**It’s the fastest way to start determining if something was built with accessibility in mind.**_

Use `focusin` event to easier see which html element that is being focused. Marcy adds this to the developer console:
```js
document.body.addEventListener('focusin', () => console.log(document.activeElement))
```

#### 🛠 Challenge: Search for Issues by Hitting Tab

Issues found:
-  When focusing items in the menu I don't get focus indicators and I can't see the submenu.
- Several links only have a `#` tag, which makes sense if you are not done with defining links on the page.

#### 🛠 Solution: The Issues Found

>When tabbing through the CampSpots app, here are some issues you should find:
>
>-   There isn’t much of a visual indication to where you are >focused on the page.
>-   Several links have the only  `#`  symbol, but this isn't a huge deal at the moment since this is a prototype app.
>-   The Browser status bar sometimes shows that you have selected a link to a different page, but it doesn’t open a nav menu and you can’t see where you are visually in the mega menu ("Plan Your Trip", "Ways to Stay", "Resources").
>-   You could Tab into the form fields on both pages but hitting Tab again skips over the Submit button. Skipping the submit button with the Tab key could be acceptable if the form Submit was disabled until required fields were filled.

#### What can you do when you have identified some issues?

>_A typical workflow is to identify a few problems, examine markup in DevTools, then jump over to the code to remediate the issues._

- Use dev tools in browser to investigate them
	- Check focus styles by forcing their interactive states (example: `a`-tags can  give us a hint)
	- Check HTML

By investigating we can find that we need to remove the `outline: none` that is being applied to all elements.

We also need to swap the elements in the mega menu from `div` to `button` to make them focusable.

### Lesson 2 - Fix Focus and Operability Issues for Keyboard Interactivity

#### Issues that need fixing
**Issue 1:** Update the stylesheet so that focus styles are visible. The styles can be found at `scss/_defaults.scss`.
**Issue 2:** The MegaNav needs its items to be replaced with a semantic element that is focusable and operable. The markup can be found at `_includes/header.html`
**Issue 3:** The links inside of the MegaNav sections should not be focusable when the menu is closed.

#### My solutions to the issues

Solution 1: Update styles from 
```scss
* {
	box-sizing: border-box;
	outline: 0;
}
```
to
```scss
* {
	box-sizing: border-box;
}
```

Solution 2: Update `megamenu-navitem` divs to be `button` instead and apply the styling suggested by Marcy (just to fix the default styles applied by the browser).

Solution 3: Ensure that they have `display: none` instead of just `opacity: 0` and `display: flex`, when they aren't visible. 

When **not** hovered or active:
```scss
.megamenu-submenu {
	background-color: var(--bgcolor-menu);
	color: var(--color-primary-light);
	display: none;
	opacity: 0;
	font-size: 0.85rem;
	height: 0;
	left: 0;
	max-width: var(--width-wide);
	overflow: hidden;
	position: absolute;
	width: 100%;
}
```

When hovered or active:
```scss
.megamenu-section.active  .megamenu-submenu,
.megamenu-section:hover  .megamenu-submenu {
	height: 225px;
	display: flex;
	opacity: 1;
	overflow: hidden;
	z-index: 1;
}
```

### Lesson 3 - Toggle the Menu and add Escape Key Handling with JavaScript

The handling of adding the `active` class was already in the local `menu.js` file, so I simply read through the lesson material and watched the videos. Most of this stuff is handled automagically anyway when using libraries like react-router etc. for navigation elements. Good however to refresh my memory regarding how to write this in vanilla JS 😄



## Useful resources
- [WebAIM mailing list](https://webaim.org/discussion/)
- [“Understanding”](https://www.w3.org/WAI/WCAG21/Understanding/)
- [“How to Meet”](https://www.w3.org/WAI/WCAG21/quickref/) 
- [Workshop resources with links](https://workshop-resources.testingaccessibility.com/#workshops-testing)

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzODA2OTAyMCwtMTQ3NDk2MzA2Nyw5OD
ExNjk1MTQsNzQ3OTU3OTcsMTQ2MzM0OTM2NiwtMTU2OTgwNDAz
NCw4NzAwNzQ5MTgsMzczOTc4NTEsLTExOTUzNjg4MDgsODI4Nz
g5MTMwLDkxNTc5NjkwMyw0MjM3MDE5ODgsMTU5OTM2OTQyNyw3
MzA5OTgxMTZdfQ==
-->