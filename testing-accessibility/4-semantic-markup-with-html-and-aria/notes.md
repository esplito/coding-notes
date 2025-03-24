Semantic Markup with HTML and ARIA 

Github repo: https://github.com/testing-accessibility/workshop-semantics-html-aria

Deployed version of the workshop application: https://workshop-semantics-html-aria.testingaccessibility.com/

**Table of contents**
- [Section 1 - Intro & overview](#section-1---intro--overview)
- [Useful resources](#useful-resources)

## Section 1 - Intro & overview

>This workshop focuses on learning how to communicate what structure and relationships exist in your website through proper HTML elements (semantic markup) and ARIA use. - Marcy Sutton

> Semantic Markup and ARIA is the foundation of accessible structure. - Marcy Sutton

In this workshop we are working on the campsports application again, but it is now built in React:

>This site is built in React so you’ll get a chance to see what techniques and patterns come up when building out features and pages in a JavaScript-heavy environment (an area where accessibility is frequently forgotten or missed). - Marcy sutton

Note on the work in this workshop:

>A majority of our time working with semantics and ARIA will be on the Listing Detail page. - Marcy Sutton

## Section 2 - Build Page Structure Through Headings and Semantic Landmark Elements

> In this section, we will look at how to analyze the structure of a page and improve it to be more accessible to users.
### Lesson 1 - Analyze Heading Structure

Use [Accessibility Insights extension for Chrome](https://chrome.google.com/webstore/detail/accessibility-insights-fo/pbjjkligggfmakdaogkfomddhfmpjeni) and [Web Developer Toolbar](https://chrome.google.com/webstore/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm?hl=en-US) to analyze the heading structure.

> 💡Tip
>
>As an exercise, open up a page you work on or visit often and analyze the heading structure. What potential heading candidates do you see and how does that compare to what you find running these tools? 


### Lesson 2 -  Implement Semantic Headings for Page Listing Detail

> The important thing to take away here is that different components can have their own heading levels that are independent from those in other components. When various components come together on a rendered page, we need to pay attention to the overall outline. - Marcy Sutton

#### 🛠 Challenge: Add Heading Structure to Page Listing Detail

> Your challenge is to build a heading structure for Listing Detail page template in `page-listing-detail.js` that represents the visual content hierarchy of the page.


> At the top of every page’s structure should be a single H1.
>
> We’ve seen that our site is missing an H1, and we know that it needs to be above the H2s in the MegaNav.
> 
> This means that even though Cranberry Lake visually looks like a page title, it can’t be the page’s H1. - Marcy Sutton

What I did:
- Changed listing name to `h2`
```jsx
	<h2 className='listing-name'>{data.listingName}</h2>
```
- Changed description, amenities and calendar to `h3`
```jsx
<h3>Description</h3>
...

<h3>Amenities</h3>
...

<h3>Calendar</h3>
```

🚨 Note: Menu structure is subjective. Marcy chose to mark the MegaNav menu items as headings because they contribute major areas to the page structure. They also have buttons nested inside for interactivity which means that they both contribute to semantic structure and keyboard/screen reader operability for the MegaNav navigation. 🚨

> No matter which approach you take in your own projects, make a healthy heading structure a priority for pages, and remember to test with a screen reader! - Marcy Sutton

### Lesson 3 - Inject a Page Heading with a React Portal

Each page needs a **single h1** that describes the most important piece of content on the page.

It could:
- introduce the overall site
- the most important content on a page

The decision is subjective!

In this exercise we use React Portal to inject a visually hidden h1 outside the current component's DOM structure.

##### 🛠️ Challenge: Add an H1 with React Portal
I added the following inside the `Listing` component in `page-listing-detail.js`:
```jsx
<HeaderPortal>
<h1 className='visually-hidden'>Camp Spots - {data.listingName}</h1>
</HeaderPortal>
```

CSS for `visually-hidden`:
```css
.visually-hidden {
    clip: rect(0 0 0 0);
    clip-path: inset(50%);
    height: 1px;
    overflow: hidden;
    position: absolute;
    white-space: nowrap;
    width: 1px;
}
```

#### Summary

What does a good heading structure?
- Ensures that screen reader users are able to navigate our page efficiently

Note: Talk to other team members since there are subjective decisions about what should be headings. 

You might need input from different perspectives:
- SEO
- Marketing
- Information Architecture

### Lesson 4 - Implement Semantic HTML5 Landmark Elements

> ... landmark elements create signposts around major sections of our page content and are a great alternative to using only generic divs! - Marcy


In this lesson we should add top-level landmarks, which are landmarks that are not nested inside any other landmark. 

Examples:
- `header`
- `main`
- `footer`
- `aside`

`header` and `footer` can however be used for content nested inside `main` and `section`.

ℹ️ In this exercise Axe Devtools flags the `h1` as being rendered outside of a landmark even though we have wrapped the `header`around it. This is because it is rendered in a portal outside of the natural DOM order.

Marcy's take on it:
>As noted earlier, it is technically not a violation for an H1 to not have a landmark wrapped around it (even though axe DevTools classifies it as a best practice and “moderate”).
>
>We know our site has a healthy heading structure, which has a user impact that I think is more important than doing something to satisfy a tool.

 🤩 Remember this:
>Ultimately, the decision is up to you! You might need to get creative with solutions when faced with architecture constraints. Consider user impact and test with screen reader users to know for sure whether a solution poses a problem or not.

### Lesson 5 - Challenge: Implement Semantic Landmarks in Page Listing Detail

💡**Note about section **💡

> In some ways, `section` is the new div– it’s become overused in markup and won’t necessarily be announced in a screen reader by default as a result.
>
> To get the most out of the `section` element, we need to add an accessible name that will add it to the document outline and identify this particular section in Assistive Technology. We can use an `aria-label` with an arbitrary string or an `aria-labelledby` attribute pointing to an ID on another element to accomplish this. - Marcy

My markup after the exercise:
```jsx
<BodyClassName className="header-overlap page-listing-detail">
  <>
    <HeaderPortal>
      <h1 className="visually-hidden">Camp Spots - {data.listingName}</h1>
    </HeaderPortal>
    <main>
      <header
        className="page-header"
        style={{ backgroundImage: `url(${headerImageUrl}` }}
      >
        <div className="page-header-content wide-layout">
          <h2 className="listing-name">{data.listingName}</h2>
          <p className="location">{data.location}</p>
        </div>
      </header>
      <section
        className="wide-layout two-parts-70-30"
        aria-label="Site description and calendar"
      >
        <div>
          <h3 className="h4-style">Description</h3>
          <div
            className="description-text"
            dangerouslySetInnerHTML={{
              __html: sanitizeHtml(data.description),
            }}
          />
          <h3 className="h4-style">Amenities</h3>
          <div className="amenity-icons grid">
            {data.amenities.map((amenity, index) => {
              return (
                <div key={index}>
                  <Icon name={amenity} showText={true} />
                </div>
              );
            })}
          </div>
        </div>
        <div>
          <h3 className="h4-style">Calendar</h3>
          <DatePicker />
        </div>
      </section>
      <section className="wide-layout" aria-label="Image gallery">
        <div className="detail-images">
          {data.detailImages.map((image, index) => {
            let detailImageUrl = LoadedImageUrl(imageURLs, image.imageSrc);
            return (
              <img key={index} src={detailImageUrl} alt={image.imageAlt} />
            );
          })}
        </div>
      </section>
    </main>
  </>
</BodyClassName>
```

### Lesson 6 - Check Page Listing Detail Accessibility with Voice Over

#### Some tips
> 💡 Hit `Fn + CMD + F5` to activate VoiceOver (`Cmd + F5` on a desktop keyboard).

> 💡 Hit `CTRL + Option + U` to show the VoiceOver Rotor. (Cycle through with left and right arrow keys. Up and down navigates through items. Enter or space will select one.)

> 💡 Hit `CTRL` if you want VoiceOver to stop reading.

Important note about the use of `section` and screen readers:
>In order to have a section landmark show up in the screen reader, it needs to have an accessible name. The `aria-label` technique is one approach, but we’ll go more into this topic again later. - Marcy Sutton

### Lesson 7 - Explore More Semantic Landmark Element Options

Example of other semantic structures that provide additonal information:
- `ul` combined with `li` will read out something like "`<text of item>`item 1 out of 4 "

Marcy suggests to bookmark this page: https://developer.mozilla.org/en-US/docs/Web/HTML/Element

> I will caution that MDN and other docs aren’t always on top of current accessibility gotchas, so it is important to do your own testing to ensure pages work as expected with Assistive Technology. - Marcy Sutton

## Section 3 - Add Accessibility Information through ARIA

### Lesson 1 - What is ARIA and when to use it?

💡Tip: Bookmark and read more about the [ARIA 1.2 specification at w3.org](https://www.w3.org/TR/wai-aria-1.2/)

> The first rule of ARIA (Accessible Rich Internet Applications) is essentially "Don't use ARIA". Use native HTML elements first, instead.
>
> When the time comes and you do need it, you'll want to know you're doing it right.

There's additional information and rules at the following page: https://www.w3.org/TR/using-aria/

> When you apply ARIA, you have to test and make sure it works with Assistive Technology. Plenty of teams have learned about ARIA, then started adding it to everything without realizing that under the hood it was having a negative impact on accessibility.

**End goal of ARIA**: 🎯 Add accessibility information that is beneficial for screen reader users so that they can interact with a page and understand it. 🎯

More resources regarding ARIA here: https://workshop-resources.testingaccessibility.com/#workshop-semantic-html-aria

There's a [community-driven website called a11ysupport.io](https://a11ysupport.io/) that publishes support of different ARIA features in different browsers

### Lesson 2 - Add Implicit ARIA Roles to the Date Picker

#### Exercise: Interact with the Date Picker

- Can you reach and operate everything? 
	- **No I can't even reach the date picker** or reserve button when tabbing through the page.
- How are the months and dates announced?
	- There's no connection between months, dates or days when using VoiceOver. 
	- It also announces both "Monday" and the abbreviation "M". 
		- However, at least is says that they are a part of group, but I get no indication of what they are grouped with.
- Is it clear when a date is selected or already booked?
	- No, it says nothing about it when I use VoiceOver.

#### Implicit ARIA Roles

When you use elements like `<button>` you get implicit aria roles and other accessibility benefits. In this case `role` equals `button`. If I were to build the same thing with a `div` I would have to explicitly add `role="button"` and add interactive functionality that I get automatically when using `<button>`.

### Lesson 3 - Add ARIA States, Roles, and Properties to the Date Picker

#### 🛠️ Challenge: Implement ARIA Role, State, and Property for Date Picker Grid Items

> Your challenge is to increase the accessibility of the dates in the date picker grid. This can be accomplished by working through these three tasks:
>
>	- Each of the dates should be focusable and given a proper ARIA role.
>	- An ARIA state should be added that indicates whether or not a date has been chosen.
>	- Add an ARIA label property that enables the screen reader to announce the full date instead of just the number shown visually.

Read through [ARIA widget attributes](https://www.w3.org/TR/wai-aria-1.1/#attrs_widgets) to get a sense of what to use. `aria-pressed` can be used for buttons.

>💡Tip: Always verify the ARIA role, state or property that you’ve chosen to use is intended for the use-case that you have in mind.

Code after fixes:
```jsx
return <button 
aria-label={
          `${dayjs(day.date).format('MMMM D')}${isDaySelected(day) ? ' selected' : ''}`
      }
      aria-pressed={
          isDaySelected(day) ? 'true' : 'false' 
      }
      className={...}
      key={index}
      onClick={() => selectDay(day)}

	<time date-time={day.date}>{day.dayOfMonth}</time>
	<span className='icon' aria-hidden='true'></span>
</button>
```

##### 🛠 Challenge: Fix Date Picker Key Markup

- Update the "Booked", "Available", "Selected" info elements to use semantic elements.

Code after fixes:
```jsx
<ul className="date-key">
    <li className="date-key-item-wrap">
      <span className="date-key-item booked">
        <span className="icon" aria-hidden="true"></span>
      </span>

      <span className="date-key-text">Booked</span>
    </li>

    <li className="date-key-item-wrap">
      <span className="date-key-item available">
        <span className="icon" aria-hidden="true"></span>
      </span>

      <span className="date-key-text">Available</span>
    </li>

    <li className="date-key-item-wrap">
      <span className="date-key-item selected">
        <span className="icon" aria-hidden="true"></span>
      </span>

      <span className="date-key-text">Selected</span>
    </li>
  </ul>

  <button className="reserve-btn">Reserve</button>
```

### Lesson 4 - Test Date Picker Accessibility with a Screen Reader

There's still some issues:
- Previous and next months are announced as "less than June" etc.
- Toggling a date does not give immediate feedback

>The important thing is that we now have introduced programmatic accessibility information into the date picker.

>💡Tip: In the Coding Accessible Interactions & Mechanics workshop, you’ll learn techniques for enhancing the functionality and user experience of the date picker without sacrificing its accessibility features!

In Safari and VoiceOver the semantic information about the list that we added is stripped away. To fix that we need to add `role="list"` to the `ul`.

_Why is VoiceOver doing this in Safari?_ 
Because it’s relying on the presence of bullet styles, which we don’t have.

🚨 Note the following! 🚨
>There is a chance that after making this change that some accessibility testing tools will tell us that we’ve added an explicit role that we don’t need. But it’s better to have the screen reader work as intended than it is to leave it off to make a tool happy. 


## Section 4 - Craft Accessible Names to Expose the Purpose of Elements on a Page

>In this section, you'll learn the about crafting accessible names and descriptions for Assistive Technology. We will also discuss the best approach to working with labels and placeholders for forms, with gotchas that come up along the way.

### Lesson 1 - What is an Accessible Name?

Accessible names are used to label elements so that people using Assistive technologies can be informed of the purpose of those elements.

**Some ways to add accessible names**:
- `aria-label`
- `aria-labelledby`
- `title` ⚠
	> Adding a `title` creates a description that will be optionally read aloud after an accessible name (meaning it can create redundancy). It can also contribute to an accessible name as a fallback and is commonly used to label iframes. But it will only show up visually for mouse users. If you use `title`, be sure to test it.
	


The spec, [Accessible Name and Description Computation](https://www.w3.org/TR/accname-1.1/#h-mapping_additional_nd_te), for deciding which attribute should be the accessible name (if you have provided multiple) can be [found at W3C](https://www.w3.org/TR/accname-1.1/#terminology).

### Lesson 2 - Analyzing Accessible Names in the Search Form

#### Accessible names for Forms

- If an input has an associated `label` (using `for` on the label and `id` on the input) then the text of the `label` will become the accessible name instead of the placeholder of the `input`. 
	- Note: The placeholder will be read aloud as a description (can be configured as a screen reader setting).




## Section 5 - Programmatic Accessibility Information with ARIA

To be completed.

## Section 6 - Accessibility Object Model (AOM)

To be completed.