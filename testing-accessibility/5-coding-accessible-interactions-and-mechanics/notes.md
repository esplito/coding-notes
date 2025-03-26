# Coding Accessible Interactions and Mechanics

Github repo: https://github.com/testing-accessibility/workshop-interactions-mechanics

Deployed version of the workshop application: https://workshop-interactions-mechanics.testingaccessibility.com/

**Table of contents**
- [Section 1 - Intro & overview](#section-1---intro--overview)

## Section 1 - Intro & overview

>For accessibility, it’s worth pointing out that JavaScript is necessary for things like responding to events and setting ARIA attributes that can track state.
>
>In this workshop, we’re going to focus on a lot of JavaScript. But remember, it layers on top of the basics of semantic HTML and well-styled CSS. - Marcy
## Section 2 - The Keyboard

>In this section, we’ll revisit keyboard accessibility in the CampSpots application and examine techniques for improving the keyboard user’s experience on the web. - Marcy
### Lesson 1 - Identifying Keyboard Operability Issues

>💡Tip: As an exercise, change the focus outline CSS to a color that provides an accessible contrast ratio against the search button.

>**In this section, we’ll dive deeper into patterns, conventions, and techniques of how to make things respond to the keyboard.**

>💡Tip: Other elements that are focusable by default include form inputs and links with href attributes.

>💡Tip: When building a more involved interaction like the date picker, it’s valuable to do some testing with people who rely on the keyboard. Their feedback helps make sure you’re providing a good experience without making assumptions.

### Lesson 2 - Resources for Keyboard-Focused Development

Useful resources to help us test and implement keyboard-friendly web applications:
- [No Mouse Days Package](https://github.com/marcysutton/no-mouse-days)
	- "The idea is to have a package you can install that turns off the mouse cursor in your application one day a week." - Marcy
	- Alternative: Run the following in your console on the page you like to test:
	  `document.body.style.cursor = "none"`
- [The ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
	- "Whenever you’re building a web application, it’s helpful to follow patterns." - Marcy
	- "... a wealth of information about the conventions behind the most common controls used in web development. Each of the patterns they outline include a description of expected keyboard interactions." - Marcy
	- When building custom components compare it to [the list of patterns](https://www.w3.org/WAI/ARIA/apg/patterns/)
		- 🚨 It is not production-ready patterns. You have to test them too! 🚨

### Lesson 3 - CSS Visibility Techniques in the CampSpots Component Sandbox

Small examples of common components that we can use for experimentation can be found at `http://localhost:1234/component-sandbox` when running the project locally.

>When we apply different CSS visibility techniques to the button, we can look at how the Accessibility Tree changes.
>
>It’s a useful tool for troubleshooting issues that might be caused by a parent element further up the tree. - Marcy

**Techniques covered in the lesson**:
- "visually-hidden" class (the class we have used previously in other workshops to hide content visually but still make it available in the accessibility tree)
  
- "opacity"
	- apply `opacity: 0;` in CSS
	- we don't see the element but it still rendered and you'll get a tab stop at the element.
	  
- "display"
	- apply `display: none;` in CSS
	- "... a big hammer that hides the element from everybody in every context." - Marcy
	- "One interesting fact is that you can still use `aria-labelledby` to reference an element with `display: none;` applied to it."  - Marcy
	  
- "visibility"
	- apply `visibility: hidden;` in CSS
	- "This is a good technique if you don’t want something affect your layout but it’s irrelevant for a screen reader to read aloud." - Marcy
	  
- "aria-hidden"
	- Isn't a CSS approach
	- "If a screen reader is able to focus on a tab stop that doesn’t have accessibility information, it can cause what is known as a _ghost control_. It can be quite confusing! Chrome has evolved in this scenario, where the element is `aria-hidden` but still focusable, to render it with accessibility information anyway." - Marcy
	- apply `aria-hidden="true"` and `tabindex="-1"` to element in HTML
		- "On its own, `aria-hidden="true"` affects screen readers and not the keyboard." - Marcy
#### Debugging Visibility Issues

>If you find yourself in a situation where you can’t see where you are on the page or you can’t operate a control, check out the CSS in DevTools

>Because of the _box of spiders_ that the `focus` pseudo-selector implementation has been historically for mouse users, a new `focus-visible` pseudo-selector was created for keyboard users. Focus-visible uses heuristics to apply focus styles more intelligently for keyboard interactions where focus couldn’t be updated or adapted. 

Do the following in Chrome's CSS pane (in DevTools):
>Click the `:hov` option, then select `:focus` and `:focus-visible` to force the element to be activated for both scenarios. 

### Lesson 4 - Implement a Skip Link Component

>When a keyboard user navigates, they will have to Tab through the menu in order to move down to the main content of the page. Of course, this isn’t as bad as having to Tab through all of the non-visible menu items like we saw previously but it still doesn’t provide a great experience.
>
>Skip links to the rescue!
>
>Not only are skip links useful for keyboard access, they are also a requirement in [WCAG Success Criterion 2.4.1, Bypass Blocks (Level A)](https://www.w3.org/TR/WCAG21/#bypass-blocks).

Common usage:
- "Skip to main content" to get past repeated navigation

Occasional usage:
- "Back to top" on a long page

Other useful areas:
- Non-modal dialogs
- or other places where it would be nice to be able to jump around the page with a keyboard

A **skip link can**:
- Point to an ID attribute on a container element
	- Example: `<a href="#main">` -> `<main id="main">`

A **skip link should**:
- Be visible to all keyboard users
	- and **not be visually-hidden for screen reader use only** (this was common previously)




## Section 3 - Focus Management

To be completed.

## Section 4 - Check your work with screen readers

To be completed.

## Section 5 - Announcements with Assistive Technology

To be completed.

## Section 6 - Advance Scripting with ARIA

To be completed.