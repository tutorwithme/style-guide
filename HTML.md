HTML Style Guide
================

This style guide applies to regular HTML files (.html, .htm), Embeddded Ruby HTML files (\*.html.erb) and HTML embedded in Javascript files (\*.js.coffee, \*.js).

Doctype
-------
For HTML 5, the doctype is quite easy to remember:

```html
<!DOCTYPE html>
```

Whitespace
----------

* Keep lines fewer than 80 characters long. Enable the margin line option in your IDE if you have this function available.
* Use soft-tabs (spaces instead of `\t` characters) with **two spaces** per indentation level. This might be a setting in your IDE; `TAB ->` key presses should create four spaces  of indention automatically.
* Hard break lines of text that are longer than 80 characters.
* Break long tags after attributes:

    ```html
    <a href="http://www.w3.org/TR/2012/WD-html5-20120329/"
       title="HTML5 W3C Draft Document" target="_blank" />
    ```
* Indent the content of block tags if the content spans more than a single line:

    ```html
    <p>This is a short paragraph.</p>
    <p>
      This is a long paragraph and requires more than one line. Mauris
	  condimentum, ipsum vel laoreet rhoncus, lacus massa convallis mi, et
	  tincidunt ligula dui vel felis. Sed ac orci vel justo rhoncus auctor ut
	  eget magna. Cum sociis natoque penatibus et magnis dis parturient montes,
	  nascetur ridiculus mus.
    </p>
    ```

Tags and Attributes
-------------------
* Write all tags and attributes in lower case.
* Use **double** quotes (`"`) for attributes even though they are optional.
* Use HTML 5 data attributes (`data-*`) to add meta-data over JavaSscript solutions.

    ```html
    <li data-id="5" data-count="3">Category 5</li>
    ```

Guidelines
----------
* Paragraphs of text should always be placed in a `<p>` tag. Never use multiple `<br>` tags.
* Use the old HTML form for line-breaks (`<br>`) over the XHTML form (`<br />`).
* Items in list form should always be in `<ul>`, `<ol>`, or `<dl>`, Never a set of `<div>` or `<p>`.
* Every form input that has text attached should utilize a `<label>` tag. ***Especially radio or checkbox elements.***

    ```html
    <label>
      <input type="radio" name="answer" value="1" /> Yes
    </label>
    <label>
      <input type="radio" name="answer" value="0" /> 
    </label>
    ```   

* Always include `<thead>` and `<tbody>` tags in tables even though you don't really need them.