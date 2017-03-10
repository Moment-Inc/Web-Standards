# CSS

**Contents**

- [Terminology](#terminology)
	- [Rule Declaration](#rule-declaration)
	- [Selectors](#selectors)
	- [Properties](#properties)
- [Syntax & Formatting](#syntax--formatting)
	- [ID Selectors](#id-selectors)
	- [Quotes](#quotes)
	- [Numbers](#numbers)
	- [Lists](#lists)
	- [Maps](#maps)
	- [Rule Ordering](#rule-ordering)
	- [State Rules](#state-rules)
	- [JavaScript Hooks](#javascript-hooks)
+ [Nesting Selectors](#nesting-selectors)
+ [CSS Selectors](#css-selectors)
	- [Reusability](#reusability)
	- [Location Independence](#location-independence)
	- [Portability](#portability)
	- [Performance](#performance)
- [Comments](#comments)
- [Advanced Sass](#advanced-sass)
	- [Mixins](#mixins)
	- [Placeholders](#placeholders)
- [Suggested](#suggested)
	- [Property Ordering](#property-ordering)
	- [Naming Conventions](#naming-conventions)

---
#Terminology
###Rule declaration

Name given to a selector with an accompanying group of properties.

```css
.rule {
  display: block;
  background-color: #000;
}
```

###Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, class, ID, or attributes.

```css
.element-class {
  /* */
}
[type="text"] {
  /* */
}
```

Further Reading:

- [Mozilla Developer Network: Attribute Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)
- [CSS-Tricks: Attributes](https://css-tricks.com/almanac/selectors/a/attribute/)
- [Jenkov: Attributes](http://tutorials.jenkov.com/css/selectors.html#attribute-selector)

###Properties

Properties give the selected elements of a rule declaration their styles.

```css
.some-selector {
  display: block;
  color: #000;
}
```
----------
#Syntax & Formatting

Having a standard way of writing CSS means that code will always look and feel familiar to all members of the team. Code guidelines promote consistency, ensure a better environment to work in, and help with reading and updating of code.

- SASS or SCSS
- Two (2) space indents, no tabs.
- Follow BEM naming convention.
- Do not use ID selectors.
- Properly written multi-line CSS rules.
- One selector per line, one rule per line.
- Include a single space before the opening brace of a ruleset.
- Include a space after each comma in comma-separated property or function values.
- Include a semi-colon at the end of the last declaration in a declaration block.
- The closing brace should be on its own line.
- Use hex values.
- Separate each ruleset by a blank line.
- Avoid writing vendor prefixed CSS.

(Disclaimer: some of the syntax guidelines will not apply to Sass syntax.)

```css
// Correct
.foo,
.bar {
  display: block;
  background-color: #fff;
  color: #000;
}

// Incorrect
.foo, .bar {
    display:block; background-color: #fff;
  color: #000 }
```

###ID Selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessary high level of specificity to your rule declarations. Most importantly, they are not re-usable. And they are put to better use by using them to select elements with JS. So, in order to avoid problems with change in class names, and JS functionality being broken, do not use IDs as they are reserved for JS functionality.

###Quotes

Neither CSS nor Sass require strings to be quoted, but because the vast majority of other languages do require strings to be quoted, and for the sake of consistency, we believe strings should be quoted in all situations other than the few detailed below. Besides consistency, there are several other reasons for this choice:

- color names are treated as colors when unquoted
- most syntax highlighters will throw an error on unquoted strings
- helps with general readability

URLs:
```css
// Correct
.foo {
  background-image: url('/assets/images/image.jpg');
}

// Incorrect
.bar {
  background-image: url(/assets/images/image.jpg);
}
```

Take note: some CSS values (such as `font-family`) [should not be quoted](http://www.sitepoint.com/sass-reference/string/). These declarations (e.g.: `font-family: ‘sans-serif’`) will fail silently due to the Sass compiler expecting an identifier, not a quoted string.

```sass
// Correct
$font-type: sans-serif;
$font-type: 'Helvetica', Arial, sans-serif;

// Incorrect
$font-type: 'sans-serif';
```

###Numbers
- Never display trailing zeros.
- A 0 value should never have a unit.
- Always add a 0 before a decimal.

```css
// Correct
.foo {
  padding: 20px;
  margin: 0;
  opacity: 0.5;
}

// Incorrect
.bar {
  padding: 20.0px;
  margin: 0px;
  opacity: .5;
}
```

###Lists

- multiline
- always comma separated (even last item)
- always wrapped in parenthesis

```sass
// Correct
$list: (
  value-one,
  value-two,
  value-three,
)
$colors: (
  red $red,
  purple $purple,
  green $green,
)

// Incorrect
$list: (value-one, value-two, value-three);
$list: value-one, value-two, value-three;
$list: 'value-one', 'value-two', 'value-three';
```

Further Reading:

- [Understanding Sass lists](http://hugogiraudel.com/2013/07/15/understanding-sass-lists/)
- [Sass maps vs nested lists](http://www.sitepoint.com/sass-maps-vs-nested-lists/)
- [Working with lists & @each loops](https://benfrain.com/working-with-lists-and-each-loops-in-sass-with-the-index-and-nth-function/)

###Maps

- spaces after the colon `:`
- opening braces `(` on the same line as the colon `:`
- quoted keys if they are strings
- comma `,` at the end of each key/value
- closing brace `)` on its own new line

```sass
// Correct
$colors: (
  blue: $blue,
  gray: $gray
);
$colors: (
  blue: (
    light: #09679e,
    dark: #06486e
  ),
  orange: (
    light: #ff5316,
    dark: #e45122
  )
);

// Incorrect
$colors: (blue: $blue, gray: $gray);
```

Further Reading:

- [Using Sass Maps](http://www.sitepoint.com/using-sass-maps/)
- [Debugging Sass Maps](http://www.sitepoint.com/debugging-sass-maps/)
- [Extra Map functions in Sass](http://www.sitepoint.com/extra-map-functions-sass/)
- [Sass Maps are Awesome](https://viget.com/extend/sass-maps-are-awesome)
- [Introduction to Sass Maps & Example Usage](http://webdesign.tutsplus.com/tutorials/an-introduction-to-sass-maps-usage-and-examples--cms-22184)

###Rule Ordering
- @extend
- @include
- Regular style properties
- Nested Pseudo Classes & Pseudo Elements
- Nested Selectors

```sass
.foo {
  $scoped-variable: 'bar';
  @extend %module;
  @include type(12, 14);
  background: #000;
  &.nested-class {
    color: #fff;
  }
  &:before {
    content: "";
    @include symbols(cross);
  }
  &:hover {
    background: blue;
  }
  span {
    display: none;
  }
}
```

###JavaScript Hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript specific classes to bind to, by adding the suffix `JS`

```html
<button type="button" class="nav__trigger__button nav__trigger__buttonJS">Menu</button>
```

#Nesting Selectors

We recommend nesting selectors no more than three levels deep.


#CSS Selectors
- Avoid using ID’s for style.
- Use meaningful names: `$visual-grid-color` not `$color` or `$vslgrd-clr`.
- Avoid using the direct descendant selector `>`.
- Avoid using HTML tags in class names: `.news` not `.news-section`
- Avoid using HTML tags on classes for generic markup: `div.widgets`
- Avoid nesting within a media query.

###Location Independence

Given the ever-changing nature of most UI projects, and the move to more component-based architectures, it is in our interests not to style things based on where they are, but on what they are. That is to say, our components’ styling should not be reliant upon where we place them—they should remain entirely location independent.

A component shouldn’t have to live in a certain place to look a certain way.

[(source)](http://cssguidelin.es/#location-independence)


###Performance

Generally speaking, the longer a selector is, the slower it is, for example:

```css
body.home div.header ul {}
```
… is a far less efficient selector than:

```css
.global-nav {}
```

Browsers read CSS selectors right-to-left. A browser will read the first selector as:

* find all `<ul>` elements in the DOM
* check if they live inside an element with a class of `.header`
* check that `.header` class exists on a `<div>` element
* check that it lives anywhere insde any elements with a class of `.home`
* finally, check that `.home` exists on a `<body>` element

Where as the second selector: find all elements with a class of `.global-nav`

To further compound the problem, we are using descendant selectors (e.g. `.foo .bar {}`). The upshot of this is that a browser is required to start with the rightmost part of the selector (i.e. `.bar`) and keep looking up the DOM indefinitely until it finds the next part (i.e. `.foo`). This could mean stepping up the DOM dozens of times until a match is found.

This is just one reason why nesting with preprocessors is often a false economy; as well as making selectors unnecessarily more specific, and creating location dependency, it also creates more work for the browser.


#Comments

Use comments to separate logical groups of styles within a document.

- Prefer line comments `//` over `/* */` as line comments are ignored with compilers.
- Prefer comments on their own line. Avoid end-of-line comments.
- Write detailed comments for code that isn’t self-documenting:
  - Compatibility, browser-specific fixes, or fallback
  - Purpose of selector or rule set
  - Experimental properties
- Divide up sections with comment headers.


```sass
// ================================
// Style Sheet Name
// ================================

// --------------------------------
// Section Comment Header
// --------------------------------

// Single Line Comments
```


----------
#Advanced Sass
###Mixins

Mixins allow us to create re-usable parts of CSS without having to write repetitive code. However, far too often, mixins are written in a way that bloat the size of our file through duplication. A mixin therefore should only be used if an argument is present to modify the style.

```sass
// Correct
@mixin rgba($color, $alpha) {
  background-color: rgba($color, $alpha);
}

// Incorrect
@mixin button {
  display: inline-block;
  padding: 10px;
  font-size: 15px
}
```

Some examples where are `mixin()` usage is most fitting:

- Media Queries
- Keyframe animations
- Sprites & symbols
- Font & line-height properties

Further reading:
- [DRY-ing Out Your Sass Mixins](http://alistapart.com/article/dry-ing-out-your-sass-mixins)
- [The Mixin Directive](http://www.sitepoint.com/sass-basics-the-mixin-directive/)


###Property Ordering

Currently there are three display styles:

1. Alphabetical Order
2. Declaration Type (position, display, colors ... )
3. [Concentric CSS](https://github.com/brandon-rhodes/Concentric-CSS) (starts outside, moves inward)

|      | Alphabetical                                                        | Decelaration                                                       | Concentric                                           |
|------|---------------------------------------------------------------------|--------------------------------------------------------------------|------------------------------------------------------|
| Pros | No argument about sorting of properties.                            | Declarations are grouped together.                                 | No argument about sorting of properties.             |
| Cons | Weird not seeing width & height or bottom & top next to each other. | Lots of room for interpretation, where does white-space belong to? | Requires time to adapt & recall order of properties. |

Focusing on alphabetical or concentric requires recollection or time to sort accordingly. Utilizing declaration leaves some room for interpretation. However, declarations are grouped together and just as easy to find. There’s no hard rule set on ordering, especially when most rule declarations contain on average 5–8 properties.

Here is a suggested declaration order:

- Display
- Positioning
- Colors & Typography
- View: Opacity & Visibility
- CSS3: Animations, Transforms, Transitions
- Miscellaneous: Pointers, Overflows, Scrolling …
- Z-index

