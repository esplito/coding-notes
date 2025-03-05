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
    <article>
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
    </article>
  </>
</BodyClassName>
```

