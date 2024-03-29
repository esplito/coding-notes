# Setting the Stage for Accessibility

**Table of contents**
- [Lesson 1 - What is Digital Accessibility?](#lesson-1---what-is-digital-accessibility)
- [Lesson 2 - Why is Accessibility important?](#lesson-2---why-is-accessibility-important)
- [Lesson 3 - Language & Definitions of Accessibility](#lesson-3---language--definitions-of-accessibility)
- [Lesson 4 - What Makes a Website Accessible?](#lesson-4---what-makes-a-website-accessible)
- [Lesson 5 - Accessibility Needs All of Us](#lesson-5---accessibility-needs-all-of-us)
- [Useful resources](#useful-resources)

## Lesson 1 - What is Digital Accessibility?

🔖 **Definition of digital accessibility (a11y)** 🔖
> _Digital accessibility refers to the practice of preventing and removing barriers that keep disabled people from accessing, reading, or writing digital content and interacting with online features._

🎯 **The goal** 🎯
----> _Eliminate barriers_ to access.
1. Catch issues early in the development phase 
2. Will probably need to talk to designers about things that can be problematic
3. If a11y issues are found, prioritize and remember for the next thing you build!

💡 **Accessibility is...** 💡
1. A civil right to participate in society
2. A key part of design:
> There are analogues between acccessibility in digital spaces and accessibility in the architectural features of public and private spaces ("the built environment"). Being an accessibility expert is like being an architect. We need to prioritize structural access that includes disability in design and user experience.
3. Often forgotten. 🙃

🥁 **What can we do to _not_ make it forgotten?** 🥁

- Share info about its importance
- Keep beating the drum to get more people on board and excited about improving access.
- Prioritize from the beginning -> _Must_ be part of the requirements for the features we ship.
- Communicate its importance in your organization. 
- Find allies and colleagues who are positioned to advocate with you among your management and leadership teams.

>**If accessibility is marginalized, it creates a self-perpetuating culture of discrimination.**

No keyboard support or screen reader support? = 🚨 Exclusion 🚨.

Sometimes using words like _discrimination_ or _ableism_ can get people to pay more attention to accessibility.

> _Inaccessibility_ is discrimination in practice, whether intentional or not. 

### a11y for individuals
- It's important for independence.
	- You otherwise might have to:
		- Share your private account information with someone to get your banking errands done (because of an inaccessible banking website).

### a11y for businesses
🚩**Common excuse** 🚩
"That's not our customer" -> Well, if the website isn't accessible , they can't be your customer.

🌟 **Benefits of prioritizing a11y** 🌟
1. More revenue from a whole new group of people.
2. If you hire individuals with disabilities
	- You get better feedback early in the design & development process and in turn avoid costly rework later
3. The business can highlight the a11y work they've done. By doing this in Diversity, Equity & Inclusion (DEI) reports, it can stand out and gain more customers.
4. You can avoid costly lawsuits by being proactive.
5. Helps expand the business into government and educational sectors where accessibility is a legal requirement.
6. Being proactive gives you an opportunity to be innovative when building, instead of chasing compliance.

**Other aspects**
📈 Including an accessibility statement and inviting feedback are great ways to continuously improve.

### a11y for society

🌟 Can help reduce social isolation and improve mental health for people with disabilities.
🌟 Not being excluded and instead able to participate, creates opportunities.

> When accessibility is a priority, _everyone benefits_

Marcy's suggestion for more reading:
📖  "Development and implementation of a framework for estimating the economic benefits of an accessible and inclusive society." from the journal of Equality, Diversity and Inclusion. 📖 

## Lesson 2 - Why is Accessibility important?

### Accessibility as a Moral Case
> Doing it because it's the necessary and right thing to do

No access = people with disabilities may not:
1. Access your site
2. Perform actions independently and securely
3. Participate in society in a digital world
4. Make valuable contributions in the workplace

 
**Do you feel like you are swimming upstream when pushing for accessibility as the right thing to do?** 😫   
Then it can be very helpful to appeal to the business case for accessibility. 📈

🏛 Marcy also refers to the [capitol crawl](https://www.history.com/news/americans-with-disabilities-act-1990-capitol-crawl) in 1990 where people demonstrated and crawled up the steps of the U.S Capitol in DC to show how people with disabilities are impacted by lack of physical access in public spaces. (this lead to the legislation, "Americans with Disabilities Act", ADA) 🏛

Similarly, if accessibility isn't prioritized within the digital space, then the impacts are echoed.

### Accessibility as a Business Case

The following stats are mentioned and they are mostly focused on the US, but also the whole world:
> - At least **one billion people** - 15% of the world's population, have a recognized disability.
> - People with disabilities & the people in their lives control **$8 trillion** in spending
> - The disposable income for working-age people with disabilities is **$645 billion**, and is comparable to other market segments like African Americans and Hispanic people.
> - There were **4055** ADA-related lawsuits filed against businesses in the United States in 2021 - 15% increase from 2020.

You can make use of these numbers if stakeholders are more likely to listen to 💰💸🤑

### Accessibility: The Legal Situation

There are different laws protecting digital civil rights, depending on where your team is located.

**USA 🇺🇸**
- Americans with Disabilities Act (ADA)
- Air Carrier Access Act (ACAA)
- Communications and Video Accessibility Act (CVAA)
- Section 508

> Section 508 of the Rehabilitation Act in the United States requires that any software created or procured by the government is accessible. - Marcy Sutton

⚠️ It is **currently not an ADA requirement** to follow the Web Accessibility Initiative's WCAG standards. ⚠️

**Canada 🇨🇦**
- Accessible Canada Act
- Accessibility for Ontarians with Disabilities Act (AODA)

**EU 🇪🇺**

- Web Accessibility Directive
	- Affects the public sector such as governments
- European Accessibility Act
	- **Will include the private sector.** → Implementation phase in member countries will run at least through **2025**. 

Marcy also recommends to follow Lainey Feingold for more about the legal situation of accessibility. 
📺 Highly recommends: [Feingold's talk from CSUN 2022](https://www.youtube.com/watch?v=Z4JPPbtveeI&ab_channel=CenteronDisabilitiesatCSUN) 📺

## Lesson 3 - Language & Definitions of Accessibility

Marcy highlights the importance of having a similar set of definitions for terms and acronyms that you encounter when working with accessibility. They fall into three categories:
1. Technologies ⌨️
2. Guidelines & Standards 📜
3. Miscellaneous 🧰

Don't assume that everyone knows what these terms mean.

### Markup & Code definitions
- **HTML** - HyperText Markup Language
	- The structure for how we deliver content on the Internet.
	- HTML tags becomes Elements when loaded.
- **CSS** - Cascading Style Sheets
	- The style and presentation of our HTML documents
- **JavaScript** (JS)
	-  Makes sites interactive
	- You need JS for a lot things for accessibility
- **DOM** - Document Object Model
	- "... is the data representation of the objects that comprise the structure and content of a document on the web. HTML markup is turned into a DOM structure that CSS can style and JavaScript can manipulate."
	
- **SVG**
	- XML-based markup language for describing 2D based vector graphics.

### Browsers, Apps, and Tools definitions
- **API** - Application Programming Interface
	- Accessibility APIs represent objects in a user interface, exposing information about each object through the operating system platform and web browser to assistive technologies. (they differ)
		- Test screen readers on the most commonly used devices and browsers. (for example if most of your users are on iOS and Safari)
- **User agent (UA)** is a line of text identifying the browser and operating system to the web server. 
	- Nowadays we a lot more feature-based identifiers supported in CSS (like prefers-reduced-motion etc.) so we don't need to check the User agent as much.
	- The keyboard focus default styles in the browsers can differ for example. So you might need it for that.
- **Assistive Technology (AT)** 
	- "... refers to hardware and/or software that provides functionality to meet the requirement of users with disabilities that go beyond those offered by main stream user agents."
	- **Examples:**
		- Screen readers
		- Braille displays (being able to read the output from a screen reader with your finger tips instead of your eyes)
		- Magnifiers
		- Text-to-speech
		- Speech recognition
		- Alternate keyboards
			- switches = small number of keys (maybe 3) 
				- There's different switch interfaces you can enable.
			- sip & puff = control your computer with a joystick that you control with your mouth.
				- etc.
- **Client Rendering**
	- Can impact accessibility.
		- Example: Navigation → Client-side routing
- **Native Mobile**
	- Apps built for native mobile devices, iOS, Android etc.
	- WCAG applies to Native Mobile.
		- WCAG is very web-focused so it can be confusing trying to apply it to native mobile apps.
		
- **Hybrid Apps**
	- Compiles to a native application, but is built with web technologies.
	- Examples: Ionic, React Native, Apache Cordova (formerly known as PhoneGap), NativeScript.
	
(Side note: I've actually built apps with PhoneGap a few years ago 😅😅)
- **Electron**
	- Build desktop applications using web technologies
	- Example apps: Slack, VS Code, Microsoft Teams
- **Progressive Web Apps**

### Standards & Miscellaneous definitions

- **W3C** - World Wide Web Consortium 
	- Develops and maintains web guidelines & protocols.
	- Standards
		- HTML
		- CSS
		- SVG
		- WCAG
		- XML
		- HTTP
		- and loads of APIs.
- **WHATWG** - Web Hypertext Application Technology Working Group
	- Another community working on similar things as W3C.
	- Since 2019 → works together with W3C on a single HTML standard.
- **WCAG** - Web Content Accessibility Guidelines
	- Published by the Web Accessibility Initiative of the W3C.
	- 4 categories:
		- Perceivable
		- Operable
		- Understandable
		- Robust
	- Current version: **2.1** (2.2 due out late 2022)
	- A complete rethink of WCAG called "Silver" (or version 3.0) will also come at some point.
- **ARIA** - Accessible Rich Internet Applications
	- also called WAI-ARIA (from the Web Accessibility Initiative)
	- Includes a standard set of roles, states and properties that provide accessibility information in web pages and apps. 
	- Current version: **1.1**
- **Normative**
	- "Official and approved wording in a specification; holds up in a court of law. In contrast to non-normative, or an informative aid that is often used in explanations or tutorials."
	- Example: WCAG 2.1
- **Conformance**
	- "Conformance to a standard means that you meet or satisfy the requirements of the standard. A.k.a 'compliance'. "
	
- **Inclusive Design**
	- "A design process that considers the full range of human diversity with respect to ability, language, culture, gender, age and other forms of human difference."
	- Accessibility is a measure of how we implement this with guidelines and best practices.

## Lesson 4 - What Makes a Website Accessible?

Regardless of what kind of role you have, there's a few areas that can help you create accessible websites (& apps):
1. Inclusive & intuitive design 🎨
2. Headings, landmarks and semantic structure 🏛
3. Text alternatives 🙏
4. Accessible forms 🔢
5. Interactivity 🖱⌨️

### Inclusive & intuitive design 🎨
Here's a few things designers should take into consideration to make things more accessible:

- **Colors** → Bold contrast between foreground and background so text is discernible 
- **Layouts** → should accommodate (and reflow with responsive web design) for variable text size and zooming. 
	- Reflow is a success criteria for accessibility
- **Touch targets** → appropriately sized for human fingers and movement. (example: buttons & links)
	- It can, for example, be difficult to click something when you have a tremor and are using a mouse.
- **Obvious designs for interactive controls**
	- Not assuming that people know what icons means.
	- Avoid design anti-patterns
- **Allow users to opt-in to motion and animation**
	- Motion & animation can, for example, give some people migraine.

### HTML Headings 🏛

> Using proper HTML headings creates a content hierarchy that screen readers can navigate. - Marcy Sutton

🌟 **Important:** Only one h1 per page and heading levels should increase in order. (h1, h2, h3... h6)🌟

🚨 **Common mishandling of HTML headings**🚨
> Another common mishandling of HTML headings is choosing them based on text size. Styling text is a job for CSS, not HTML! - Marcy Sutton

### HTML Landmarks 🏛
> HTML5 landmark elements give us a method to wrap our content into semantic sections. - Marcy Sutton

Marcy says that all of the content on our page should be wrapped inside one of the landmark elements that are supported by newer browsers:
- `<header>`
- `<nav>`
- `<main>`
- `<footer>`
- `<section>`
- `<article>`
- `<aside>`

**Takeaway:** These elements allows people using AT to navigate & understand our content even if they can't see the screen.

📖 (More about these can be found in the "Semantic Markup with HTML & ARIA" workshop.) 📖 

### Other semantic elements 🏛
Examples:
- `<p>`
- `<ul>`
- `<ol>`
- `<button>`
- `<table>`
- `<figure>`
- `<hr>`

Developers often tend to build their own custom buttons and tables and this either leads to a lot of work needed to support AT or that accessibility concerns are ignored.  It is **not recommended** 😅

>Using semantic elements can provide better support for assistive technologies by default. It also ends up being less work than if we had to add accessibility features manually.
>
>Work smarter, not harder! - Marcy Sutton

💡 Assistive technologies (AT) can navigate by collections of semantic elements. 💡

### Text alternatives 🙏
> Graphic and non-text content needs to be described so it can be used in formats people need, such as large print, braille, speech,  symbols or simpler language. 
> 
> Every purposeful image you use on a website needs to include meaningful alt text. - Marcy Sutton

Marcy recommends [W3C's alt decision tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) if you need to figure out what to write in the alt attribute of your image.

Streaming video & audio content have additional considerations which are required:
- **Captions** as an alternative to spoken word
- **Audio descriptions** that describes what's on screen beyond the dialogue (some shows on Netflix have this)

**Live sign language** is also a text alternative.

### Accessible web forms 🔢
> Forms are a huge part of the web. They’re also one of the most common things that are done wrong. - Marcy Sutton

The above quote says it all, I think. A lot of things on the web revolves around submitting information and for some reason we as developers tend to create UI that does not even utilize the power of the `<form>`-element.

Then why does this happen so often? Marcy mentions one reason:
- It can be tricky to style! 🧑‍🎨

So don't do a custom form with divs! Use `<form>` and get lots of accessibility wins for free.

>The good news is that browsers are, they're working on making these things easier to style.
>Fingers crossed, especially select, ‘cause that one has been the hardest. - Marcy Sutton

🌟 **Side note from myself:** I know that Remix promotes the use of the [FormData Interface](https://developer.mozilla.org/en-US/docs/Web/API/FormData) and I guess that it will become more popular and be very useful when interacting with `<form>`s in for example React.

Form markup gives semantic structure:
- `<form>`
- `<fieldset>`
- `<legend>`

Other elements:
- `<input>`
	- radio button
	- checkbox
	- text field
- `<select>`
- `<label>`

One of the most important accessibility features that is mentioned is adding labels to inputs and textareas in a form. 

**Benefits of labels 💁ℹ️**
1. When properly bounded to an input you'll get an increased interactive click area (you can click the label) that will automatically focus you into that input.
2. Screen readers will inform users what the control is for (e.g. "do they want my address or my email?")

🚨 **What to avoid** 🚨
1. Hiding label text with CSS
2. Using input placeholder as label text. (as soon as you start typing you might forget what you are supposed to enter in that field)
3. Losing accessibility due to custom styling 

**Validation** ⚠️
Any sort of feedback that is given to the user when submitting a form incorrectly, should be accessible, so the error text needs to be exposed to assistive technologies.

### Interactivity goes beyond the mouse 🖱⌨️
>Anything that a mouse user can do, a keyboard and screen reader user must also be able to do.
>
>This doesn’t necessarily mean that the interaction needs to be exactly the same, but there should be a way for someone using assistive technology to complete their task. - Marcy Sutton

Some HTML elements provide keyboard interactivity for free! 🥳
- `<button>`
- `<a href="...">`
- `<details>` & `<summary>`
	- kind of a native accordion!

Keyboard-operable elements can be reached with the "Tab"-key.

## Lesson 5 - Accessibility Needs All of Us
In this lesson Marcy encourages everyone to contribute to the accessibility community. 

Small excerpt that I found motivational:
>It can be a struggle sometimes but keep at it. Your persistence and sharing of knowledge with your colleagues can make an impact.

Let's make the web more accessible! 🥳

## Useful resources
- [Alt decision tree - W3C](https://www.w3.org/WAI/tutorials/images/decision-tree/)

TODO: Add more useful links from each lesson here.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4MDgyODA3NiwxNTM5ODQ1MzIyLDg1Mz
EyNDk0Myw2NzU1NzA2MTEsLTM5NzE5OTcyOSwxNzg3MTI5MzA5
LDEzMzk1NzI2MzYsLTk4MTE4MTY3NiwtOTQ4NzM3MzIzLDIwMT
g1MDExMzcsMjg1NTI2NDAxLDMyNDg0MjI2NCwtMTA4MzAzNTgw
MiwxMjYwMTY5MzgzXX0=
-->