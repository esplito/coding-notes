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

> The goal is to prevent access barriers from being passed on to development when they could have been tweaked or rethought earlier. This can cut down on the number of feedback loops involved with creating an accessible solution, thus saving time and money in the long run (and who doesn’t love that?). - Marcy Sutton

#### Example of tools for collaborating on accessible code prototypes
- Design or wireframes to get the discussion going
- HTML, CSS & Javascript prototypes
- Parcel for bundling stuff together quickly
- Codepen for sharing it
- Storybook for more advanced UI development (without having to create an actual application)

>💡Tip
Storybook offers an official [a11y addon](https://storybook.js.org/addons/@storybook/addon-a11y)  powered by Deque's [axe-core](https://github.com/dequelabs/axe-core), which can automatically catch up to [57% of WCAG issues](https://www.deque.com/blog/automated-testing-study-identifies-57-percent-of-digital-accessibility-issues/).

#### Terms you might hear while collaborating 🗣
- **Design system**: a complete set of standards, principles, docs,  UI patterns and components (that help fulfill those standards).
- **Pattern library**: represents components in a design system.
- **Style guide**: a document with rules/standards for graphic style (such as color, typography, icons, images). Also includes brand principles on how to use it.

There's opportunities to highlight accessibility in the above tools. One example would be to state in your style guide how you should use headings in an accessible way.

A truly accessible design system would include accessibility through out.

#### Takeaway🚨 
>The main takeaway is that looking at individual components and documentation are both opportunities for accessibility to be improved, hopefully not forgotten. - Marcy Sutton

Opportunities to get accessibility right in different building blocks of a design system:
-   Color and design aesthetics
-   Type readability
-   Semantic structure within components
-   Text alternatives
-   Interactivity
-   Configurability and flexibility
	- That designs are modular and responsive
-   Documentation

### Lesson 2: Ensuring Accessible Interactions

#### 💡 Tips to approach interactivity with accessibility in mind 💡
> 💡 **Anything mouse users can do, keyboard and Assistive Technology users must also.**
> 💡 **Make interactive controls obvious.**
>  💡 **Keyboard focus indicators must be visible.**
>  💡  **Use a minimum target size of 24 x 24 pixels.**
>  💡 **Beware  [changes of context](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html#context-changedef "Link opens in a new window")  on focus or input.**
>  💡  **Include ways to extend or adjust time limits.**
>  💡 **Content shown on hover or focus must be dismissible and persistent.**

#### Recommendations
- Study these in the Web Accessibility Guidelines
- At the very least take "baby steps" like ensuring:
>-   Interactive controls are keyboard focusable and operable
>-   Visible focus indicators are present
>-   Interactive touch targets are large enough for human fingers, including people with tremors

#### Affordances in Interaction Design
> 📖 An  _affordance_  is a term for what a user can do with an object based on their capabilities.

> 💡 It answers the question:  _What actions can they take?_

>💡Tip
>Accessible affordances can help you design products with good usability. When users know how to interact with your product right away, they don’t have to spend time learning your UI and can focus on what’s important: solving their problems.

Marcy mentions example of having other interactive elements than just swipe ("Tinder"-app for Pets had buttons and swiping). Swiping is problematic when using a screen reader. You need affordance in form of another type of UI element, such as buttons.

### Lesson 3: Identify Potential Accessibility Issues in Design Systems

**Goal of this lesson 🎯**
- Get you thinking about *potential accessibility issues* when reviewing design systems, mockups, layouts, and components.

**Includes challenges that provides opportunity to:**
- practice structuring feedback
- highlighting accessibility problems before they get baked into implementations.

Prerequisites:
- Have https://www.tpgi.com/color-contrast-checker/ installed.

>(CCA) is a free color contrast checker tool that allows you to easily determine the contrast ratio of two colors using an eyedropper tool.
> This desktop application works anywhere on your screen–not just the web browser.

#### WCAG Color Contrast Guidelines

WCAG 2 Guidelines to check when designing UI for varied color perception:

>-   **Body text:** with a minimum ratio of  4.5 : 1 and enhanced ratio 7 : 1
>-   **Large-scale text (120-150% larger than body text):** minimum ratio of  3 : 1 and enhanced ratio 4.5 : 1
>-   **Active user interface components and graphical objects such as icons and graphs:** minimum ratio of  3 : 1 and enhanced ratio not defined

🚨 *"Encourage your design team to provide real examples of interactive elements and typography. They should also be testing combinations for contrast themselves."* - Marcy Sutton 🚨

Marcy would recommend to use a contrast plugin in Figma to ensure that contrast ratio can be easily tested already in the design phase.

#### **🛠** Challenge: Find Color Contrast Issues
Identify color & contrast issues in this design.

![Interactive components from a design system](https://testingaccessibility.com/_next/image?url=http%3A%2F%2Fres.cloudinary.com%2Ftesting-accessibility%2Fimage%2Fupload%2Fv1655845415%2Fdesign-and-people-skills%2Fdesigns-as-prototyping%2Fidentify-potential-accessibility-issues-in-design-systems%2FUntitled_q6og1g.png&w=3840&q=100)

Issues identified by myself:
1. Subscribe has 2.5:1 in contrast ratio. It fails on all checks that CCA does.
1.4.3 (Contrast Minimum) AA: ❌
1.4.6 (Contrast Enhanced) AAA: ❌
1.4.11 (Non-text Contrast) AA:  ❌

2. Submit button fails on some checks. Has contrast ratio 3.4:1.
1.4.3 (Contrast Minimum) AA: (regular text) ❌  (large text) ✅
1.4.6 (Contrast Enhanced) AAA: ❌
1.4.11 (Non-text Contrast) AA:  ✅

3. The controls (checkbox etc.) also get same results as submit button (3.7:1 contrast ratio).

4. The slim toggle fails on the background that it sits, with a contrast ratio of 1.4:1.
5. Zoom icon does not meet AAA-requirements when highlighted. It fails on regular text. It passes on large text though.
6. "New camp spot" text in the toolbar fails on all checks in CCA
7. The dividers also fails on all checks in CCA
8. Same goes for the text in the text boxes.

Notes made from the solution:
- (Submit button text) "... for button text larger than 24 px, it would pass with a ratio of 3:1."

> Sampling colors with CCA is a useful way to get early feedback. I recommend that you share this tool with your design team so you can collaborate and learn together. - Marcy Sutton


#### **🛠** Challenge: Find Iconography & Label Issues
> The following challenge requires you to identify one overarching issue with iconography in the design system and another major issue with text labels.

**You don’t need any tools for this challenge.**

![Components from a design system](https://testingaccessibility.com/_next/image?url=http%3A%2F%2Fres.cloudinary.com%2Ftesting-accessibility%2Fimage%2Fupload%2Fv1655922459%2Fdesign-and-people-skills%2Fdesigns-as-prototyping%2Fidentify-potential-accessibility-issues-in-design-systems%2FUntitled_17_ni4g9l.png&w=3840&q=100)

Identified issues:
1. The design should answer the questions "What is the intent of these? And can any user of this UI understand that?". This design assumes that the user knows what the icons mean and that the user can distinguish what they are.
2. There's nothing that indicates how and when these icons should be used.

Possible fixes: 
3. complement icons with text labels
4. Add accessibility annotations to the design so that we know what the expected implemented behaviour for these components is.

Notes from solution:
>- There is also an issue with showing additional controls on focus, which we discussed in the previous section on interactions and affordances.
>- Labels display text throughout the interface. Adding labels to buttons, menu items, and views helps people understand the current context they are in and what they can do next. Clear text labels are essential to the overall accessibility of a design system.
>- The form uses placeholders as labels, which means as soon as the user types a field there’s no indication of what it’s for.
>Having a persistent text label above each input would make this form more accessible from a cognitive perspective.

#### **🛠** Challenge: Find Content Layout Issues
>Your challenge is to identify possible issues with these two layouts and discuss your expectations of what you will implement as a developer or communicate with a design team.

(Rich text editor + rendered article)
![Layouts from a design system](https://testingaccessibility.com/_next/image?url=http%3A%2F%2Fres.cloudinary.com%2Ftesting-accessibility%2Fimage%2Fupload%2Fv1655922504%2Fdesign-and-people-skills%2Fdesigns-as-prototyping%2Fidentify-potential-accessibility-issues-in-design-systems%2FUntitled_20_vhexsn.png&w=3840&q=100)

Identified issues that I would want to discuss with other developers and designers:
1. Icons without labels in rich text editor
2. There seems to be no way in indicating/defining heading levels. What is the expected heading levels and how do I know which headings that I'm editing?
3. There's tons of contrast issues in the rich text editor. I would suggest to have a better contrast between the rich text boxes and the background. Also a better contrast between the text in the boxes and the box color.
4. Labels inside rich text elements. I would suggest to put them + some describing text above each rich text box.
5. There's no way to describe the images with alt text.

Notes from solution:
>- For the rendered version of the article, it would be unclear to users if the three boxes are static content or if they are interactive. Some subtle treatments could be added to make it more evident that they are interactive, without relying on a hover and focus state to indicate as such
>- The editor layout contains several contrast issues. Testing and increasing the contrast of this main heading, subheading, and the `H1` would be beneficial. All of these texts look very faint. 

>💡Tip
> Collaborating, providing feedback, and influencing design earlier in the process saves you from trying to shoehorn fixes later in the development cycle.

### Lesson 4: Accessibility Feedback Loop Discussion
> At your organization, where in the software development cycle does accessibility feedback happen?

🎯 We should strive to integrate it as early as possible in the cycle. -> this means that we have to learn to collaborate with teams across the organization.

#### 📝 Exercise: Feedback Loop Reflection
**Q:** When in the cycle is accessibility feedback provided to the design team? What about the dev team?
**A:** Almost never. It only happens if a developer or designer is interested in accessibility. I try to raise accessibility issues to designers as soon as I get a Figma Design. I can count on one hand the number of times that someone has raised accessibility issues to the dev team (once or twice by the design team). I would love if Product Management, Content Management & Design raised accessibility issues to us.

**Q:** How successful is the accessibility feedback loop for your current processes?
**A:** There's no standardized feedback loop. So I would say not very successful.

**Q:** What processes would you like to try?
**A:** I would like to:
1.  Incorporate accessibility annotations in the design process in Figma.
2. See that Product Management set accessibility requirements from the start so that PMs & designers have tackled a lot of issues before the devs are involved.
3. See that everyone uses a Contrast Ratio plugin or other accessibility plugins in Figma.

## Section 4 - Using animation & motion safely

>Animation can have profound accessibility implications, inducing a variety of responses in some people including motion sickness, distraction, seizure risk, and more.

>In this section, we'll discuss how to use motion and animation, but most importantly, we'll discuss how to use them safely.

### Lesson 1: Use animation & motion safely

Marcys recommendation:
>-   Not making assumptions that everyone wants motion.
>-   You can make the thing move but give users control to turn it on or off. In fact, make motion opt-in and not the default.
>-   Your UI animations should be designed with purpose and have a reason for being there!

📚 Recommended reading: https://source.opennews.org/articles/motion-sick/

### Lesson 2: Implementing Prefers Reduced Motion

>If you do have large amounts of movement that cover a lot of visual real estate, it is **strongly recommended that you offer an option to turn off, or reduce, motion**.

>The `prefers-reduced-motion`  [media feature](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries#media_features) is used to detect if the user has requested that the system minimize the amount of motion it uses.

Here's how you can enable it on your Mac:
To enable this, you can follow these steps:

1.  Click Apple menu > System Preferences, click Accessibility, then click Display.
2.  Select Reduce motion.

#### Example of how to target this in CSS
```css
@media (prefers-reduced-motion: reduce) {
  .animation {
    animation: none;
    transition: none;
  }
}
```

#### Example in Javascript
```js
var motionQuery = matchMedia('(prefers-reduced-motion)');
function handleReduceMotionChanged() {
    if (motionQuery.matches) {
        /* adjust 'transition' or 'animation' properties */
    } else {
        /* standard motion */
    }
}
motionQuery.addEventListener('change', handleReduceMotionChanged);
handleReduceMotionChanged(); // trigger once on load
```

💡 If you want an *opt-in approach* you can use `prefers-reduced-motion: no-preference` instead!

### Lesson 3: WCAG Success criterion for Motion
Some motion-related WCAG guidelines that Marcy highlights:
-   [**2.2.2 Pause, Stop, Hide**](https://www.w3.org/TR/WCAG21/#pause-stop-hide)  - Level A. *"This one is huge for people with ADHD and other conditions that make moving websites hell to navigate and use."*
-   **[2.3.1 Three Flashes or Below Threshold](https://www.w3.org/TR/WCAG21/#three-flashes-or-below-threshold)** - Level A. There is also a Level AAA criterion with no threshold. 
-   [**2.3.3 Animation from Interactions**](https://www.w3.org/TR/WCAG21/#animation-from-interactions)  - Level AAA. *"While a Level AAA criterion, this one typically isn’t complicated to meet by making transitions and animations respond to reduced-motion settings. So why not do it?"*

### Lesson 4: Motion on website challenges
#### 🛠️ Challenge: Identify Accessibility Issues Related to Motion
>Visit the CampSpots Events page (”Plan Your Trip” then “Events” from the menu), then take a couple of minutes to write down your reflection on the following questions for each example:

**Q: What would be the impact on people when things are moving/animating on the screen?**
A: 
- You might get overwhelmed and have difficulties seeing the actual content that you are interested in. 
- There are no controls for pausing/playing the video that is in the hero.
- The rotating sun is distracting and can't be stopped either.

**Q: How should these issues be addressed?**
A: 
- Add controls to the video-element and do not make it autoplay by default.
- implement `prefers-reduced-motion` so that no animations are running when that is turned on (such as the rotating sun)

From solution:
>💡Tip
Under WCAG guidelines, any media that autoplays should have controls that allow the user to pause or stop playback. A best practice is to not make content autoplay at all.

Page tested: https://workshop-design-people-skills.testingaccessibility.com/plan-your-trip/events.html

#### 🛠️ Challenge: Add Prefers Reduced Motion to a Video
> Your challenge is to write a JavaScript function that will conditionally apply the `autoplay` attribute to the video player based on whether or not the user has indicated they want to reduce motion.

I created the following (quickly) and I've tested by emulating `prefers-reduced-motion` in Chrome. (I could possibly have done it the other way around set `prefers-reduced-motion: no-preference)` and instead `if(media.matches)`)
```js
function setAutoplayBasedOnPreference (videoElement) {
  const media = window.matchMedia("(prefers-reduced-motion: reduce)");
 
  if (!media.matches){
   videoElement.setAttribute('autoplay', '');
    videoElement.play();
  } 
}

const video = document.querySelector('video')

setAutoplayBasedOnPreference(video);
```
I also realized now that this will not respond to if I change preferences and do not reload the page. [The solution suggested by Marcy](https://codepen.io/marcysutton/pen/WNzjBwg) would be better I believe (using eventlisteners):
```js
const video = document.querySelector('video')

var motionQuery = matchMedia('(prefers-reduced-motion: no-preference)');

function handleReduceMotionChanged() {
    if (motionQuery.matches) {
        /* standard motion */
        video.setAttribute('autoplay', '')
    } else {
        /* do not show motion */
        video.removeAttribute('autoplay')
    }
}

motionQuery.addEventListener("change", handleReduceMotionChanged)

handleReduceMotionChanged(); // trigger once on load
```

#### 🛠️ Challenge: Add Prefers Reduced Motion to an SVG
> Your challenge is to remove the spinning animation from the sun SVG.

I just updated the selector targeting the SVG to have this content instead (adding the media query inside the selector is possible due to SCSS):
```scss
.page-events svg {
  animation: rotation 10s linear infinite;
  width: 150px;
  margin-left: auto;
  display: block;
  @media (prefers-reduced-motion: reduce) {
	    animation: none;
	    transition: none;
  }
}
```

Marcy suggested to just add this (which would work in regular CSS too):
```css
@media (prefers-reduced-motion: reduce) {
    .page-events svg {
	    animation: none;
	    transition: none;
	}
}
```

## Section 5 - Finding answers to accessibility issues
> This module focuses on developing strategies for finding answers to accessibility issues and how to feel confident in those answers. You will also learn how to build that accessibility "muscle" to understand user impact and solutions.

### Lesson 1: Finding answers to accessibility issues
> The big caveat here is that we all have our own biases, as discussed earlier.
> Part of finding answers is “knowing what you don't know” and not automatically assuming that everything you find will be correct.

Marcy suggest to do the following when "building your accessibility muscles" 💪:
- 🧪 **Test, test, test!** Test & validate with people with disabilities so that you know whether something is a problem or not.
- 📚 Reference the [WebAIM mailing list](https://webaim.org/discussion/) & use it as a resource to find answers! 
- Pay attention to [WCAG success criteria](https://www.w3.org/TR/WCAG21/) + [“Understanding”](https://www.w3.org/WAI/WCAG21/Understanding/) documentation + [“How to Meet” quick reference](https://www.w3.org/WAI/WCAG21/quickref/).
- Look for reputable blogs (check Twitter/X, mailing lists & a11y slack) & be sure to do some fact checking first. The advice might be obsolete or incorrect
-   Read a11y tool docs and issue trackers on GitHub, for example  [NVDA issue tracker](https://github.com/nvaccess/nvda/issues)  and  [WebKit bug tracker](https://bugs.webkit.org/).

### Lesson 2: Testing tools and processes  for accessibility

Marcy recommends the following tools & processes:
- Render in a web browser 🌐
- Test controls with the keyboard ⌨️
- Use accessibility browser extensions 🔌
- Check color contrast (for example with CCA) 🎨
	- using (if you are on Windows) Windows High Contrast Mode can also highlight improvements
- Zoom in and out 🔎
- Test with screen readers 🔊

> 💡Tip
>There’s lots more on these topics in the Manual Testing workshop!

List of links with tools: https://workshop-resources.testingaccessibility.com/#workshops-testing

### Lesson 3: Using WCAG in an Organization

**Step 1: Decide on WCAG Standard & Level**
-   [WCAG 2.2](https://www.w3.org/TR/WCAG22/)  was published on 5 October 2023.
	- It's backwards compatible with 2.1 & 2.0
- [WCAG 3 Working Draft](https://www.w3.org/TR/wcag-3.0/) got published on 24 July 2023.
- Decision to make 🤝:
	- Does the organization strive for level AA or AAA?

> 💡 These decisions can impact your _Definition of Done_ as your team works through issues in JIRA, GitHub, or a similar tracking system.

🚨 You all need to work towards the same standard in order to effectively prioritize issues. 🚨 

**Step 2: Develop a systematic testing plan**
- Plan on testing often
	- Make sure that it fits into the workflow of your teams
- Build in the possibility to review accessibility processes regularly
	- 🎯 This can help ensure adequate coverage for manual & automated testing

> For example: in retrospectives or quarterly reviews, revisit whether your testing plan is working

🚨🚨 **Important!** 🚨🚨
> Accessibility testing is not a “one and done” situation!

**Step 3: Look beyond the standards**
WCAG is a good starting point, but it is not always "the best" and it does not always mean that things will be user-friendly. Look for other ways of improving accessibility beyond the standards.

Marcy suggests to look at Deque's resource “[Manage Accessibility through Organizational Policy](https://dequeuniversity.com/tips/manage-accessibility)”

### Lesson 4: Strategies for finding answers to accessibility issues

In this lesson we get to learn and try some strategies for finding answers to different accessibility issues.

> Whatever the information source, you need to be discerning in what you take in. You also need to be diligent in testing and validating a proposed solution to ensure that what you found actually solves a user-impacting problem.

#### Finding an Answer to a Problem Demo

##### The CampSpots Checkout Process
Test here: https://workshop-design-people-skills.testingaccessibility.com/plan-your-trip/passes.html

**Issues**
- Radio buttons in modal when clicking on a subscription, has keyboard access issues.
> The custom text field associated with the last radio button steals focus and removes the current selection from any other radio button. There is also no visible outline to indicate where focus is.

> 💡Tip
> Potential fixes can be code or design focused. Don’t rule either out!

##### 🛠️ Challenge: Finding an Answer to a Problem
🎯 Search the web for solutions to the problem above.
🎯 Spend maximum 10 minutes looking for an answer

> _Hint: The search terms “radio text input steals focus” is a good start._
> 
The problem is that if I want to donate 3 $ and select that with the arrow keys, after I've done that and press tab to go to the next things to select, it changes my selection to the custom text input. So how can that be solved?

Note: I tried to find an answer, but could not find a good one. I'll have to check how Marcy found a solution 😄

##### 🛠️ Solution: Finding an Answer to a Problem
The radio button keyboard issue would probably fail the [2.1.1 Keyboard Success criterion](https://www.w3.org/TR/WCAG22/#keyboard) so while investigating you could say that you are trying to fix a bug related to that criterion.

> "For this one, making it so that tab does not, like maybe this text input shouldn't be in the tab order if I haven't selected that radio button. That could be a potential solution. Add some JavaScript that makes that basically a read-only input until I use the arrow keys on, make the radio buttons the only thing that's interactive? And if I hit tab, if I haven't selected this particular radio button, let me skip by that text input. That's probably what I would recommend." - Marcy

> "As you're exploring, trying to find answers, know that sometimes code, if you're finding that you're really having to hack at it, a redesign could be warranted." - Marcy

## Section 6 - Establish a culture of accessibility
> 💡Tip
> Focus on accessibility because it’s the right thing to do.

> This section will teach you how to encourage a culture of accessibility across your team and your organization.
> 
>Ableism is discrimination in favor of able-bodied people. As you progress through this section, you'll learn how to start designing ableism out of systems.
> 
> Any validation you can do up front to try and get an estimate of how long it will take for issues to be fixed is something that product management and leadership love.

**What will I learn in this section?**
🎯 How to encourage a culture of accessibility across your team and your organization.
🎯 How to start designing ableism out of systems.
🎯 Strategies for scoping & prioritization accessibility issues so that they gain traction.

### Lesson 1: Create a culture of accessibility
**What will I learn in this lesson?**
🎯 How to get started with making a change across your team and your organization.




## Useful resources
TODO: Add more  useful links.

- [Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/)
- [The Web Content Accessibility Guidelines v. 2.1](https://www.w3.org/WAI/standards-guidelines/wcag/)
- [Accessible Brand Colors by Use All Five](https://abc.useallfive.com/)
- [Overlay Fact Sheet](https://overlayfactsheet.com/) 
- [Your Interactive Makes me Sick](https://source.opennews.org/articles/motion-sick/)


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ0MDE0OTkyOCwzNjY3MjY1MCwtMTM3OD
I0MDYxLC0yMDc0OTU0OTA3LC0xMjI5NTAzMzE4LC0xOTY0ODMy
MDc1LDk4OTk2NTQsMTM1NDk2MTI4LDE4OTA2NjQ5NTUsMTEwMz
gyNjQ0NywtODY2ODEyOTg4LDEzMzYyMTk5MjksLTY5ODU5NDYx
MCwxMTc3NjY5MjE5LC0xMDEwOTY2OTYwLC05MTk0NzgyMTYsLT
QyMDY3Njg1MSwxOTAzMTk4Mjc3LC05Mjc5OTEzMjAsMTcyNzA1
MjU4OF19
-->