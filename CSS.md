CSS Style Guide
===============

This style guide mostly applies to the extended CSS syntax called Sass but due to it's similar syntax to CSS it can be used for CSS as well in most cases.

Coding Style
------------

* Keep lines fewer than 80 characters long. Enable the margin line option in your IDE if you have this function available.
* Use soft-tabs (spaces instead of `\t` characters) with **four spaces** per indentation level. This might be a setting in your IDE; `TAB ->` key presses should create four spaces  of indention automatically.
* Use `image-url()` instead of `url()` so that the [asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html#css-and-sass) can insert the proper resource file name when the css is generated.
* Use `//` for comment blocks (instead of `/* */`). This is supported thanks to Sass.
* Put spaces after `:` in property declarations.
* Put spaces before `{` in rule declarations.
* Put `{` on the same line as rule declarations.
* Use web safe hex color codes like `#000` when possible. You typically don't need the extra precision from the 24-bit color range (i.e. `#f1f1f1`).
* Use `rgba(0,0,0,0.5)` only if you need opacity control.
* Never use a unit (`px`, `em`, etc.) when the value is `0`.
* Use EMs (`em`) for font sizes. Here is a handy [px to em converter](http://pxtoem.com/).
* List properties in alphabetical order.

Here is good example syntax:

```scss
body {
    background: #555 image-url("footer_bg.gif");
    color: #666;
    font: normal 0.85em Arial, sans-serif;
    margin: 0;
    padding: 0;
}
```

Naming
------
Use dashes (`-`) in class names and IDs to separate words. The terms should go from general to specific. For example, `header-inner-logo` is a good ID name but `logo-inner-header` is not.

Specificity (classes vs. ids)
-----------------------------

Elements that occur **exactly once** inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

* **Good** candidates for ids: header, footer, modal popups.
* **Bad** candidates for ids: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class names over ids), prefer direct descendant selectors by default, and use as little specificity as possible. Here is a good example:

```html
<ul class="category-list">
  <li class="item">Category 1</li>
  <li class="item">Category 2</li>
  <li class="item">Category 3</li>
</ul>
```

```scss
ul.category-list { // element + class namespace

  & > li { // direct descendant selector > for list items
    list-style-type: disc;
  }

  a { // minimal specificity for all links
    color: #f00;
  }
}
```
### CSS Specificity guidelines

If you must use an id selector (`#selector`) make sure that you have no more than one in your rule declaration. A rule like `#header .search #quicksearch { ... }` is considered harmful.

The class names `disabled`, `warning`, `danger`, `success`, `hover`, `selected`, and `active` should always be namespaced by a class (`btn.danger` is a good example).


SCSS
----

### Introduction

Sass has two syntaxes. We are using the SCSS syntax or "Sassy CSS" and the filename ends with `.scss` (and `.css` before that, which has to do with the Rails asset pipeline). The other (older) syntax is the "indented syntax" or just "Sass". Here how the two compare:

```scss
/* SCSS */
$blue: #3bbfce;
$margin: 16px;

.content-navigation {
    border-color: $blue;
    color:
    darken($blue, 9%);
}

.border {
    padding: $margin / 2;
    margin: $margin / 2;
    border-color: $blue;
}
```
Scss is a superset of CSS which means that any CSS code is valid Scss. Sass however isn't like that since it omits `{`, `}` and `;` from the code:

```sass
/* Sass */
$blue: #3bbfce
$margin: 16px

.content-navigation
    border-color: $blue
    color: darken($blue, 9%)

.border
    padding: $margin / 2
    margin: $margin / 2
    border-color: $blue
```
Always use the **SCSS** syntax when creating a new stylesheet.

### Variables & Functions
Sass supports the use of variables. This can be handy for repeatedly using a color that should remain consistent everywhere it's used.

```scss
/* SCSS */
$blue: #3bbfce;
$margin: 16px;

.content-navigation {
  border-color: $blue;
  color:
    darken($blue, 9%);
}

.border {
  padding: $margin / 2;
  margin: $margin / 2;
  border-color: $blue;
}
```
Sass also supports [numerous functions](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html). In the example above `$blue` was darkened by `9%` using the provided function. Here is the resulting CSS:

```css
/* CSS */
.content-navigation {
  border-color: #3bbfce;
  color: #2b9eab;
}

.border {
  padding: 8px;
  margin: 8px;
  border-color: #3bbfce;
}
```

### Nesting

Sass avoids repetition by nesting selectors within one another. The same thing works with properties.

```scss
/* SCSS */
table.hl {
    margin: 2em 0;
    td.ln {
    text-align: right;
    }
}

li {
    font: {
    family: serif;
    weight: bold;
    size: 1.2em;
    }
}
```
```css
/* CSS */
table.hl {
    margin: 2em 0;
}
table.hl td.ln {
    text-align: right;
}

li {
    font-family: serif;
    font-weight: bold;
    font-size: 1.2em;
}
```

### Parent References

What about pseudo classes, like `:hover`? There isn’t a space between them and their parent selector, but it’s still possible to nest them. You just need to use the Sass special character `&`. In a selector, `&` will be replaced verbatim with the parent selector.

```scss
a {
  color: #ce4dd6;
  &:hover { color: #ffb3ff; }
  &:visited { color: #c458cb; }
}
```
```css
a {
  color: #ce4dd6; }
  a:hover {
    color: #ffb3ff; }
  a:visited {
    color: #c458cb; }
```

### Mix-ins

Even more useful than variables, mixins allow you to re-use whole chunks of CSS, properties or selectors. You can even give them arguments.

```scss
/* SCSS */
@mixin table-base {
  th {
    text-align: center;
    font-weight: bold;
  }
  td, th {padding: 2px}
}

@mixin left($dist) {
  float: left;
  margin-left: $dist;
}

#data {
  @include left(10px);
  @include table-base;
}
```

```css
/* CSS */
#data {
  float: left;
  margin-left: 10px;
}
#data th {
  text-align: center;
  font-weight: bold;
}
#data td, #data th {
  padding: 2px;
}
```

### Selector Inheritance

Sass can tell one selector to inherit all the styles of another without duplicating the CSS properties.

```scss
/* SCSS */
.error {
  border: 1px #f00;
  background: #fdd;
}
.error.intrusion {
  font-size: 1.3em;
  font-weight: bold;
}

.badError {
  @extend .error;
  border-width: 3px;
}
```

```css
/* CSS */
.error, .badError {
  border: 1px #f00;
  background: #fdd;
}

.error.intrusion,
.badError.intrusion {
  font-size: 1.3em;
  font-weight: bold;
}

.badError {
  border-width: 3px;
}
```

CSS 3
-----
Starting from Rails 3.1, the asset pipeline allows us to use Sass and with it you can use the [compass gem](http://compass-style.org/) to be more productive. Compass will generate cross-browsers css code for you. For example, to get `border-radius` working in all browsers that support it, you need to supply `-vendor-` prefixes for each browser engine and then finally provide the official CSS 3 spec:

```scss
/* SASS + Compass */
@import "compass"; // This line is needed above all scss files that use compass / CSS 3

.rounded {
    @include border-radius(5px);
}
```
```css
/* CSS */
.rounded {
    -webkit-border-radius: 5px; /* Chrome 4, Safari 3.1 - 4 */
       -moz-border-radius: 5px; /* Firefox 2 - 3.6 */
            border-radius: 5px; /* CSS 3 (Firefox 4+, Chrome 5+, Safari 5+, IE 9+, Opera) */
}
```