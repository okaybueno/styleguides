# okay bueno CSS / Sass Styleguide

*A hardly reasonable approach to CSS and Sass, mostly copied from the awesome team at AirBnB*

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [SCSS Architecture](#scss-architecture)
    - [Folder Structure](#folder-structure)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
    - [Type](#type)
    - [Global Rules](#global-rules)
    - [Important](#important)
    - [Breakpoints](#breakpoints)
    - [Media Queries](#media-queries)
    - [z-index](#z-index)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Generic classes](#generic-classes)
    - [Nested selectors](#nested-selectors)
1. [License](#license)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.block {
  font-size: 1em;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```scss
/* some selector */ {
  background: $color-brand-primary;
  color: $color-bg-primary;
}
```

**[⬆ back to top](#table-of-contents)**

## SCSS Architecture

### Folder Structure
A good folder structure for your project looks sth. like this:
 
/style
|
| -- global
|    | -- _all.scss           #import all global scss files here
|    | -- _animations.scss
|    | -- _base.scss
|    | -- _breakpoints.scss
|    | -- _colors.scss
|    | -- _layout.scss
|    | -- _reset.scss
|    | -- _type.scss
| -- components
|    | -- _all.scss           #import all component scss files here
|    | -- _buttons.scss
|    | -- _forms.scss
|    ...
| -- pages
|    | -- _all.scss           #import all page scss files here
|    | -- _buttons.scss
|    | -- _forms.scss
|    ...
-- main.scss #import all all.scss files here

Don't worry, we also have a boilerplate to get you started. In general, it's important to distinguish between:

**Global**

All values that are used and shared across pages and components should be declared here: font styles, generic layouts, colors, etc.

**Components**

All reusable components go here: Buttons, Form Elements, Overlays, Tables, etc.

**Pages**

All rule declarations that are tightly tied to one specific page should go here. You can also scope components in these rule declarations for layout specific adjustments. Please do not scope components and states in here, that might be more useful as element modifiers that should be declared within the component rule declarations.


## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation.
* Prefer dashes over camelCasing in class names.
  - Underscores and PascalCasing are okay if you are using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors.
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations.
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line.
* Put blank lines between rule declarations.

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```scss
.avatar {
  border-radius: 50%;
  border: 2px solid $white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Leave a generic comment for quick fixes and hacks: "Needs refactoring"
  - Compatibility or browser-specific hacks

### BEM

BEM is awesome. Why? That's why:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind.

* CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
* Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**Example**

```jsx
// ListingCard.jsx
function ListingCard() {
return (
  <article class="listing-card listing-card--featured">

    <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

    <div class="listing-card__content">
      <p>Vestibulum id ligula porta felis euismod semper.</p>
    </div>

  </article>
);
}
```

```css
/* ListingCard.css */
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

* `.listing-card` is the “block” and represents the higher-level component
* `.listing-card--featured` is an “element” and represents a descendant of `.ListingCard` that helps compose the block as a whole.
* `.listing-card__title` is a “modifier” and represents a different state or variation on the `.listing-card` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

### Type 

In general, try to avoid mixing px, em, and rem values across the project. Ideally, we use a px baseline value set globally in base.scss and define rem values for all text classes in type.scss. Please refrain from setting font-specific properties in generic rule declarations (e. g. div, span, etc.)

### Global Rules

Do not apply any properties globally to all divs or child elements of html / body

**Really Bad**

```css
html > div {
  line-height: 2em;
}
```

### Important

Using !important is usually only the last resort — we've all been guilty of using that under time pressure. Sometimes it may be necessary with overwriting e. g. plug-ins / library styles, in which case it's still not great, but acceptable. If you know it's a weakness, please leave a comment in the code.


### Breakpoints

We should limit the number of breakpoints generally used in a project to the necessary minimum. Breakpoints should always be declared as variables. No more than 5 breakpoints should be used in any given project, special custom breakpoints may only be used in emergencies(!) or in controlling e. g. VH media queries

```scss
$breakpoint-mobile: 500px;
$breakpoint-tablet-small: 768px;
$breakpoint-tablet: 1000px;
$breakpoint-desktop: 1440px;
$breakpoint-desktop-large: 1920px;
```

### Media Queries

Ideally, we only use max-width media queries in overwrites to reduce the complexity of the code and the number of breakpoints, e. g.

```scss
.block {
  width: 60vw;
  
  @media(max-width: $breakpoint-tablet) {
    width: 80vw;
  }
  
  @media(max-width: $breakpoint-mobile) {
    width: 90vw;
  }
}
```

### z-index

To keep track of complex z-index hierarchies, please use the predefined z-index function (function z) which can be found in the layout.scss file. The function should look like this:

```scss
@function z($name) {
    @if index($z-indexes, $name) {
        @return (length($z-indexes) - index($z-indexes, $name)) + 1;
    } @else {
        @return null;
    }
}
$z-indexes: (
  'modal'
  'navbar',
)
```

An usage example can be found in this [Codepen](https://codepen.io/okay_alex/pen/xBVoxV)

**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the beginning makes the entire selector more flexible, as rules can be overwritten easily.

    ```scss
    .btn-green {
      @include transition(background 0.5s ease);
      background: $color-success;
      font-weight: bold;  
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      @include transition(background 0.5s ease);
      background: $color-success;
      font-weight: bold;
      

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity in much the same way as well-named functions. In general, mixins should only be used if they accept argument, otherwise you may want to consider creating generic classes.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Generic classes

Scope of generic classes tbd.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2015 Airbnb / 2019 okay bueno

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**
