
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

The handling of adding the `active` class was already in the local `menu.js` file, so I simply read through the lesson material and watched the videos. I also implemented the Escape key handling in my local env. 

Most of this stuff is handled automagically anyway when using libraries like react-router etc. for navigation elements. Good however to refresh my memory regarding how to write this in vanilla JS 😄

>💡Tip
>There’s  [an entire Focus Management module](https://testingaccessibility.com/learn/interactions-and-mechanics/focus-management)  in the Coding Accessible Interactions & Mechanics workshop.

### Lesson 4 - Fix Additional Keyboard Interactivity Issues

#### 🛠 Challenge: Update Submit Buttons on Home & About Pages

>Both the Homepage and the About page have forms with div-based submit buttons.
>Update the markup on both buttons to enable them to be keyboard focusable and operable.

I changed so that button is the following (instead of a div):
```html
<button  class="btn-submit btn-lookingglass">
	<span  class="icon-lookingglass-white"></span>
</button>
```

And so that the form has the following for now (since we don't have search implemented):
```html
<form
class="main-search form-wrap layout"
id="home-form"
onsubmit="javascript: event.preventDefault(); alert('submitted form');"
>
<!--- more content here --->
</form>
```
I did the same thing on the form on about page. 

#### Summarizing the keyboard testing module
I believe that this module has demonstrated how much difference simply focusing on keyboard testing does. By ensuring that things are focusable and operable with a keyboard, we improve the accessibility of an application quite drastically. It forces us to "use the platform" better and to think about Semantic HTML and not just divs all over the place.

I like the following that Marcy said at the end of the module:
>"With every feature and Pull Request that comes in, chip away at making your application accessible by doing some keyboard testing."

## Section 3 - Testing with Tools & Browser Extensions

**🧰 Developer Tools to Install:**

-   Developer Tools to Install:  [axe Developer Tools Browser Extension](https://www.deque.com/axe/browser-extensions/)  (Chrome or Firefox)
-   [Colour Contrast Analyser](https://www.tpgi.com/color-contrast-checker/)  (Mac or Windows desktop)
-   [Accessibility Insights](https://accessibilityinsights.io/)  (Chrome or Firefox)
-   [Web Developer Toolbar](https://chrispederick.com/work/web-developer/)  (Chrome or Firefox)
-   [axe-linter for VSCode](https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter)

### Lesson 1 - Identify Color Contrast Issues with Color Contrast Analyzer & Chrome DevTools

#### 🛠 Challenge: Fix MegaNav Contrast Issues
Changed header color to `--color-accent-dark` and it seems to work :)

In `CSS Overview` in Chrome it now passed their contrast ratio check and it also passes when using CCA.

### Lesson 2 - Identify Common Issues with Axe Accessibility Linter

#### Install axe-linter
> If you are a VSCode user, I suggest installing the axe Accessibility Linter extension from the [Visual Studio Code Marketplace](https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter). Similar to tools like ESLint, axe-linter will highlight common accessibility issues in your project files and suggest fixes.

#### Image Alt tags
When we open up `header.html` we can see that axe-linter tells us that we are missing alt tags on images in the header. The error:
```bash
axe-linter (image-alt): Ensures <img> elements have alternate text or a role of none or presentationaxe-linter[image-alt](https://dequeuniversity.com/rules/axe/4.9/image-alt?application%3Daxe-linter)
```

Marcy recommends using W3C's [alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) for creating useful image descriptions in the alt tag.

The questions from that alt tree:
- **Does the image contain text?**
- **Is the image used in a link or a button, and would it be hard or impossible to understand what the link or the button does, if the image wasn’t there?**
- **Does the image contribute meaning to the current page or context?**
- **Is the image purely decorative or not intended for users?**
- **Is the image’s use not listed above or it’s unclear what  `alt`  text to provide?** 

##### Header
Here's what Marcy thinks about the image descriptions in the header:
> For these nav menu images, it could be argued either way if they contribute to the content of the menu. If you think a person using a screen reader would benefit from a description of the images in the MegaNav, add some alt text. Otherwise, set the alt to a blank string and the screen reader won’t announce it.

> 💡Tip
>For now, don’t worry about alt text on SVG images. There’s more on this in the Accessible Naming module in the Semantic Markup workshop.

##### Homepage
There's a `tents.jpg` on the home page that actually adds context and Marcy suggests "A festival of tents at sunset".

>💡Tip
>
>You don't need to add "Photo of..." to your alt text descriptions. When a screen reader sees the img tag, it will already announce it as an image.


### Lesson 3 - Scan a Page for Issues with Lighthouse & Axe Developer Tools

#### Google Lighthouse

Marcy's thoughts on it: 
>It’s nice that Lighthouse points out that not every accessibility issue can be automatically detected as well as a suggested list of things to test manually, but it’s not my go-to tool.
>
>If you are on a machine that is pretty locked down and won’t let you install third-party tools or browser extensions, Lighthouse could be a good alternative.

My comment on it: It's not my go-to tool either. I always aim to have 100 in accessibility score, but I know that it is not indicative of whether the application is actually accessible. There's often a ton of work left even if you get 100 in score.

#### Axe Developer Tools

Marcy's comment on this tool:
>For general issue scanning, my go-to tool is Axe Developer Tools. 

She also warns about some limitation to what Axe Dev Tools scans:
>⚠️**Caution**
>
>Axe DevTools **will only scan what has been rendered on the page, not what is hidden or inactive**!

![screenshot of the button issue found by axe dev tools](https://github.com/esplito/coding-notes/blob/master/axe-devtools-example.png?raw=true)

>There are several Issue Tags present that point us to places we can go to learn more.
>
>-   `wcag2a`  is pointing us to WCAG 2.0 level A, which is included in WCAG 2.1.
>-   `wcag412`  is pointing us to WCAG success criterion 4.1.2.
>-   `section508`  and  `section508.22.a`  are related to US Government standards.
>-   `ACT`  is related to Automated Conformance Testing (ACT), a standard for writing automated accessibility testing rules.
>-   `cat.name-role-value`  is a category tag used internally by the axe-core API.

Marcy suggest we focus on `wcag412` and the corresponding guideline, [WCAG Section 4.1.2 - Name, Role, Value](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value.html), which is level A and therefore critical.

We can find more info about the issue found by axe by pressing "more information" in the devtool. In this case we get redirected to https://dequeuniversity.com/rules/axe/4.9/button-name?application=AxeChrome where we can see:
- User impact (minor to critical)
- Disabilities Affected
- Standards related to it
-  WCAG success criteria (2.0, 2.1, 2.2)
- Related guidelines

### Lesson 4 - Check Heading Structure with Web Developer Toolbar

Marcy says this about Web Developer Toolbar:
> This is one of my favorite tools for checking the structure of a page. Ideally, browsers would build this tooling feature in for us. But for now, this classic add-on is very helpful.

🚨 **Note about the tool:** 🚨
> Note: you may encounter issues with Web Developer Toolbar occasionally where headings are shown even though they aren’t readable with Assistive Technologies. For example, content marked with `aria-hidden="true"` or as part of Web Components templates may be listed even though they shouldn’t. Be sure to test in a screen reader to verify user impact before filing any issues stemming from this tool!

I open up the Web Developer Toolbar, press `Information`, then press `View Document Outline`.

We are missing a `h1` & `h2` on homepage.

We are missing `h1`, `h3`, `h4`, `h5` on about page.

#### Deciding Heading Levels Based on Design
>Sometimes you'll receive a design that lacks a visual heading for a major content section.
>If this happens, what should you do?
**Options:**
- Talk to your design team about adding a visual heading that describes the section for everyone to see and use.
- Sometimes a heading should be a label -> change it semantically, but keep the visuals
- Last resort if you encounter a lot of resistance regarding changing a visual heading: _"add a heading of the correct level using visually-hidden CSS as a last resort."_

**Remember the following! 🚨**
>... the important thing to keep in mind is that you should always pick the correct heading level for the content within an overall semantic page structure. **A proper semantic structure is one the most important things in accessibility.**

>💡Tip
>Don’t use headings for their font sizes. That’s what CSS is for!

### Lesson 5 - Visualize Tab Stops, Headings, and Landmark Regions with Accessibility Insights

#### Accessibility Insights

>The Accessibility Insights FastPass is similar to what we've seen with Axe DevTools, so we won't go into that part further.

Marcy recommends the "Ad hoc" tools in Accessibility Insights:
- Tab Stops Tool
- Headings Tool
- Landmarks Tool

##### Tab Stops tool
 There's a tab stops tool that visualizes the focus order when you tab through a page.

Example of it from the home page in the example website:
![Tab stop visualization on camp sports homepage](https://testingaccessibility.com/_next/image?url=http%3A%2F%2Fres.cloudinary.com%2Ftesting-accessibility%2Fimage%2Fupload%2Fv1657563632%2F01-manual-testing%2F03-testing-with-devtools%2F05-visualize-tab-stops-headings-and-landmark-regions-with-accessibility-insights%2Fcf105fab-28c0-4193-84fb-a15da0efee22_udi1wu.png&w=3840&q=100)

Tab stops tool before our fixes to the menu:
![Tab stop visualization on camp sports homepage - before fixing the menu. had a messy focus pattern.](https://testingaccessibility.com/_next/image?url=http%3A%2F%2Fres.cloudinary.com%2Ftesting-accessibility%2Fimage%2Fupload%2Fv1657563833%2F01-manual-testing%2F03-testing-with-devtools%2F05-visualize-tab-stops-headings-and-landmark-regions-with-accessibility-insights%2F4f05916a-fc5a-4f91-a26a-1b213ed75029_mzsyab.png&w=3840&q=100)

##### Heading tool
> Toggling on the Headings function, Accessibility Insights will draw colored boxes around the headings and include an indicator of their level.

##### Landmark tool
>Similar to the Headings tool, the Landmarks tool will highlight and label the page's landmarks.
>This is useful for visualizing the landmarks you currently have and determining what on your page might work well in a region.

#### Accessibility Insights Assessment
>The Assessment tool is an awesome guided tool that has you work through the WCAG AA success criteria, including things that can't be automated.
>Each section provides information about what you need to do in order to test your application. Once you've performed the test, you self-report whether it passed or failed.

### Lesson 6 - Challenge: Fixing Issues Found with Lighthouse & Axe DevTools

#### 🛠 Challenge: Fix the Homepage Search Button Issue

>The Axe DevTools scan found an issue with the Search icon button on the homepage not having discernible text. This button accompanies two form fields for searching camp locations and dates.
>
I decided to go with aria-label since the button performs an action when pressed. It would not make sense to "hide" it from screen readers etc. I believe it would be even better to have the text visually next to the "looking glass", but that would require (in a work scenario) to discuss it with UX/UI designer.
```html
<button  class="btn-submit btn-lookingglass"  aria-label="Search">
	<span  class="icon-lookingglass-white"></span>
</button>
```

#### 📝 Exercise: Fix More Axe DevTools Issues

- Added `<html lang="en">`
- Fixed som SVG alt tags to be empty string since they provide no context

### Lesson 7 - Challenge: Fixing Structural Issues with Headings and Landmarks

> Work through fixing these issues on the Home and About pages, checking with the Web Developer Toolbar and Accessibility Insights as you go.

>💡Tip
>When you adjust heading levels you’ll usually need to update the styling as well.

>When you’re confident in your fixes, run Lighthouse and Axe DevTools again to check your scores.

My fixes (on homepage):
- I wrapped the logo + "Campspot" link text in h1.
- I added an h2 above the form with the content "Find your camp spot"
- changed header from `div` to `header`
- changed footer from `div` to `footer`
- changed `<div id="main">` to `<main id="main">`

My fixes (on about page):
- changed `<div id="main">` to `<main id="main">`
- changed `h4` to `h3`
- changed `h6` to `label`

No issues with axe dev tools after that and 100 accessibility score in Lighthouse.

### Lesson 8 - Update a Form to be Semantic (Tools Can’t Catch Everything)
- Update the Contact Form to be Semantic on about page

Fixes:
`<div>` -> `form`
`span` -> `label` (and add `for` to `label` and `id` to `input` or `textarea`)

#### About placeholders
>In our example above, the input has a placeholder of "Type your message here" and is paired with a label element and for attribute that matches its  `id`  of "message".
>
>Pairing with a visible label element is the best method of conveying the purpose of a  `textarea`,  `input`, or other form control.
>
>In this case, the placeholder is a redundant description that will be fine to disappear visually when the field is focused.

>💡Tip
>Automated tools don’t take issue with inputs that have placeholders, but it’s not the most accessible user experience!

#### Labeling a form without a visible label

>It's always best to have a visible label for a form, but there are situations and designs where you might not have that option. There are a couple different ways you could go about working through this scenario:
>
>-   Use  `aria-label`  on the input itself.
>-   Add a paired  `<label>`  element with a custom  `visually-hidden`  (but rendered) CSS class.

## Section 4 - Magnification and Zoom Testing

>Our web applications should remain usable no matter the zoom level or viewport size. It just so happens that designing and developing for people with low vision goes right along with responsive web design!

### Lesson 1 - Testing Zoom in the Browser

Marcy's suggestion:
>*"zoom in to at least the 200% level (if not all the way to 500%) to see how the page layout reacts."*

> **Pay attention to how menus, text content, images, and interactive behavior changes as you zoom in and out.**

#### 📝 Exercise: Identify Behavior when Zooming

- Zoom in and navigate around home & about page on the CampSport website.

Ask yourself:
- Which elements of the page act like you'd expect?
	- The navigation looks like it should and I can still easily use it.
	- 
- What behavior or layout changes need to be made on each page?
	- Some of the content on both home and about ends up outside the frame so I have to side scroll. Not ideal.
		- Example: Search on home page

##### Marcy's observations
On home page:
>-   The MegaNav has changed its layout in order to fit. The images that are inside the “Plan Your Trip” section disappear at 300% zoom.
>-   There’s a large gap between the navigation menu and the search form.
>-   The search form is still landscape format and runs off the side of the screen horizontally.
>-   The tents image is pretty large.
>-   The “Rest and relaxation” text fits well.
>-   The three colorful cards stay in landscape format and scroll off the screen.

On about page:
>-   The image next to the “About” text in the hero section gets scrolled off the side of the page.
>-   The Contact form also scrolls off the side of the page.

Responsiveness relates to 1.4.10 "Reflow" in WCAG.

>Referencing WCAG suggests solving reflow issues with CSS media queries (as one technique), which we’ll look at further in the next lesson.

>💡Tip
>Some users may use Operating System-level zooming to enlarge the content they’re viewing. This will not trigger media queries or adjust the layout of your pages.

### Lesson 2 - Testing responsiveness with DevTools

- Turn on the responsive feature in Chrome Devtools

>If after changing device sizes and zoom levels you still don’t see any reflow changes, the site you’re on might be missing the  [viewport meta tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag)  in the HTML document  `<head>`.
>
>The viewport meta tag tells the browser on your user’s device how to render the page. There are multiple options you can set to make a page function better on a mobile device.
>
```
<meta name="viewport" content="initial-scale=1" />
```

>⚠️Caution
>Do not disable user-scaling in the viewport meta tag! This will disallow pinch and zoom, which is a critical accessibility feature.

### Lesson 3 - Challenge: Fix Zoom Issues with CSS

I did the following fixes:
- Add ```<meta  name="viewport"  content="width=device-width, initial-scale=1" />``` to `index.html`
- Change in `_header.scss` to 
 ```scss
#header {
	color: var(--color-accent-dark);
	padding-top: 0;
	position: relative;

	@media  screen  and (min-width: 1440px) {
		padding-top: 1em;
	}
}
```
- Change in `_layouts.scss` to
```scss
.layout {
  margin: 0em auto;
  padding: 1em 0.5em;
  width: 100%;

  @media screen and (min-width: 768px) {
    padding: 4em 0.5em;
    width: var(--width-inset);
  }
}

.two-parts-50-50 {
  grid-template-columns: 1fr;

  @media screen and (min-width: 768px) {
    grid-template-columns: 1fr 1fr;
  }
}

.three-parts {
  display: grid;
  gap: 1em;
  text-align: center;
  grid-template-columns: 1fr;

  @media screen and (min-width: 1440px) {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
```
- Change in `_about.scss` to
```scss
.page-about  #header {
	min-height: 140px;
}
```
- Change in `_home.scss` to
```scss
.page-home #header {
  background: url(/images/backpacking1.jpg) no-repeat;
  background-position: top center;
  background-size: cover;
  min-height: 240px;

  @media screen and (min-width: 1440px) {
    min-height: 640px;
    background-size: contain;
  }
}

.page-home .form-wrap {
  align-items: end;
  display: grid;
  gap: 1em;
  grid-template-columns: 1fr;
  margin: 0 auto 0;

  @media screen and (min-width: 768px) {
    grid-template-columns: 5fr 5fr 1fr;
  }
}
```

## Section 5 - Screen reader testing

### Lesson 1 - An Overview of Screen Reader Software

Marcy suggests the following regading screen readers:
>I highly suggest making NVDA testing part of your regular process, as there are times when VoiceOver on a Mac might work as expected but NVDA won't. This is often because Windows screen readers have different [interaction modes](https://tink.uk/understanding-screen-reader-interaction-modes/) than VoiceOver does.

She also recommends [Parallels](https://www.parallels.com/) for running Windows on Mac.

>💡Tip
>For more information on popularity of various screen readers, browsers, and platforms, check out the [WebAIM Screen Reader Survey](https://webaim.org/projects/screenreadersurvey9/).

Always check your analytics to get an idea of which browsers and devices that your users use. You might be testing with VoiceOver on desktop, but in fact, 80% of your users are navigating to the page on iPhone. Then you should probably test with VoiceOver on an iPhone.

Links to shortcuts for NVDA, VoiceOver Desktop and VoiceOver iOS can be found in the [course github repo](https://github.com/testing-accessibility/workshop-manual-a11y-testing/tree/main/exercise5-screenreader#screen-reader-testing).

### Lesson 2 - Using VoiceOver to Read & Audit a Page

>Open up the CampSpots homepage, then launch VoiceOver by using **`Fn + CMD + F5`**  (On a Mac desktop keyboard, you can omit the **`Fn`**  key.)

>💡Tip
>You can hit CTRL at any time to get VoiceOver to stop reading.

`CTRL + Option` is the VoiceOver key.

Example: `CTRL + Option + H` will read out headings on a page.

>💡Tip
>The  [VoiceOver Shortcut Cheatsheet](https://dequeuniversity.com/screenreaders/voiceover-keyboard-shortcuts)  from Deque University is a handy resource to bookmark and/or print out for your work station.

#### VoiceOver Rotor
>With VoiceOver running, hit  `CTRL + Option + U`  on your keyboard to open the VoiceOver Rotor.
>
>With the VoiceOver Rotor open, use the right Arrow key to navigate to the next window of options for reading.
>
>If there are multiple items featured in the current window of the VoiceOver Rotor, use the up and down arrows to cycle through them and the Enter key to move to that piece of content on the page.

### Lesson 3 - Challenge: Improving VoiceOver Announcements

- Add additional structure to the MegaNav
- Update the `menu.js` logic to find the correct parent node.

Added some headings, unordered lists, list items, aria-expanded & aria-haspopup to the main buttons in the navigation etc.

### Lesson 4 - Using NVDA to Read a Page

`Tab` and `Arrow` keys work similarly in NVDA as in VoiceOver. There's differences though. If you go to the form in the exercise you'll hear a "cash register" sound in NVDA. This indicates that you went from "browse mode" to "focus mode". 

## Useful resources
- [WebAIM mailing list](https://webaim.org/discussion/)
- [“Understanding”](https://www.w3.org/WAI/WCAG21/Understanding/)
- [“How to Meet”](https://www.w3.org/WAI/WCAG21/quickref/) 
- [alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/)
- [Workshop resources with links](https://workshop-resources.testingaccessibility.com/#workshops-testing)

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTQ1NTI1NDEyLDIxODk1ODcyMywtMTk3Nj
Q4MDkwNiwtMjEwOTc4MzAxNiwxNTYxMjA3MDM0LC0xODI1ODIz
MzY2LDE1NTU0MzY4NDEsMTk2MTM5MzU1LC03NTIwNDg5NzIsND
E2ODAxNTIyLC02MjQ2NzUyMjEsLTcxNTIyODkxLC0xNTYzODg0
NTA4LDgwOTc4NDU3NywtNTIwODc5ODYyLC04ODk4MjQ5MjIsLT
EzMDc1MzU1NjgsMjQ3MTAzMzE1LDc0NTM2NDA5MCwxNDY0Mjkw
MTM0XX0=
-->