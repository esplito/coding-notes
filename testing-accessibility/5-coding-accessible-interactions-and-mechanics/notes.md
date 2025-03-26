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

## Section 3 - Focus Management

To be completed.

## Section 4 - Check your work with screen readers

To be completed.

## Section 5 - Announcements with Assistive Technology

To be completed.

## Section 6 - Advance Scripting with ARIA

To be completed.