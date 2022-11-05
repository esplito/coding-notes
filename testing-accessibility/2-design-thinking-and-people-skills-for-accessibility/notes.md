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

## Useful resources
TODO: Add more  useful links.
- [The Web Content Accessibility Guidelines v. 2.1](https://www.w3.org/WAI/standards-guidelines/wcag/)
- [Accessible Brand Colors by Use All Five](https://abc.useallfive.com/)
- [Overlay Fact Sheet](https://overlayfactsheet.com/) 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg5MDU5MzI3MCwtMjQyMjg1NTM1LC03Mz
U3ODM3NzgsMTYxOTM3NzAwNiw3ODkzMTAxNDYsLTQ1NTU1Nzc0
MCwtMTc5MTM5MzE4MywtMTkyNTc1NDQyMF19
-->