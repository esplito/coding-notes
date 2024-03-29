# Design Thinking & People Skills for Accessibility

Github repo: https://github.com/testing-accessibility/workshop-design-people-skills

**Table of contents**
- [Section 1 - Intro & overview](#section-1---intro--overview)
- [Useful resources](#useful-resources)

## Section 1 - Intro & overview

**How do we get accessibility to be successful?**
By making it a _core part_ of the product design process and addressing it earlier than development.

**What will we learn in this module?**
- Accessibility in the **different parts of the software development life cycle** (SDLC) ♻
- How to **advocate and expand** accessibility **across more roles** than developers. 📢
- The ability to recognize what's at stake without digital access and **the impact it could have if we don't make things accessible**. 🛑
- How to **collaborate in the design phase** → to share feedback earlier so that it not just coded with accessibility in mind, but also designed with it in mind! 🎨
- The ability to **solve problems and validate ideas before implementing** them. ✅
- The mechanics of **building a culture of accessibility** across your team and organization. 🙏

### WCAG Overview
>An essential piece of the puzzle is WCAG. Getting to know [The Web Content Accessibility Guidelines v. 2.1](https://www.w3.org/WAI/standards-guidelines/wcag/) will be critical to working with accessibility on teams. - Marcy Sutton

When Marcy recorded the video, WCAG Version 2.2 was estimated to be out in September 2022 (it is now estimated to be out Dec. 2022).

Guideline categories:
- Perceivable 👀
- Operable 🕹
- Understandable 🤔
- Robust 💪

In the course we'll treat WCAG as a policy rather than a strict requirement.

**What will be covered?**
- How to decide which version to choose for your organization.
- How to find answers to accessibility questions (even WCAG feels vague/complex)
- WCAG Silver (complete rethink of WCAG)
- How to stay up to date with upcoming changes

## Section 2 - Accessibility as User Experience
> After this section, you will be able to **identify accessibility problems elsewhere in the organization** and figure out **how to persuade the stakeholders in power positions effectively**. - Marcy Sutton

### Lesson 1 - Accessibility Issues Outside the Development Phase
> In this lesson, we’ll talk about common “non-code” accessibility situations and approaches for solving them. - Marcy Sutton

#### Inaccessible Brand Colors
It is quite common that brand colors don't meet requirements for contrast → they don't have sufficient contrast between foreground and background colors.

💡 [Accessible Brand Colors by Use All Five](https://abc.useallfive.com/) can be used to find colors that work better together with accessibility in mind 💡

**How can fix this is we run into this issue?**

✅ Talk to brand designers and other stakeholders and make it clear that the colors will need to be changed.

The outcome might be:
- You need another set of colors
- Designers need to set rules for when certain colors can not be combined.


#### Mobile Accessibility Issues
> We can't fix everything with code and mobile interfaces also need to be taken into account. - Marcy

Marcy mentions an example where we run into the problem that some action can only be done with swiping left or right. It would be accessible to do the same actions with labeled buttons while a mobile screen reader is running, but they would have to _be designed_ into the interface -> significant change.

She gives the following tip to avoid such a situation:
>💡 **Tip** When the design team knows how to anticipate accessibility issues, these problems can be avoided before development begins. 


#### Icon Buttons without Text Labels

It is a common design trend to use buttons that only consist of an icon with no text.

There's a few problems with this:
🚩 Not always clear to the users what the buttons do
🚩 Icon button can become hard to see if it is overlapping an image or video.

>Icon buttons need to ensure that their function can be clear and discoverable. - Marcy

Some good suggestions on how to handle when we as developers are handed a design with such icons:
💡 Talk to the design team about available options.
💡 Test with users to make sure the icons are intuitive and usable.
💡 Make text labeling configurable.

**But what if stakeholders already have signed off on a design?**
💡Tie the concern back to user experience and aim to validate the usability of the designs with actual users.

#### Embedded Content with Accessibility Problems
Have you ever had to add third party content on a web page? If so, you probably know that this often means adding an iframe which contains content that you have no control over.

🚨 They can bring their own **accessibility issues to your website**. 🚨

What can you do? 
💡 Talk to the marketing or product leadership teams about pitting potential added value vs access and compliance.

>...unfortunately, the reality is that many third-party vendors don't have the knowledge, care, or time to do so. - Marcy

#### Inaccessible Overlay Solutions

There is something called "overlay widgets" that some sites offer in an attempt to make the user experience more accessible without a lot of investment. 

However, these overlays are note enough to be compliant and often don't fix accessibility problems. 

Marcy recommends to check out the [Overlay Fact Sheet](https://overlayfactsheet.com/) if you want more information about strengths and weaknesses of overlay widgets.

>As someone passionate about designing and developing inclusive sites from the beginning, the add-on culture goes against that philosophy. Bolting on a quick fix for the long term rather than architecting something accessible from within is a real shame, especially since feedback from people with disabilities has been that the quick fixes don’t work. There are multiple layers to the problem here. - Marcy

### Lesson 2 - Including Accessibility from the Beginning of the Software Development Cycle

#### "Shifting Left" in the Software Development Lifecycle
This lesson revolves around shifting accessibility to left in the software development lifecycle, e.g. as early in the process as possible. 
>From the early stages, it is essential to include people with disabilities as part of the audience (and project team, for that matter).
>
>We want to avoid making accessibility an afterthought. - Marcy

The stages mentioned in the lesson:
-   Requirements Gathering & Planning
-   UX & Content Strategy
-   Design
-   Development
-   QA

Catching accessibility issues in the QA-phase can be extremely costly. It would be better if accessibility was thought of early in the process instead of QA having to report back to devs, designers and stakeholders that then have to rework the feature.

#### Disability impacts all of us
> There are so many different ways that people move through the world and interact with computers. This includes people with disabilities.

**Some stats mentioned**
_1 in 4_ US adults has some kind of disability:
- Vision
- Movement
- Hearing
- Thinking
- Learning
- Communicating
- Mental health
- Social relationships

#### Test user interfaces with People with Disabilities
> Teams almost always have hiring biases and barriers that lead to nearly every member being non-disabled. This means that teams’ experience is limited when it comes to knowing how an inaccessible digital experience will affect disabled people. - Marcy

How can we solve teams' limited accessibility experience then? Marcy suggests:
💡 Prioritize hiring of people with disabilities is an important start
💡Pay people with disabilities for their time and expertise to give feedback in your design and dev process (another effective way to remove biases and challenge assumptions)

You can work with companies like "Fable Tech Labs" or "Access Works".

### Lesson 3 - Identifying & Addressing Accessibility Issues on a Website

#### 🛠 Challenge: Find Accessibility Issues using Keyboard-Only Navigation
>Your first challenge will be to identify potential issues on the CampSpots Careers Portal using only the keyboard.
>
>As you work through this challenge, ask yourself:

>_How efficiently can someone navigate the site with only the keyboard? Would someone be able to complete their task?_
https://workshop-design-people-skills.testingaccessibility.com/careers-portal/index.html
###### Useful commands for navigating with keyboard ⌨
>The following keyboard commands are fundamental to navigating the web:
>
>-   Tab to go forward through interactive elements
>-   Shift + Tab to go backwards
>-   Spacebar to scroll down (Shift + Space to scroll up)
>-   Enter to submit forms and activate widgets
>-   The arrow keys
    -   Up and down to scroll the page
    -   Left, right, up, and down to navigate through some form controls and custom-scripted widgets

**I found the following issues:**
🚨 I get no UI indicating focus when tabbing on the page. It seems like there is two links that are focusable (because I see a link description in bottom right corner of the browser).
🚨 Very few elements that are focusable, but the UI seems to have lots of links that *should be focusable*.

With Devtools:
🚨 It's just divs all the way. So not a surprise that the page is so inaccessible with a keyboard.

#### 🛠 Challenge: Find Low Contrast Text Issues using Chrome DevTools
>Some people with low vision have trouble distinguishing digital text and graphical objects when certain color combinations are used.

>A contrast ratio measures the difference in brightness between the foreground and background colors of a web page.

**For this challenge, make a list of what you suspect are color contrast issues on the Careers Portal.**

**I found the following issues:**
🚨 I think the links in the menu have a bit too low contrast against a white background. Same but reversed on the blue card at the bottom of the page.
🚨 I believe the light gray descriptions have contrast issues too.

**I also tested it with the accessibility audit that Lighthouse has and got similar issues flagged.**

##### Measuring with DevTools contrast ratio (solution)
>Notice that the styles pane shows us a contrast ratio of 2.74:1 (read as 2.74 to 1) and a “no” icon.

>Compare this to the recommended WCAG AA level of 4.5:1.

**Side note:** there's also issues with neighboring colours that are very similar in the chart (different shades of green) that can difficult for some to distinguish.

#### Lesson Recap
>The main takeaway is that discovering issues is only the start: you also want to feel confident those issues will have real user impact and not land in a sea of design and technical debt. 

>Part of this process involves developing your ability to communicate compelling issues with potential solutions so that they gain traction in the software development cycle.

Consider:
_What is at stake for people with disabilities when they can’t use a site or application?_

_How should these issues be addressed through product, design, and development?_

**Important:**
>Answering these questions will help you formulate a cohesive solution that can be communicated effectively across your team and organization.

### Lesson 4: Identifying accessibility bias
>Our abilities can definitely lead to biases. We want to be careful, so we're not painting our own experience as what everyone else would go through.

>Consider the following about yourself:

-   Physical abilities
	- My answer: I'm probably quite biased since I can use a mouse & keyboard, computer screen, smartphone etc. without issues. *I sometimes forget to test with keyboard or screen reader due to this.*
-   Cognitive abilities
	- My answer: Cognitive abilities is something that I very rarely think about while developing, but I'm trying to learn what I can do to be more proactive with creating software that adheres to different cognitive abilities.
-   Position and role
	- My answer: I think about this quite often. Since I'm a web developer (and have used thousands of websites) I know a lot of UX patterns & have tools to understand what is expected of me as a user when I use an application. It can for sure affect what I presume others know. That's why it's so important to do user research to guide the development. 😄

>**For example, does your ability to use a mouse every day mean that you forget to test with the keyboard?**

Some tips from Marcy:
>1️⃣ Pay people with disabilities for their time & expertise in full-time roles and through specialized engagements.
>2️⃣ Hire people with disabilities on your teams.
>3️⃣ Work with a company like  [Fable Tech Labs](https://makeitfable.com/)  or  [Access Works](https://access-works.com/).

## Section 3 - Designs as prototyping
> In this section, **you’ll learn how to review design systems, mockups, layouts, and components for potential accessibility issues**.

> Additionally, you’ll learn **how to approach collaborating on a design by leveraging different tools**. - Marcy Sutton

### Lesson 1: Collaborate on Designs & Prototypes


## Useful resources
TODO: Add more  useful links.
- [The Web Content Accessibility Guidelines v. 2.1](https://www.w3.org/WAI/standards-guidelines/wcag/)
- [Accessible Brand Colors by Use All Five](https://abc.useallfive.com/)
- [Overlay Fact Sheet](https://overlayfactsheet.com/) 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3Nzg5NTE5NiwtMTY3MzY2MzMxNSwtNj
I2ODc4MjgsLTE0MTgyNzEwOSwtMTQyMzMxMDAsNDIxOTQ3ODQx
LDg0NDk1NzQ2OCwtMjEzNzI1OTkzNywtNjY1NjU1NzA0LC04MD
c1ODQ4NTUsLTE3MjE3MjU5ODksLTg5MDU5MzI3MCwtMjQyMjg1
NTM1LC03MzU3ODM3NzgsMTYxOTM3NzAwNiw3ODkzMTAxNDYsLT
Q1NTU1Nzc0MCwtMTc5MTM5MzE4MywtMTkyNTc1NDQyMF19
-->