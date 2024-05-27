# Workflow template for manual accessibility testing 

## Background from course module intro
> The next time you need to test a web application, create a document and follow these steps as a starting point. Include additional notes, screenshots, and links to resources along the way.
>Whether you’re responsible for mitigating issues or not, the test report you compile will help you prioritize fixes and ship a more accessible application.

## Workflow
1. Identify user flows 🕵️‍♂️
2. Keyboard testing ⌨️
3. Check heading and landmark structure 🏞
4. Scan each page with Axe Devtools 🧐
5. Check colour contrast 🎨
6. Repeat the user flow at different viewport sizes 📱
7. Test with screen readers 🗣

## Checklist

- [ ] Identify user flows 🕵️‍♂️
	- [ ] Which user flow are you testing?
	- [ ] Which pages on the site will be visited?
	- [ ] Which components are most critical?

- [ ] Keyboard testing ⌨️
	- [ ] Tab through each step in the user flow
	- [ ] Is everything that can be done with a mouse also focusable and operable with keyboard?
	- [ ] Can you see visible focus indicators?
	- [ ] Include screenshot from [Accessibility Insights Tab Stops tool](https://accessibilityinsights.io/downloads/).
	- [ ] Check for divs that should be `button`

- [ ] Check heading and landmark structure 🏞
	- [ ] Use accessibility tab in Firefox
	- [ ] Also use Accessibility Insights Heading and Landmark tools
	- [ ] Include screenshots from [Accessibility Insights Heading and Landmark tools](https://accessibilityinsights.io/downloads/).

- [ ] Scan each page with Axe Devtools 🧐
	- [ ] Take notes of any issues it finds (especially critical ones)
		- [ ] For the found issues: include links to relevant guideline documentation

- [ ] Check colour contrast (with Colour Contrast Analyzer) 🎨

- [ ] Repeat the user flow at different viewport sizes 📱
	- [ ] Take screenshots of pages at device widths where things could be improved (use developer tools to simulate different viewports)
	- [ ] Test on actual mobile device browser (if you can)

- [ ] Test with screen readers 🗣
	- [ ] NVDA in Chrome (Windows)
	- [ ] VoiceOver (Mac)
	- [ ] Lists announced properly
	- [ ] Headings announced properly
	- [ ] Can user efficiently complete their task?

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk0NjYwMDE3OCwxNTEyODM3NTIwLC0xNj
MzMDQxNDMyXX0=
-->