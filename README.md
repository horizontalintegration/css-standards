# Horizontal Digital HTML and CSS Style Guide() {

_A mostly reasonable approach to Markup and CSS_

This document is intended to:

- Improve readability
- Minimize common code smells
- Reduce errors and improve maintainability

This document is not intended to:

- Advocate specific frameworks or libraries
- Give advice on design patterns and project architecture
- Guide the reader in learning CSS

### Other Style Guides

- [JavaScript Standards](https://bitbucket.org/horizontal/javascript-standards)

## Table of Contents
1. [Overview](#overview)
1. [HTML Formatting](#html-formatting)
1. [HTML Best Practices](#html-best-practices)
1. [CSS Formatting](#css-formatting)
1. [CSS Best Practices](#css-best-practices)
1. [SCSS](#scss)
1. [Accessibility](#accessibility)
1. [Browser & Device Support](#browser-device-support)
1. [Files & Folders](#files-folders)

<a name="overview"></a>
## Overview

### Prime Directive

The standards herein are a guide to keep developers at the Nerdery interoperable amongst each other and able to produce the highest quality code given the timeline and budget constraints of a project.

For a successful project, always favor solutions that benefit the project and end users over producing "perfect code".

Always aim to meet the standards knowing you'll have to break the rules at some point. Be thoughtful about when you deviate and check with a senior software engineer or a principal software engineer for guidance.

### Expectations

* Understand and follow the standards outlined in this document.
* Understand and follow the [methodology](/standards/methodology.md/).
* Write code knowing that someone else might be maintaining it.
* Select tools that benefit the project.

### Goals

Strive to make your code:
* semantic
* accessible
* predictable
* reusable
* flexible
* resilient
* understandable
* scalable
* performant
* maintainable
* interoperable

<a name="methodology"></a>
## Methodology

The following key points and ideas make up the methodology of "how" and "why" we do Front-End Development at the Nerdery the way we do. Most of these topics are touched on in the standards in the form of rules to follow, but they will be covered more in depth in an effort to better explain and stress the importance of our methodology.

### Modularity

Approaching front end code from a modular point of view addresses the goal outlined in our standards. Writing in a modular fashion makes our code more flexible to change and resistant to breakage as sites grow larger. Modular code is more scalable and maintainable - requiring less CSS that can do more.

The goal here is not to avoid repeating a property/value pair, but to identify the patterns that can be abstracted for high reuse and to identify and create modules that are fully encapsulated logical entities that are easy to extend, and extremely portable.

A module, or "object," is the combination of some markup and the associated CSS (and sometimes Javascript). It is portable, in that it does not change based upon context. It contains all of the CSS needed for that object to function, without relying on context or utility type classes.

Objects come in all shapes and sizes. A button is an object. A heading is an object. A masthead is an object. Some classes have sub-components (class members) which are the children markup elements (and the associated CSS) that make up the object. If the objects are carefully written, you can put them together in any number of ways without having to write additional CSS.

Objects are the Lego blocks that are assembled into full blown pages and sites. Writing CSS to be modular means removing contextual dependencies and writing defensive selectors that limit the depth of applicability.

### CSS Selectors

One of the quickest ways to improve the modularity and flexibility of code is to think harder about the selectors you choose. The first step is to eliminate IDs from CSS selection. IDs do nothing that classes can't also do, but instantly limit reuse and flexibility as they cannot appear on a page more than once. They are also difficult to work with due to their high specificity. The ideal selector is a single classname like `.masthead`.

The next step is to avoid contextual selection, especially across unrelated objects. By styling an object based on context, we again reduce flexibility by needlessly tying an object's appearance to some unrelated class farther up the DOM tree. Now we've created a situation where those styles can only be used within that container, whereas had we simply written the object without contextual selection, we would be free to move that module anywhere in our site and it would still function as it should.

Contextual selection introduces complexity and unpredictability into the mix. If the next developer is unaware of a myriad of contextual conditions for an object, imagine the frustration as an object breaks or changes simply by moving it somewhere else in the site. By avoiding contextual selection, we keep our selector specificity low and gain predictable, robust objects.

Contextual selectors also add weight to our selectors, making them more difficult to work with.

```css
/* DO */
.utilNav { }

/* DO NOT */
.masthead .utilNav { }
```

Avoid overqualified selectors. Similar to contextual selectors, overqualification limits flexibility and needlessly increases selector weight.

```css
/* DO */
.utilNav { }

/* DO NOT */
div.utilNav { }
```

Avoid using elements in selectors. Elements are risky for a couple of reasons. First, they limit the markup elements we can apply a given class to - making semantic decisions for us in our CSS. Second, they have a high depth of applicability - meaning the likelihood that something will accidentally receive or collide with those styles goes up.

```css
/* DO */
.utilNav-item { }

/* DO NOT */
.utilNav a span { }
```

By simply avoiding IDs, elements and contextual selection, the potential for reuse and modularity increases dramatically.

One false sense of reuse is the use of stacked selectors. In most cases, stacked selectors are an indication that objects aren't properly being identified and abstracted into objects. If selectors are being stacked, a single class probably should have been used.

```css
/* DO */
.utilNav { }

/* DO NOT */
.newsNav,
.eventsNav,
.legalNav { }
```

### Object Identification

One key to writing modular, object based front end code is getting the granularity right. This usually involves thinking about the items on our pages "inside out" instead of "outside in."

Instead of thinking about creating pages, think about the little bits that combine to make that page. The bits are smaller and generally good at doing one thing - being a button, being a list with top borders on my items, being a container that has space for an image, a heading and some copy, being a grid to lay out major components of a site. With each object doing an excellent job, being free of contextual specificity and elemental restraints - it can be placed into the site at will, in a predictable, flexible fashion.

### Object Members

Objects may consist of a single element.

```html
<a href="#" class="btn">Push Me</a>
```

Or objects may consist of base class and any number of class members (sub-components). We prefix the name of sub-components with the base class name, followed by a hyphen to indicate a sub-component.

```html
<div class="object">
  <div class="object-member"> ... </div>
</div>
```

A very common pattern is for objects to have head, body and foot components.

```html
<div class="featurePanel">
  <div class="featurePanel-hd"> ... </div>
  <div class="featurePanel-bd"> ... </div>
  <div class="featurePanel-ft"> ... </div>
</div>
```

Components are frequently optional, and certainly aren't restricted to head, body and foot items.

```html
<div class="featurePanel">
  <div class="featurePanel-bd"> ... </div>
</div>
```

### Object Extension

We can account for variations on an object by creating an extension to that object. We prefix the name of an extension with the base class name, followed by an underscore to indicate an extension.

```html
<div class="object object_extension"> ... </div>
```

Extending objects is an important habit to get into. Always directly alter the object you want to change, instead of styling it contextually from a different object.

```html
<!-- DO -->
<div class="box">
  <a href="#" class="btn btn_secondary">Go Back</a>
</div>
```

```css
/* DO */
.btn { color: #000000; }
.btn_secondary { color: #999999; }
```

```html
<!-- DO NOT -->
<div class="box">
  <a href="#" class="btn">Go Back</a>
</div>
```

```css
/* DO NOT */
.btn { color: #000000; }
.box .btn { color: #999999; }
```

### Object Mixins

Extensions to an object should follow a single ancestry chain. Two sub-classes of an object should not appear on the same object if they are of a different ancestry chain. For these types of scenarios, we have mixins.

Mixins are a type of class extension that do not create a sub-class, but rather can be added to (mixed with) any super or sub-class of object. If a type of object has many variations that cannot be elegantly defined into discrete sub-classes, mixins may be the best solution. Mixins are generally an undesirable way to write CSS and should only be used when that is the best option availabe - usually when a design lacks consistency. Often it is better to create more base classes than to try and shoehorn everything into a single object with a lot of mixins.

We indicate an object mixin by prefixing the name with "mix-" and the base class name.

```html
<div class="object mix-object_extension"> ... </div>
```

```html
<h1 class="hdg hdg_h1"> ... </h1>
<h1 class="hdg hdg_h1 mix-hdg_cap"> ... </h1>
<h1 class="hdg hdg_h2"> ... </h1>
<h1 class="hdg hdg_h2 mix-hdg_cap"> ... </h1>
```

```css
.hdg {
  font-family: arial, helvetica, sans-serif;
  font-weight: bold;
}

.hdg_h1 { font-size: 30px; }
.hdg_h2 { font-size: 20px; }

.mix-hdg_cap { text-transform: uppercase; }
```

### Object States

States are special class extensions that represent the various states an object may be in - often used in conjunction with Javascript. States are indicated with the "is" prefix before the extension name.

```html
<div class="object object_isState"> ... </div>
```

```html
<a href="#" class="navItem navItem_isActive"> ... </a>
```

```css
.navItem {
  color: #ffffff;
  background: #000000;
}

.navItem_isActive {
  color: #000000;
  background: #ffffff;
}
```

### All Together Now

With these naming conventions, intention and structure become clear just from reading a class from right to left. Underscore means extends, hyphen indicates a class member (sub-component).

A selector this deep is rare, but it illustrates the point:

```css
.cta-bd_highlight-status { }
```

Right to left, this tells us we're dealing with: The status element which is a sub-component (-) of the highlight extension (_) of bd which is a sub-component of the cta base object. From this alone we can extrapolate the ancestry of our selector as well as part of the markup structure this object has:

```html
<div class="cta">
  <div class="cta-bd cta-bd_highlight">
    <div class="cta-bd-status cta-bd_hightlight-status"> ... </div>
  </div>
</div>
```

### Depth of Applicability

Carefully written objects limit their depth of applicability. A selector with a high depth of applicability casts too wide a net and can result in unexpected results. By writing defensive selectors from the start, we eliminate that risk. An object's depth of applicability should extend only to the known components of that module. For instance, if we started an object that is used on a product listing page:

```html
<ul class="listing">
  <li> ... </li>
  <li> ... </li>
  <li> ... </li>
  <li> ... </li>
</ul>
```

One might be tempted to style the list like this:

```css
.listing li {
  border-top: 1px solid #000000;
  padding-top:10px;
  margin-top: 10px;
}
```

This selector is not defensive enough - any number of things could go inside of .listing, including other lists, which means we'd then have to override our listing styles and make sure our overrides appear later in the CSS source file. We would improve the depth of applicability by utilizing a child combinator selector:

```css
.listing > li {
  border-top: 1px solid #000000;
  padding-top: 10px;
  margin-top: 10px;
}
```

That accomplishes the goal of limiting the depth at which that style will be applied. By making a small change in our selector, we have ensured that these styles will not collide with any of the other modules it may house. We can even take this a step further by removing the element from the selector completely, which would allow us to use this object's styles with more than one kind of markup element:

```css
.listing > * {
  border-top: 1px solid #000000;
  padding-top: 10px;
  margin-top: 10px;
}
```

Alternatively, we can just name member we wish to style and use its classname as the selector:

```html
<ul class="listing">
  <li class="listing-item"> ... </li>
  <li class="listing-item"> ... </li>
  <li class="listing-item"> ... </li>
  <li class="listing-item"> ... </li>
</ul>
```

```css
.listing-item {
  border-top: 1px solid #000000;
  padding-top: 10px;
  margin-top: 10px;
}
```

### Keeping Objects Separate in the DOM

The classes that make up separate objects should not be combined in the DOM - this leads to unpredictable results and breaks object encapsulation. Keep objects separate, both in CSS and in the DOM.

```html
<!-- DO -->
<div class="grid">
  <div class="grid-col grid-col_1of2">
    <div class="someModule"> ... </div>
  </div>
</div>

<!-- DO NOT -->
<div class="grid">
  <div class="someModule grid-col grid-col_1of2">
    ...
  </div>
</div>
```

### Separation of Concerns

By separating certain concerns amongst our objects, we end up with a more flexible set of objects. The rules of thumb are:

- Separate layout styles from container styles
- Separate content styles from container styles

By having layout be its own class of object, separate from container objects, we dramatically increase the number of layouts we can achieve while increasing the flexibility of where and how we can use our container objects. All while writing less CSS as containers no longer need lots of width and float rules, only to be overridden in other contexts. This works to simplify the code and results in more predictable results.

Similarly, by separating our content styles from our container styles, we increase the flexibility and predictability of our content - as we can put any object into any other object and it won't suddenly change.

Attempting to separate your objects into these three duties results in code that is easier to work with, flexible to change, resistant to breakage, scalable and maintainable.

### Resources

- [http://smacss.com/](http://smacss.com/)
- [http://coding.smashingmagazine.com/2012/04/20/decoupling-html-from-css/](http://coding.smashingmagazine.com/2012/04/20/decoupling-html-from-css/)
- [http://coding.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/](http://coding.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)
- [http://www.vanseodesign.com/css/smacss-introduction/](http://www.vanseodesign.com/css/smacss-introduction/)

<a name="html-formatting"></a>
## HTML Formatting

### Output

All html formatting standards are written with source files in mind. If you're using a build process, templates, or partials, the source files (not the build files) are the ones that need to follow the outlined standards.

### Closing Tags

All elements should be closed via a tag pair or self closing declaration.

```html
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
```

```html
<input type="text" />
```

### Indentation

Use 4 spaces for indenting.

Place block, list and table elements on a new line and indent the new line 4 additional spaces.

```html
<div>
  <ul>
    <li><a href="page.html">Link</a></li>
    <li><a href="page.html">Link</a></li>
  </ul>
</div>
```

When using templating logic to generate markup, indent each new line 4 additional space.

```html
{{#if}}
  <div>
    ...
  </div>
{{/if}}
```

### Trailing Whitespace

Remove all trailing whitespace.

### Capitalization

Use only lowercase.

This applies to element names, attributes, and attribute values (unless strings).

```html
<img src="felix.png" alt="Felix, a cat, wrestles with his yarn playfully" />
```

### Quotes

Use double quotes around attribute values.

```html
<input type="text" />
```
### Boolean Attributes

Boolean attributes should not have a value.

```html
<input type="text" disabled />
```

### Comments & Grouping

Use standard HTML comments to indicate the beginning and end of large sections of code.

```html
<!-- Main Content -->
<div>
  ...
</div>
<!-- // End Main Content -->
```

### TODO Comments

Mark todos and action items with a comment that includes `TODO`. Be sure that `TODO` is always uppercase.

```html
<!-- TODO - Review content styles -->
<div>
  ...
</div>
```

<a name="html-best-practices"></a>
## HTML Best Practices

### W3C Validation

* Write valid code.
* Remaining errors and warnings should be intentional.

**Resources**

* <http://validator.w3.org/>

### Doctypes

Use the HTML5 doctype.

```html
<!doctype html>
```

**Resources**

* <http://www.alistapart.com/articles/doctype/>
* <https://css-tricks.com/snippets/html/the-common-doctypes/>
* <http://en.wikipedia.org/wiki/Document_type_declaration>


### Encoding

Use UTF-8 encoding. This meta tag should always be the very first element inside of the document's `<head>` element.

```html
<meta charset="utf-8" />
```

If you end up working in code that does not use an HTML5 doctype, use the meta tag associated with the proper doctype.

```html
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
```
### HTML5 Usage

Not all projects warrant the use of HTML5 elements, but when they do, use HTML5 elements and attributes that progressively enhance modern browsers.

**Use**

- `<div data-controller="exampleDataController"></div>`
- `<input type="email" placeholder="example@domain.com" />`
- `<input type="search" />`
- `<input type="tel" />`
- `<input type="url" />`
- `<input type="text" required />`
- `<article></article>`
- `<aside></aside>`
- `<figure></figure>`
- `<footer></footer>`
- `<header></header>`
- `<mark></mark>`
- `<nav></nav>`
- `<section></section>`
- `<time></time>`
- `<small>&copy; 2015 The Nerdery</small>`

**Use with Caution**

- `<audio></audio>`
- `<canvas></canvas>`
- `<video></video>`
- `<figcaption></figcaption>`
- `<keygen></keygen>`
- `<main></main>`
- `<meter></meter>`
- `<progress></progress>`
- `<output></output>`

**Do Not Use**

- `<datalist></datalist>`
- `<dialog></dialog>`
- `<details></details>`
- `<summary></summary>`
- `<menu></menu>`
- `<menuitem></menuitem>`
- `<hgroup></hgroup>`
- `<wbr>`
- `<bdi>`

**Special Consideration**

Entirely new elements like `<video>` should use appropriate markup based fallbacks so the content is still accessible even if the browser can't render it.

```html
<video width="640" height="360" controls>
    <source src="cats.mp4" type="video/mp4" />
    <source src="cats.ogv" type="video/ogg" />
    <object width="640" height="360" type="application/x-shockwave-flash" data="cats.swf">
        <param name="movie" value="cats.swf" />
        <param name="flashvars" value="controlbar=over&amp;image=cats.jpg&amp;file=cats.mp4" />
        <img src="cats.jpg" width="640" height="360" alt="Download the video below" />
        <a href="cats.mp4">Download cats mp4 file</a>
        <a href="cats.OGV">Download cats ogv file</a>
    </object>
</video>
```

**Resources**

* <https://status.modern.ie/>
* <http://caniuse.com/>


### HTML5 Form Validation

By default, many HTML5 form inputs include validation that is provided by the browser. Error messages are on by default, though in many cases the browser implemented messages are not what a designer wants. In order to remove validation (and thereby the messages) use the `novalidate` attribute on the containing form.

```html
<form action="#" method="post" novalidate>
    <label for="email">Email address:</label>
    <input type="email" placeholder="example@domain.com" id="email" />
    <button type="submit" id="submit">Submit</button>
</form>
```

**Resources**

* <http://diveintohtml5.info/forms.html#validation>


### Optional Tags

Include tags deemed optional in HTML5 (`<html>`, `<head>`, `<body>`)

```html
<!doctype html>
<html>
  <head>
    <title> ... </title>
  </head>
  <body>
    <p> ... </p>
  </body>
</html>
```

### Type Attributes

**HTML5 Doctype**

When working with the HTML5 doctype, omit `type` attributes for stylesheets and scripts.

```html
<link rel="stylesheet" href="screen.css" />
<script src="global.js" />
```

**Legacy Doctypes**

When working with legacy doctypes older than HTML5, include `type` attributes for stylesheets and scripts.

```html
<link rel="stylesheet" href="screen.css" type="text/css" />
<script src="global.js" type="text/javascript" />
```

### Protocol

Whenever possible, omit the protocol portion (`http:`, `https:`) from URLs.

```html
<link rel="stylesheet" href="//www.nerdery.com/assets/styles/screen.css" />
<a href="//www.nerdery.com/">The Nerdery</a>
```

### HTML Entities

Use the appropriate HTML entity names or numbers for special and reserved characters.

```html
&copy; 2012-2015 Crispin &amp; Mulberry &#8482;
```
### Semantic Markup

Write clean semantic markup that clearly reinforces the meaning of the page content.

```html
<nav>
    <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#">About Us</a></li>
        <li><a href="#">Resources</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</nav>
```

```html
<div>
    <small>&copy; 2012 The Nerdery</small>
</div>
```

When generic containers are needed, use `<div>` for block level elements and `<span>` for inline elements.

**Resources**

* <http://en.wikipedia.org/wiki/Semantic_Web>
* <http://vimeo.com/38594726>
* <http://microformats.org/wiki/posh>

### Document Outline

Craft a proper document outline by using appropriate heading levels in a linear fashion.

When necessary, use headings that are hidden in an accessible way to enhance the document outline.

Note that the outline algorithm has changed in HTML5 with the use of new sectioning elements (nav, article, section and aside), allowing the creation of a document outline without using headings in a linear, hierarchical fashion. **Do not do this**. Craft an outline that is complete and accurate from a pre-HTML5 point of view.

#### HTML4

```html
<h1>Horizontal</h1>
<h2 class="isVisuallyHidden">Divisions</h2>
<h3>Horizontal Digital</h3>
<h3>Horizontal Talent</h3>
<h2 class="isVisuallyHidden">Departments</h2>
<h3>New Business</h3>
<h4>Human Resources</h4>
<h4>Finance</h4>
<h4>Information Technology</h4>
```

**Resources**

* <http://html5doctor.com/outlines/>
* <http://www.456bereastreet.com/archive/201205/make_sure_your_html5_document_outline_is_backwards_compatible/>
* <http://html5forwebdesigners.com/semantics/index.html#sectioning_content>
* <http://webaim.org/projects/screenreadersurvey/#headings>
* <http://www.paciellogroup.com/blog/2013/10/html5-document-outline/>
* <http://www.w3.org/TR/html5/sections.html#headings-and-sections>

### Microdata, Microformats and RDFa

Microdata, microformats and RDFa are all approaches to marking up certain types of data in a machine readable way. Search engines and devices/browsers can leverage this increased semantics in various useful ways. While most search engines currently support all three, Google and the WHATWG are putting their weight behind microdata and this is our preferred method of implementing rich data.

#### When to use

Here are some examples of places where we can use microdata:

- Creative Work
- Event
- Person
- Place
- Organization
- Product
- Review
- Rating

Get to know the various types of data that will benefit from this type of markup and leverage this increased semantics whenever possible.

```html
<div itemscope itemtype="http://schema.org/Product">
    <h2 itemprop="name">Strawberry Champagne Vinaigrette</h2>
    <a href="/shop-our-products/strawberry-champagne-vinaigrette" itemprop="url">
        <img src="/media/images/products/strawberry-champagne-vinaigrette.jpg" alt="" itemprop="image" />
    </a>
    <div>
        Item: <span itemprop="sku">753604</span>
    </div>
    <div itemprop="description">
        Drizzle this sweet, gorgeous ruby dressing over salads or grilled meats. Limited time.
    </div>
    <div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
        <div>
            <span itemprop="ratingValue">3</span> stars
        </div>
        <div>
            <span itemprop="reviewCount">4</span> reviews
        </div>
    </div>
    <div itemprop="offers" itemscope itemtype="http://schema.org/Offer">
        <span itemprop="price">$8.99</span>
    </div>
</div>
```

**Resources**

* <http://schema.org/docs/gs.html>
* <https://developers.google.com/structured-data/rich-snippets/?hl=en&rd=1>
* <http://schema.org/docs/faq.html?hl=en&rd=1>
* <https://developers.google.com/structured-data/?hl=en&rd=1>
* <http://microformats.org/get-started>
* <http://html5doctor.com/microformats/>

### Image Formats

Know image formats well and choose the best format for each scenario. The goal is the best image quality at the smallest file size.

**Resources**

* <http://en.wikipedia.org/wiki/JPEG>
* <http://en.wikipedia.org/wiki/Portable_Network_Graphics>
* <http://en.wikipedia.org/wiki/GIF>

### Image Usage

Consider options carefully when choosing whether or not images will be included as inline images or background images. If an image is considered to be content or if a content editor would want to change the image, it should be inline in the markup as an `<img>` element. If an image is used to enhance the visual aesthetics of the interface and supplies no extra meaning to the markup it should be a CSS background image.

### Table Usage

Tables should be reserved for tabular data and never used to achieve page layout.

To make tables semantic and accessible, always:

* Use `<thead>`, `<tbody>` and `<tfoot>` to differentiate between the table header, table body, and table footer
* Use `<tr>` to separate table rows
* Use `<th>` for table headers and `<td>` for table data
* Include a proper scope on the `<th>`


```html
<table>
    <thead>
        <tr>
            <th scope="col">Year</th>
            <th scope="col">Make</th>
            <th scope="col">Model</th>
            <th scope="col">Color</th>
            <th scope="col">Horsepower</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2000</td>
            <td>Toyota</td>
            <td>Matrix</td>
            <td>Silver</td>
            <td>140hp</td>
        </tr>
        <tr>
            <td>2007</td>
            <td>BMW</td>
            <td>328xi</td>
            <td>Jet Black</td>
            <td>255hp</td>
        </tr>
    </tbody>
</table>
```

<a name="css-formatting"></a>
## CSS Formatting

### Capitalization

Use camelCase for selector names and lowercase for properties & property values (unless strings).

```css
.mySelector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Name Delimiters

For more information on classes, class members, class extensions and class mixins, read the [methodology](/standards/methodology.md/).

**Class Names**

Use camelCase for class names.

```css
.featurePromo {
  padding: 10px;
  border: 1px solid #bbbbbb;
  background: #ffffff;
}
```

**Sub Components**

Use a hyphen before a class member (sub-component) name, prefixed with the base class name. The class member name is camelCased.

```css
.featurePromo-hd {
  margin-bottom: 10px;
}

.featurePromo-bd {
  position: relative;
  padding: 5px;
}

.featurePromo-ft {
  border-top: 1px solid #000000;
}

.featurePromo-statusMessage {
  padding: 10px;
  border: 1px dotted #ff0000;
}
```

**Extensions**

Use an underscore before a class extension name, prefixed with the base class or sub-component class it extends. The extension name is camelCased.

```css
.featurePromo_primary {
  border-color: #ff0000;
  background: #aa6600;
}

.featurePromo_lastChild {
  border: none;
}

.featurePromo-bd_inset {
  padding: 20px;
}
```

**Mixins**

Use `mix-` followed by the base class, then the rest of the class extension name. The extension name is camelCased.

```css
.hdg {
  font-family: arial, helvetica, sans-serif;
  font-size: 32px;
  font-weight: bold;
  text-transform: uppercase;
  color: #000000;
}

.hdg_1 {
  font-size: 30px;
}

.hdg_2 {
  font-size: 20px;
}

.mix-hdg_brandColor {
  color: #cccccc;
}
```

**States**

Use an underscore between a state class and the class it extends. Prefix the state class name with `is`. The state name is camelCased

```css
.menuItem_isActive {
  font-weight: bold;
  text-decoration: underline;
}
```

**Themes**

Name class theme extensions prefixed with `theme-` followed by the base class, then the rest of the class extension name. The extension name is camelCased

```css
.masthead {
  background: #333333;
}

.theme-masthead_cats {
  background: transparent url("../images/cat-bg.jpg") no-repeat 0 0;
}

.theme-masthead_wolfPack {
  background: transparent url("../images/3-wolves-bg.jpg") no-repeat 0 0;
}
```

### Brackets

Place the opening curly-bracket of each rule block on the same line as the last selector.

Place the closing curly-bracket of each rule block on its own line after the final property of the rule block.

```css
.selector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Indentation

Use 4 spaces for indenting.

Indent each property in a rule block 4 spaces.

```css
.selector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Property Whitespace

Put each property on its own line.

Follow each property with a colon and a single space.

```css
.selector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Semi-colons

Follow each property value with a semi-colon.

```css
.selector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Trailing Whitespace

Remove all trailing whitespace.

### Rule Block Separation

Separate each rule block by an empty line.

```css
.selector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}

.selector {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Property Order

CSS properties should be grouped by commonality (display, then box-model, then positioning, then background/color, then typography, then misc/etc).

```css
.selector {
  display: block;
  width: 400px;
  height: 222px;
  padding: 4px;
  margin: 2px;
  position: absolute;
  top: 22px;
  left: -12px;
  color: #ffffff;
  font: 12/1.2 normal Helvetica, Arial, san-serif;
  text-transform: uppercase;
  text-indent: 0;
}
```

### Vendor Prefixes

Include vendor prefixes for all properties that have had a vendor prefix in the past.

It is not necessary to include vendor prefixes for specific rendering engines if it never existed.

Include the W3C standard property after the vender prefixed properties.

```css
.selector {
  -webkit-border-radius: 6px;
  -moz-border-radius: 6px;
  border-radius: 6px;
}
```

```css
.selector {
  -webkit-transform: rotate(40deg);
  -moz-transform: rotate(80deg);
  -ms-transform: rotate(40deg);
  -o-transform: rotate(40deg);
  transform: rotate(40deg);
}
```

**Resources**

* <http://caniuse.com/>

### Multi-Value Properties

Format multi-value properties by starting a new line for each value and indenting each line 4 spaces.

```css
.selector {
  background: transparent url("space.png") repeat-x 0 0;
  background:
    transparent url("stars-1.png") repeat-x -130% 0,
    transparent url("stars-2.png") repeat-x 40% 0;
}
```

### Single Properties

It is acceptable to put rule blocks with a single property on a single line.

Include a space after the opening bracket and before the closing bracket.

```css
.selector { background-position: 0 -100px; }
```

No line break is required between rule blocks when the selectors are part of the same object and the properties match.

```css
.icn_twitter { background-position: 0 -100px; }
.icn_facebook { background-position: 0 -200px; }
.icn_youtube { background-position: 0 -300px; }
```

### Quotes

Use double quotes.

**File Paths and Data URIs**

Use double quotes for file paths and data URIs.

```css
.selector {
  background-image: url("background.png");
}
```

**Font Family**

Use double quotes for font names unless they are system fonts that don't require them.

```css
.selector {
  font-family: "LeagueGothic", Helvetica, Arial, sans-serif;
}
```

**Generated Content**

Use double quotes around generated content values.

```css
.selector:before {
  content: "Chapter:";
  display: inline;
}
```

**Attribute Selectors**

Use double quotes around values in attribute selectors.

```css
input[type="text"] {
  color: #ff0000;
}
```

### Multiple Selectors

Separate multiple selectors in the same rule block with a comma and place each selector on a new line.

```css
.navItem:focus,
.navItem:active {
  color: #ff0000;
  font-family: Times, "Times New Roman", serif;
}
```

### Comments & Grouping

Group related rule blocks by base object using the standardized section comment style.

```css
/* ---------------------------------------------------------------------
Box
------------------------------------------------------------------------ */

.box {
  padding: 20px;
  background-color: #cccccc;
}
```

If a section requires additional subsets of comments, a single line comment is acceptable.

If a property / value pair needs additional clarity or is not self-documenting, add comments on the same line immediately following the value.

```css
/* ---------------------------------------------------------------------
Horizontal List
------------------------------------------------------------------------ */
.hList {
  overflow: hidden;
  *zoom: 1; /* ie6-7 float clearing */
}

.hList > * { float: left; }

/* horizontal spacing extensions */
.hList_tight > * + * { margin-left: 8px; }
.hList_std > * + * { margin-left: 16px; }
.hList_loose > * + * { margin-left: 24px; }

/* adds vertical pipes between list elements */
.hList_divided > * + * {
  margin-left: 12px;
  padding-left: 12px;
  border-left: 1px solid;
}
```

```css
.carousel {
  position: relative; /* A positioning reference for .carousel-nav */
}

.carousel-nav {
  position: absolute; /* Positioned against .carousel base class */
  right: 20px;
  bottom: 20px;
  z-index: 10; /* Pull navigation on top of .carousel-slides */
}
```

### Hexadecimal Notation

Always use six character and lowercase hexadecimal notation.

```css
.selector {
  background-color: #ff0000;
}
```

### TODO Comments

Mark todos and action items with a comment that includes `TODO`. Be sure that `TODO` is always uppercase.

```css
/* TODO - Review content styles */
.selector {
  color: #ff0000;
}
```

### Table of Contents

Do not include a table of contents in the CSS.

```css
/* DO NOT */
/* ---------------------------------------------------------------------
Table of Contents

1 - Masthead
2 - Utility Classes
3 - Grid
4 - Media Object
5 - Icons

------------------------------------------------------------------------ */
```



<a name="css-best-practices"></a>
## CSS Best Practices

### Encoding

Do not declare a `@charset` in the CSS.

```css
/* DO NOT */
@charset "UTF-8";

.selector {
  color: #ff0000;
}
```

### Browser Reset

```css
/* ------------------------------------------------
RESET CSS (thanks Eric Meyer)
------------------------------------------------ */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font-weight: inherit;
  font-style: inherit;
  font-family: inherit;
  vertical-align: baseline;
}

body {
  line-height: 1;
}

ol, ul {
  list-style: none;
}

blockquote, q {
  quotes: none;
}

blockquote:before, blockquote:after,
q:before, q:after {
  content: '';
  content: none;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
}

:focus {
  outline-width: 0;
}

html {
  overflow-y: scroll; /* Always show a vertical scrollbar, even when there is no scrolling */
}

html {
  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
}

/* ------------------------------------------------
 HTML5 Element Reset
------------------------------------------------ */
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, main {
  display: block;
}

audio, canvas, video, progress, picture {
  display: inline-block;
}

template {
  display: none;
}

/* ------------------------------------------------
 Form Reset Styles
------------------------------------------------ */
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration,
input[type="search"]::-webkit-search-results-button,
input[type="search"]::-webkit-search-results-decoration {
  -webkit-appearance: none;
}

input[type="search"] {
  -webkit-appearance: none;
  -webkit-box-sizing: content-box;
  -moz-box-sizing: content-box;
  box-sizing: content-box;
}

textarea {
  overflow: auto;
  vertical-align: top;
  resize: vertical;
}

::-moz-focus-inner {
  border: 0;
  padding: 0;
}
```

**Do not** use normalize.css or a universal selector reset.

### Classes & IDs

**Do not** use `id` for styling purposes

Acceptable uses for `id` include:

* JavaScript selection - prefix these with `js-` to indicate they are for JavaScript only
* Assigning form labels to inputs
* Jump links between sections of a page

```html
<div class="carousel" id="js-carousel"> ... </div>
```

```html
<label for="firstName">First Name</label>
<input type="text" id="firstName" class="input input_txt" />
```

```html
<a href="#mainContent">Skip to Main Content</a>
...
<div class="content" id="mainContent"> ... </div>
```

**Resources**

* <http://www.stubbornella.org/content/2011/04/28/our-best-practices-are-killing-us/>
* <http://screwlewse.com/2010/07/dont-use-id-selectors-in-css/>
* <http://oli.jp/2011/ids/>

### Contextual Selectors

Avoid cross module contextual selection.

```css
/* DO NOT */
.box .hdg { }
```

If a single module can make use of a contextual selector (as the markup structure is known), be sure to apply it with the proper depth of applicability.

```css
.list > li { }
```

### Selector Specificity

Keep selector specificity as low as possible, opting for a single class as the best selector.

```css
.box {
  padding: 20px;
  background-color: #cccccc;
}
```

**Resources**

* <http://www.w3.org/TR/css3-selectors/#specificity>
* <http://www.stuffandnonsense.co.uk/archives/css_specificity_wars.html>
* <http://css-tricks.com/specifics-on-css-specificity/>
* <http://www.standardista.com/css3/css-specificity/>

### Separation of Concerns

Using the [outlined methodology](#methodology), separate layout styles from container styles and content styles from container styles.

**Resources**

* <https://smacss.com/>
* <http://www.smashingmagazine.com/2012/04/20/decoupling-html-from-css/>
* <http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/>
* <http://www.vanseodesign.com/css/smacss-introduction/>

### Naming Conventions

**Name Quality**

Class names should be specific enough so that you can comprehend the name without context, but generic enough for reuse throughout the code base.

```css
.wrapper { }
.masthead { }
.globalFooter { }
.cta { }
.btn { }
.box { }
.icn { }
```

**Numerical/Enumerated Naming**

Do not use numbers, letters, the greek alphabet, or any other sequential naming convention.

```css
/* DO NOT */
.box1 { }
.box2 { }
.box3 { }
```

There are a few notable exceptions:

* Heading tags: `.hdg_1`, `.hdg_2`, etc. are appropriate as they indicate hierarchy.
* Vertical rhythm classes: `.vr_2x`, `.vr_3x`, etc. are appropriate as they indicate a ratio.

### Class Chaining

Avoid chaining classes.

```css
/* DO */
.btn_primary { }

/* DO NOT */
.btn.primary { }
```

An exception to this rule is chaining state classes, as they are often applied via JavaScript and chaining classes in this way provides an appropriate level of reuse.

```css
.btn.isActive { }
```

### Body / Page Classes

Avoid using body / page classes. This includes classes on the `<html>` or `<body>` element, or page-based classes on wrapper `<div>`s.

The exception to this rule is when leveraging Modernizr or other feature detection to add classes to the `<html>` element, allowing for feature based fallbacks in CSS.

```html
<html class="no-js">
...
</html>
```

```css
.carouselItem {
  float: left;
  margin: 0 10px;
}

/* Stack carousel items if no JS */
.no-js .carouselItem {
  float: none;
  margin: 0 0 20px;
}
```

### Shorthand

Use of shorthand is encouraged unless setting a single value.

When using shorthand, be explicit and define all values.

```css
.selector {
  margin: 12px 0 16px 0;
}
```

```css
.selector {
  font: italic small-caps normal 13px/1.6 Arial, Helvetica, sans-serif;
}
```

### Browser Hacks

Browser hacks are an acceptable alternate means of including browser specific code.

Browser specific code should not be needed in most cases - but should be used to handle known browsers bugs such as emulating inline-block in IE7.

If a hack is included, a comment is required to explain the reasoning.

```css
.selector {
  display: inline-block;
  *display: inline; /* ie6-7 emulate inline-block */
  *zoom: 1; /* ie6-7 emulate inline-block */
}
```

If you need to include a large amount of code targeted at a specific browser, instead utilize conditional comments that allow for inclusion of browser specific stylesheets. Keep in mind that this technique hinders your ability to utilize the cascade.

```html
<!--[if IE 9]><link href="ie9.css" rel="stylesheet" /><![endif]-->
<!--[if lte IE 8]><link href="ie8.css" rel="stylesheet" /><![endif]-->
<!--[if lte IE 7]><link href="ie7.css" rel="stylesheet" /><![endif]-->
<!--[if lte IE 6]><link href="ie6.css" rel="stylesheet" /><![endif]-->
```

**Resources**

* <http://browserhacks.com/>

### Multiple Stylesheets

All CSS should be placed in the same stylesheet with the following exceptions:

* Browser specific stylesheets
* Media specific stylesheets (print)
* Breakpoint specific stylesheets (width media queries)
* Area specific stylesheets (standard site, admin area)

```html
<link href="screen.css" rel="stylesheet" media="screen" />
<link href="print.css" rel="stylesheet" media="print" />
```

Do not use `@import` to include stylesheets. All stylesheets should be linked in the head of your document.

```css
/* DO NOT */
@import url("layout.css");
@import url("typography.css");
```

IE conditional stylesheets should use `lte` declarations, with the exception of IE9.

```html
<!-- DO -->
<!--[if IE 9]><link href="ie9.css" rel="stylesheet" /><![endif]-->
<!--[if lte IE 8]><link href="ie8.css" rel="stylesheet" /><![endif]-->
<!--[if lte IE 7]><link href="ie7.css" rel="stylesheet" /><![endif]-->
<!--[if lte IE 6]><link href="ie6.css" rel="stylesheet" /><![endif]-->

<!-- DO NOT -->
<!--[if lte IE 9]><link href="ie9.css" rel="stylesheet" /><![endif]-->
<!--[if IE 8]><link href="ie8.css" rel="stylesheet" /><![endif]-->
<!--[if IE 7]><link href="ie7.css" rel="stylesheet" /><![endif]-->
```

### Units

The recommended unit for font-sizes is pixels.

```css
.selector {
  font-size: 12px;
}
```

The recommended units for setting box model related properties are pixels and percentages.

```css
.box {
  width: 25%;
  margin-bottom: 10px;
}
```

When building RWD websites and web applications, alternative units of measurement may be more appropriate, but should be chosen with care to do what's best for the project while striving to maintain our [goals](#goals).

### Line-Height

Declare line-height values as unit-less. Unit-less line heights act as a multiplier to the text size for a given element. Always leave a comment explaining the math.

```css
.selector {
  font-size: 12px
  line-height: 1.25; /* 15px / 12px = 1.25 */
}
```

**Resources**

* <http://meyerweb.com/eric/thoughts/2006/02/08/unitless-line-heights/>
* <https://developer.mozilla.org/en-US/docs/Web/CSS/line-height>

### Color Values

Use hexadecimal notation for color values. RGBA is acceptable if opacity is needed, but be sure to provide a fallback for browsers that don't support RGBA.

```css
.selector {
  color: #ff0000;
}

.selector {
  color: #ff0000; /* fallback for non-rgba browsers */
  color: rgba(255, 0, 0, 0.8);
}
```

### Clearing Floats

All floats should be cleared using the micro clearfix syntax.

```html
<ul class="hList">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```

```css
.hList:before,
.hList:after {
  content: " ";
  display: table;
}

.hList:after {
  clear: both;
}

.hList > li {
  float: left;
}
```

**Resources**

* <http://nicolasgallagher.com/micro-clearfix-hack/>
* <https://css-tricks.com/all-about-floats/>

### Sprites

Use sprites whenever possible to improve performance by reducing http requests. Use multiple sprites that are categorized appropriately (nav-sprite, icon-sprite, etc.) as opposed to using a single super sprite for the entire site.

![JPG Example](/standards/_images/sprite-icon.png)

**Resources**

* <http://alistapart.com/article/sprites>
* <https://css-tricks.com/css-sprites/>

### Image Replacement

When using background images that include text, it's important that there is a text equivalent in the markup itself so that the text is accessible. Use a suitable image replacement technique to keep the text hidden from view.

```html
<a href="/home/" class="nav-home">Home</a>
```

```css
.nav-home {
  background-image: url("nav-home.png"); /* image for display of nav home button */
  background-repeat: no-repeat;
  background-color: transparent;
  border: 0;
  overflow: hidden;
}

.nav-home:before {
  content: " ";
  display: block;
  width: 0;
  height: 100%;
}
```

**Resources**

* <https://css-tricks.com/css-image-replacement/>
* <https://css-tricks.com/examples/ImageReplacement/>

### Font Embedding

Use `@font-face` to embed custom fonts.

Font services like TypeKit or the Google Font API may be used. If embedding from a third party site is required, opt to use a CSS method over one that requires JavaScript.

**Do not use FAUST, SIFR or Cufon.**

```css
@font-face {
  font-family: 'GiantHeadOTRegular';
  src: url('../fonts/giant-head/giant-head-regular-ot-webfont.eot');
  src: url('../fonts/giant-head/giant-head-regular-ot-webfont.eot?#iefix') format('embedded-opentype'),
       url('../fonts/giant-head/giant-head-regular-ot-webfont.woff') format('woff'),
       url('../fonts/giant-head/giant-head-regular-ot-webfont.ttf') format('truetype'),
       url('../fonts/giant-head/giant-head-regular-ot-webfont.svg#GiantHeadOTRegular') format('svg');
  font-weight: normal;
  font-style: normal;
}
```

**Resources**

* <http://www.fontsquirrel.com/>
* <https://typekit.com/>
* <http://www.google.com/fonts/>
* <http://www.fontspring.com/blog/further-hardening-of-the-bulletproof-syntax>

### Font Stacks

Choose font stacks with care, being mindful to ensure all bases are covered. Good font stacks generally have 2-4 font fallbacks declared and set the browser default serif or sans-serif last in the stack.

```css
.selector {
  font-family: "LeagueGothic", Helvetica, Arial, sans-serif;
}
```

**Resources**

* <http://nicewebtype.com/notes/2009/04/23/css-font-stacks/>
* <http://www.cssfontstack.com/>

### Positioning

Do not use absolute positioning to layout the main sections of a page.

Do not use relative positioning to "shim" elements around the page to achieve pixel perfection.

Do use positioning contexts when necessary to achieve a flexible and robust object. When using positioning in CSS, leave a comment describing the context.

```html
<div class="carousel">
  <ul class="carousel-slides">
    ...
  </ul>
  <ul class="carousel-nav">
    ...
  </ul>
</div>
```

```css
.carousel {
  position: relative; /* A positioning reference for .carousel-nav */
}

.carousel-nav {
  position: absolute; /* Positioned against .carousel base class */
  right: 20px;
  bottom: 20px;
}
```

### Document Flow

Take advantage of the natural document flow when spacing content by using *only* margin bottom on content elements so they can easily be interchanged while keeping a predictable document flow and avoiding margin collapsing issues.

Additionally use padding on containers to keep a consistent "edge".

```html
<div class="box">
  <div class="userContent">
    <h2>All About Cats</h2>
    <p> ... </p>
    <p> ... </p>
    <h3>Nocturnal Habits</h3>
    <p> ... </p>
    <p> ... </p>
  </div>
</div>
```

```css
.box {
  padding: 20px;
  background: #ffffff;
}

.userContent h2 {
  font-size: 24px;
  color: #333333;
  margin-bottom: 12px;
}

.userContent h3 {
  font-size: 20px;
  color: #333333;
  margin-bottom: 12px;
}

.userContent p {
  font-size: 12px;
  margin-bottom: 12px;
}
```

When dealing with inter-modular spacing, consider using a vertical rhythm object to provide predictable document flow.

### W3C Validation

Write valid code. Remaining errors & warnings should be intentional.

**Resources**

* <http://jigsaw.w3.org/css-validator/>
* <http://csslint.net/>

<a name="scss"></a>
## SCSS

### Folder Structure

The file structure when using SCSS includes 4 folders: base, helpers, landmarks, and modules.

The base folder contains:

* Elements - styles for base element like `<body>` or `<a>`.
* Fonts - `@font-face` rules for embedding fonts for global use
* Reset - common reset styles

The helpers folder contains:

* Utilities - styles for common patterns like `.isVisuallyHidden` and common mixins like `clearfix`
* Variables - all global variables used throughout the site
* Vendor - mixins for vendor prefixes

The landmarks and modules folders contain:

* A folder for each object named after the object
* A file within each folder named after the object that contains relevant styles for the object

Additionally there is a `screen.scss` file that is used to import all SCSS partials for inclusion in a single compiled `screen.css` file.

```diff
└── assets
    └── styles
        ├── base
        |   ├── _elements.scss
        |   ├── _fonts.scss
        |   └── _reset.scss
        ├── helpers
        |   ├── _utils.scss
        |   ├── _vars.scss
        |   └── _vendor.scss
        ├── landmarks
        |   ├── masthead
        |   |   └── _masthead.scss
        |   ├── navPrimary
        |   |   └── _navPrimary.scss
        |   └── footer
        |       └── _footer.scss
        ├── modules
        |   ├── blocks
        |   |   └── _blocks.scss
        |   ├── grid
        |   |   └── _grid.scss
        |   ├── hlist
        |   |   └── _hlist.scss
        |   └── media
        |       └── _media.scss
        └── screen.scss
```

When media queries are introduced into the code the folder structure becomes more complex in order to offer the modularity required to support legacy browsers.

The file structure remains the same, however the following rules apply.

* Each object in the landmarks or modules folder should use the name of the object and the name of the corresponding breakpoint in it's file name separated by a single underscore.
* If a module level variable is needed for multiple breakpoints, add a `_helpers.scss` partial. Use the aforementioned naming convention - the object name followed by `_helpers.scss`

Additionally, instead of compiling everything into a single `screen.css` file, code will be compiled into `modern.css` and `legacy.css` files using the following steps:

* Create a `_screen_breakpoint.scss` file for each breakpoint
* Import partials for each module into the corresponding breakpoint files making sure not to include media queries
* Import all breakpoint partials into `legacy.css`
* Import all breakpoint partials into `modern.css` with each file contained inside the corresponding media queries

Taking these steps provides two production ready CSS files - one of which requires media queries and one that doesn't that can be used to apply a fixed width design for legacy browsers.

```diff
└── assets
    └── styles
        ├── base
        |   ├── _elements.scss
        |   ├── _fonts.scss
        |   └── _reset.scss
        ├── helpers
        |   ├── _utils.scss
        |   ├── _vars.scss
        |   └── _vendor.scss
        ├── landmarks
        |   ├── masthead
        |   |   ├── _masthead_large.scss
        |   |   ├── _masthead_medium.scss
        |   |   └── _masthead_screen.scss
        |   ├── navPrimary
        |   |   ├── _navPrimary_large.scss
        |   |   ├── _navPrimary_medium.scss
        |   |   └── _navPrimary_screen.scss
        |   └── footer
        |       ├── _footer_large.scss
        |       ├── _footer_medium.scss
        |       └── _footer_screen.scss
        ├── modules
        |   ├── blocks
        |   |   ├── _blocks_helpers.scss
        |   |   ├── _blocks_medium.scss
        |   |   └── _blocks_screen.scss
        |   ├── grid
        |   |   └── _grid_medium.scss
        |   ├── hlist
        |   |   ├── _hlist_helpers.scss
        |   |   ├── _hlist_large.scss
        |   |   ├── _hlist_medium.scss
        |   |   ├── _hlist_screen.scss
        |   |   └── _hlist_small.scss
        |   └── media
        |       └── _media_screen.scss
        ├── _screen.scss
        ├── _screen_large.scss
        ├── _screen_medium.scss
        ├── _screen_small.scss
        ├── legacy.scss
        └── modern.scss
```

### Partials

Sass files that are not intended to be compiled independently, should be named with a leading underscore.

```diff
_media.scss
```

### Silent Comments

Silent comments should be used when the comment is irrelevant to the output.

```scss
//----------------------------------------------------------------------
// Micro Clearfix Mixin
//----------------------------------------------------------------------
@mixin clearfix() {
  &:before,
  &:after {
    content: " ";
    display: table;
  }

  &:after {
    clear: both;
  }
}
```

### Variables

**Global Variables**

Variables defined outside of ruleset are considered to be global and are used for setting values used across multiple modules, such as brand colors.

Define all global variables in the global `helpers/_vars.scss` partial.

Write global variables using uppercase letters delimited by underscores.

```scss
$BRAND_COLOR_RED: #ff0000;

.nav a {
  color: $BRAND_COLOR_RED;
}
```

**Module Variables**

Module variables are defined outside a ruleset and are intended for use only with a single module.

If the variable is used only in a single file, define it at the top of the file above all rulesets.

If the variable is used by more than one file in the module, define it in a module level `_module_helpers.scss` partial.

To prevent variable collisions, module variables should be prefixed with the base object name separated by a single dash.

```scss
$example-SPACING_UNIT: 10px;

.example-hd {
  margin-bottom: $example-SPACING_UNIT;
}

.example-bd {
  margin-bottom: $example-SPACING_UNIT;
}
```

**Private Variables**

Private variables are defined inside a single ruleset and are intended for use only within that ruleset.

Write private variables using uppercase letters delimited by underscores, and preceded by a single underscore.

```scss
.exampleObject {
  $_SIZE: 200px;
  $_OFFSET: -($_SIZE / 2); // Pull the object half its size to center it.
  position: absolute;
  top: 50%;
  left: 50%;
  height: $_SIZE;
  width: $_SIZE;
  margin-top: $_OFFSET;
  margin-left: $_OFFSET;
}
```

### Operators

Operators should always be surrounded by a single space.

Use of operators should always be accompanied by a comment.

```scss
.exampleObject {
  $_SIZE: 100px;
  $_PULL: -($_SIZE / 2); /* Pull the object half its size to center it. */
  position: absolute;
  top: 50%;
  left: 50%;
  height: $_SIZE;
  margin-top: $_PULL;
  margin-left: $_PULL;
}
```

### Nesting

Avoid nesting selectors.

```scss
/* DO */
.hList {
  @include clearfix();
}

.hList > li {
  float: left;
}

/* DO NOT */
.hList {
  @include clearfix();

  > li {
    float: left;
  }
}
```

Also avoid nesting pseudo classes & pseudo elements.

```scss
/* DO */
.nav a {
  color: #ffffff;
}

.nav a:hover {
  color: #00ff00;
}

/* DO NOT */
.nav a {
  color: #ffffff;

  &:hover {
    color: #00ff00;
  }
}
```

There are a few scenarios where nesting is acceptable. These includes things like sandboxing an entire stylesheet to avoid conflicting selectors, wrapping a WYSIWYG stylesheet, and nesting media queries to make a small change that's not at a minor breakpoint.

When nesting is appropriate, all selectors should be indented one level deeper than the parent selector.

```scss
.exampleObject {
  padding: 10px;
  @media (max-width: 500px) {
    padding: 20px;
  }
}
```

### Mixins

Use mixins to define styles that can be reused throughout the site to avoid repeating common code blocks. Mixins can be used at both the global and module level. Mixins used by a single module should be prefixed with the module name, and defined in the appropriate location.

Mixin definitions should always include the parenthesis.

Provide default arguments when their absence could cause a compilation error, otherwise, avoid using default arguments in favor of optional arguments.

```scss
@mixin isVisuallyHidden() {
  width: 1px !important;
  height: 1px !important;
  margin: -1px !important;
  border: 0 !important;
  padding: 0 !important;
  clip-path: inset(100%) !important;
  clip: rect(0 0 0 0) !important;
  overflow: hidden !important;
  position: absolute !important;
  white-space: nowrap !important;
}
```

If a mixin is used to produce vendor prefixes of a single property, it should mimic the W3C naming and syntax.

```scss
@mixin transition($transition) {
  -webkit-transition: $transition;
  -moz-transition: $transition;
  -o-transition: $transition;
  transition: $transition;
}
```

### @extend

Do not use `@extend`.

```scss
/* DO */
.message {
  padding: 10px;
  border: 1px solid #cccccc;
}

.message_success { color: #00ff00; }
.message_error { color: #ff0000; }

/* DO NOT*/
.message {
  padding: 10px;
  border: 1px solid #cccccc;
}

.message_success {
  @extend .message;
  color: #00ff00;
}

.message_error {
  @extend .message;
  color: #ff0000;
}
```

### @control directives

`@if`, `@each`, `@while`, `@for`

Flow control directives should never use the single line syntax.

Curly braces should be formatted identically to selectors.

```scss
.exampleObject {
  @if 1 + 1 == 2 {
    border: 1px solid;
  }
}
```

### Other @directives

**@import**

Do not include the file extension when using `@import`.

```scss
@import "_media";
```

**@media**

It is acceptable to use `@media` rules nested in selectors if they are used for small tweaks. Separate stylesheets (`screen`, `screen_small`, `screen_large`, etc) should be used for major breakpoints.

```scss
.exampleObject {
    padding: 10px;
    @media (max-width: 500px) {
        padding: 20px;
    }
}
```

**@function**

Function naming should follow the same naming conventions as mixins.

**@warn**

It is acceptable to use `@warn` in-lieu of TODO comments.

**@debug**

It is acceptable to use `@debug` provided it is not committed to the version control repository.


<a name="accessibility"></a>
## Accessibility

### Source Order

Use logical source order as assistive devices follow source top to bottom.

```html
<div class="masthead">
  ...
</div>
<div class="navigation">
  ...
</div>
<div class="contentMain">
  ...
</div>
<div class="contentRelated">
  ...
</div>
<div class="footer">
  ...
</div>
```

### JavaScript Content

JavaScript content should be accessible to screen readers. The best option for this is to use a logical source order and keep appended / prepended items near the element that triggers their appearance.

If this isn't an option rely on ARIA techniques instead.

```html
<div class="contentMain">
  <a href="#myModal" class="js-spawn-modal">View Larger Image</a>
  <div class="modal" id="myModal">
    ...
  </div>
</div>
<div class="footer">
  ...
</div>
```

### Page Titles

Include a unique title for each page.

```html
<title>Horizontal Digital: HTML &amp; CSS Style Guide - Accessibility</title>
```

### ARIA Roles

**Landmark Roles**

Use landmark ARIA roles on all projects.

The landmark ARIA roles include:

* **banner** - wraps the typical site masthead. Use only once per page.
* **search** - place on a form that performs search.
* **navigation** - place on your navigation container (for in-document and site-wide navigation).
* **main** - place on container that holds the main page content. Use only once per page.
* **complementary** - place on containers that are akin to the `<aside>` HTML5 element.
* **contentinfo** - place on element that contains meta information about the page or content, often the site footer. Use only once per page.

When using HTML5 elements, like `<header>` and `<footer>`, provide progressive enhancement for screen readers by including ARIA landmark roles on these elements as well, even though some screen readers may see this as redundant.

```html
<div class="masthead" role="banner">
  ...
  <form class="search" action="#" method="post" role="search">
    ...
  </form>
  ...
</div>
<div class="navigation" role="navigation">
  ...
</div>
<div class="contentMain" role="main">
  ...
</div>
<div class="contentRelated" role="complementary">
  ...
</div>
<footer class="footer" role="contentinfo">
  ...
</footer>
```

If the project requirements allow, you may combine the use of landmark ARIA roles with HTML5 elements.

```html
<header class="masthead" role="banner">
  ...
  <form class="search" action="#" method="post" role="search">
      ...
  </form>
  ...
</header>
<nav class="navigation" role="navigation">
  ...
</nav>
<main class="contentMain" role="main">
  ...
</main>
<aside class="contentRelated" role="complementary">
  ...
</aside>
<footer class="footer" role="contentinfo">
  ...
</footer>
```

**Resources**

* <http://www.w3.org/1999/xhtml/vocab#XHTMLRoleVocabulary>
* <http://www.paciellogroup.com/blog/2010/10/using-wai-aria-landmark-roles/>

### Skip Links

In order for keyboard users to easily skip to the main content of a site without tabbing through all the previous content, use a "skip to content" link. It is also acceptable to include a "skip to navigation" link if it takes an excessive number of tabs to get focus on the navigation.

It's best practice to bring focusable into view when they are focused.

```html
<body>
  <a href="#page-content" class="isVisuallyHidden">Skip to Content</a>
  <div class="masthead">
    ...
  </div>
  <div id="page-content" class="contentMain">
    ...
  </div>
</body>
```

```css
.isVisuallyHidden:not(:focus):not(:active) {
  width: 1px !important;
  height: 1px !important;
  margin: -1px !important;
  border: 0 !important;
  padding: 0 !important;
  clip-path: inset(100%) !important;
  clip: rect(0 0 0 0) !important;
  overflow: hidden !important;
  position: absolute !important;
  white-space: nowrap !important;
}
```

### Alt Attributes

Use `alt` attributes on all images.

If the image is in a link, the alt text should specify the action that will be taken upon clicking the image.

Include any text content that appears in an image in the `alt` attribute.

```html
<!-- DO -->
<a href="index.html"><img src="../images/logo.png" alt="Crispen &amp; Mulberry Home" /></a>

<!-- DO NOT -->
<a href="index.html"><img src="../images/logo.png" alt="Logo" /></a>
```

If a content image contains no useful information or text, include the `alt` attribute but leave the value empty.

Do not describe the image. Never use the phrases "image of" or "picture of" in `alt` values.

```html
<!-- DO -->
<img src="../images/business.jpg" alt="" />

<!-- DO NOT -->
<img src="../images/business.jpg" alt="Picture of two gentlemen shaking hands with a blue sky and tree in the background" />
```

### Longdesc Attributes

Do not rely on the longdesc attribute for consistent accessibility.

Instead, opt for including the content as text on the page or provide a link to the text alternative on the page.

```html
<img src="infographic.jpg" alt="A list of all the presidents with facial hair listed chronologically" />
<a href="presidents-with-facial-hair.html">See the list of presidents who had facial hair while in office.</a>
```

### Tables

Include `thead`, `tbody` and `tfoot` to indicate the table header, table body and table footer.

Declare table header cells and scope them properly.

```html
<table>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Goals</th>
      <th scope="col">Assists</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Evgeni Malkin</td>
      <td>50</td>
      <td>59</td>
    </tr>
    <tr>
      <td>Ilya Kovalchuk</td>
      <td>37</td>
      <td>46</td>
    </tr>
  </tbody>
</table>
```

### Form Labels

All form controls, except `<button>`, require a corresponding `<label>`, even if the label is hidden from view.

The `for` attribute is required on all labels.

```html
<label for="name">Name:</label>
<input type="text" id="name" />
```

If possible, it is preferable to wrap inputs with the corresponding label.

```html
<label for="professionDoctor">Doctor <input type="radio" name="profession" id="professionDoctor" /></label>
<label for="professionLawyer">Lawyer <input type="radio" name="profession" id="professionLawyer" /></label>
<label for="professionOther">Other <input type="radio" name="profession" id="professionOther" /></label>
```

### Form Button Values

Form input buttons require a `value` attribute.

```html
<input type="submit" value="Submit" />
```

```html
<input type="reset" value="Reset" />
```

Image type inputs require an `alt` attribute.

```html
<input type="image" src="button.png" alt="Submit" />
```

Button elements require a value between the opening and closing tags.

```html
<button type="submit">Submit</button>
```

### Form Placeholder Text

Use placeholder text to provide a hint to the user.

Whenever possible, do not use `placeholder` text as a replacement for a `<label>`. When necessary, opt for visually hiding the label instead.

```html
<label for="email" class="isVisuallyHidden">Email</label>
<input type="email" id="email" placeholder="user@domain.com" />
```

### Form Grouped Controls

Use a `fieldset` to logically group form controls.

If form labels don't clearly identify what information is being collected, use a `legend` to label the group.

Keep legend text short as it is read by screen readers before each input in the fieldset.

```html
<fieldset>
  <legend>Profession</legend>
  <label for="professionDoctor">Doctor <input type="radio" name="profession" id="professionDoctor" /></label>
  <label for="professionLawyer">Lawyer <input type="radio" name="profession" id="professionLawyer" /></label>
  <label for="professionOther">Other <input type="radio" name="profession" id="professionOther" /></label>
</fieldset>
```

### Form Validation

Form inputs that have validation requirements - like required fields, or an expected email format - should be clearly labeled in an accessible manner indicating the requirement. They should also include the `aria-required` attribute with the value set to `true`.

Form validation and submission should be accessible to both the mouse and keyboard.

All form inputs that are validated client side should also be validated in the same manner on the server side.

If a field returns as invalid, it should include the `aria-invalid` attribute with the value set to the proper value (`true`, `false`, `grammar`, `spelling`).

```html
<form action="#" method="post">
  <ul>
    <li>
      <label for="formUsername">Username <span class="required">*</span></label>
      <input type="input" id="formUsername" required aria-required="true" />
    </li>
    <li>
      <label for="formEmail">Email Address <span class="required">*</span></label>
      <input type="email" id="formEmail" required aria-required="true" />
    </li>
    <li>
      <label for="formPassword">Password <span class="required">*</span></label>
      <input type="password" id="formPassword" required aria-required="true" aria-describedby="password-requirements" />
      <span id="password-requirements">Use 6-20 characters, at least 1 uppercase letter and 1 number.</span>
    </li>
    <li>
      <input type="submit" value="Sign Up" />
    </li>
  </ul>
</form>
```

**Resources**

* <http://alistapart.com/article/aria-and-progressive-enhancement>
* <http://www.deque.com/blog/accessible-client-side-form-validation-html5-wai-aria/>
* <https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-invalid_attribute>
* <http://webaim.org/techniques/formvalidation/>

### Hiding Content

When hiding content on a page, choose the appropriate method.

Using `display: none;` or `visibility: hidden;` will hide content from a screen reader.

If content is meant to be available to a screen reader but not to a sighted user, hide the content by using a utility class.

```html
<h2 class="isVisuallyHidden">Featured Articles</h2>
```

```css
.isVisuallyHidden {
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
  position: absolute;
  clip: rect(0 0 0 0);
  overflow: hidden;
}
```

**Resources**

* <http://snook.ca/archives/html_and_css/hiding-content-for-accessibility>
* <http://webaim.org/techniques/css/invisiblecontent/>

### Focus Styles

Include `:focus` styles to ensure proper keyboard accessibility. Keep in mind, `:focus` is included in `reset.css`, so it will need to be intentionally styled to fit your project requirements.

**Resources**

* <http://24ways.org/2009/dont-lose-your-focus>

### Tabindex

A site should be navigable via keyboard. Write markup that is semantic and supports a logical tab index. Modifying the `tabindex` should be a last resort.

### Magnification

Web pages should remain usable and understandable when the text is magnified via the browser's magnification feature. This includes "pinch zooming" on touch devices. The page may not remain aesthetically pleasing, but it should remain understandable and usable.


<a name="browser-device-support"></a>
## Browser &amp; Device Support

### Support Documentation

Identify which browsers and devices are supported in the `README.md` file.

### Progressive Enhancement

Progressive enhancement methodology should be used to the fullest extent possible on all projects.

**Resources**

* <http://alistapart.com/article/understandingprogressiveenhancement>
* <http://alistapart.com/article/progressiveenhancementwithcss>
* <http://alistapart.com/article/progressiveenhancementwithjavascript>

### Design Accuracy

Surgical precision is expected when recreating our static design assets into dynamic websites. While RWD makes this more challenging, you can still test for visual accuracy at the breakpoints provided in the comps.

Test the accuracy of your work as part of your everyday workflow. You can easily test how accurate the website is in relation to the initial design assets in any environment using the following steps:

* Take a screenshot in the browser.
* Paste the screenshot as the top layer in the design asset.
* Drop the opacity of the screenshot to 50%.
* Invert the colors of the screenshot.
* Everything that matches turns a 50% grey and "disappears" from view while the remaining artifacts that are misplaced generally turn hot pink and neon blue.

![An Overlay Example](/standards/_images/sample-overlay.png)

### Testing

Test all required browsers and devices early and often. A feature is not considered complete until it is tested natively in all required browser and device environments.

<a name="files-folders"></a>
## Files &amp; Folders

### Folder Structure

Our standard folder structure is as follows:

```diff
└── assets
    ├── media
    |   ├── images
    |   ├── fonts
    |   └── uploads
    ├── styles
    ├── scripts
    └── vendor
```

Additionally, an example of a typical sass directory structure can be found in [the sass section](/standards/scss.md#folder-structure).

### File Names

Use lowercase for all file names (including images) with hyphens as delimiters between words.

The only exception to this rule is SCSS partials which should be prefixed with an underscore.

**Good Examples**

* modern.css
* global-masthead.html
* josh-with-a-horses-head.png
* _vars.scss

### Storage of Dev Assets

Do not store development assets such as wireframes, PSDs, sprite source files, etc. in the actual project respository.

# }
