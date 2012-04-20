CSS and Sass Style Guide
========================

This style guide mostly applies to Sass but due to it's similar syntax to CSS it can be used for CSS as well in most cases.

Sass Syntax
-----------
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
Always use SCSS syntax when creating a new stylesheet.

Margin
------

Keep lines fewer than 80 characters long. Enable the margin line option in your IDE if you have this function available.

Indention
---------
* Use soft-tabs (spaces instead of `\t` characters) with **four spaces** per indentation level. This might be a setting in your IDE; `TAB ->` key presses should create four spaces  of indention automatically.

Variables & Functions
---------------------
Sass supports the use of variables. This can be handy for repeatedly using a colour that should remain consistent everywhere it's used.

```scss
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
Sass also supports numerous functions. In the example above `$blue` was darkened by `9%` using the provided function. Here is the resulting CSS:

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

Nesting
-------

* Sass avoids repetition by nesting selectors within one another. The same thing works with properties.

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
    Here is the resulting CSS:
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
    
Mix-ins
-------

Syntax
------
* Use `image-url()` instead of `url()` so that the [asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html#css-and-sass) can insert the proper resource file name when the css is generated.

CSS 3
-----
Starting from Rails 3.1, the asset pipeline allows us to use Sass and with it you can use the [compass gem](http://compass-style.org/) to be more productive. Compass will generate cross-browsers css code for you. For example, to get `border-radius` working in all browsers that support it, you need to supply `-vendor-` prefixes for each browser engine and then finally provide the official CSS 3 spec:

```scss
/* SASS + Compass */
@import "compass"; /* This line is needed above all scss files that use compass / CSS 3 */

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