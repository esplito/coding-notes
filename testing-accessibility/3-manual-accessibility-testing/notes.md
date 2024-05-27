
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
-  When focusing items in the menu I don't get a hint to what page they are pointing towards
- Several lin

## Useful resources
- [WebAIM mailing list](https://webaim.org/discussion/)
- [“Understanding”](https://www.w3.org/WAI/WCAG21/Understanding/)
- [“How to Meet”](https://www.w3.org/WAI/WCAG21/quickref/) 
- [Workshop resources with links](https://workshop-resources.testingaccessibility.com/#workshops-testing)

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTE3MTM1NDUsMzczOTc4NTEsLTExOTUzNj
g4MDgsODI4Nzg5MTMwLDkxNTc5NjkwMyw0MjM3MDE5ODgsMTU5
OTM2OTQyNyw3MzA5OTgxMTZdfQ==
-->