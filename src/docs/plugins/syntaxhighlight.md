---
eleventyNavigation:
  key: Syntax Highlighting
  order: 2
  excerpt: Code syntax highlighting using PrismJS without client-side JavaScript.
---
# Syntax Highlighting Plugin

A pack of Eleventy plugins for syntax highlighting. No browser/client JavaScript here, these highlight transformations are all done at build-time. Supports individual line highlighting.

* This is the documentation for `eleventy-plugin-syntaxhighlight` `v3.x`.
* [GitHub](https://github.com/11ty/eleventy-plugin-syntaxhighlight).

## Contents

[[toc]]

## Template Compatibility

* Nunjucks
* Liquid
* Markdown

## Installation

Available on [npm](https://www.npmjs.com/package/@11ty/eleventy-plugin-syntaxhighlight).

```
npm install @11ty/eleventy-plugin-syntaxhighlight --save-dev
```

Open up your Eleventy config file (probably `.eleventy.js`) and use `addPlugin`:

{% codetitle ".eleventy.js" %}

```js
const syntaxHighlight = require("@11ty/eleventy-plugin-syntaxhighlight");

module.exports = function(eleventyConfig) {
  eleventyConfig.addPlugin(syntaxHighlight);
};
```

{% callout "info", "md" %}You’re only allowed one `module.exports` in your configuration file, so make sure you only copy the `require` and the `addPlugin` lines above!{% endcallout %}

You are responsible for including your favorite PrismJS theme CSS and there are many ways to do that. The default themes are provided by [several CDNs](https://prismjs.com/#basic-usage-cdn) and could be easily included in a base layout, like in the example below;

```html
<html lang="en">
  <head>
    <!-- Some html boilerplate omitted -->
    <link href="https://unpkg.com/prismjs@1.20.0/themes/prism-okaidia.css" rel="stylesheet">
  </head>
````

You could also download the css file or paste its content inside a style tag. This approach allows the use of [other themes](https://github.com/PrismJS/prism-themes) from a Prism extension repository.

### Options

Optionally pass in an options object as the second argument to `addPlugin` to further customize this plugin pack.

```js
const syntaxHighlight = require("@11ty/eleventy-plugin-syntaxhighlight");

module.exports = function(eleventyConfig) {
  eleventyConfig.addPlugin(syntaxHighlight, {

    // Change which Eleventy template formats use syntax highlighters
    templateFormats: ["*"], // default

    // e.g. Use syntax highlighters in njk and md Eleventy templates (not liquid)
    // templateFormats: ["njk", "md"],

    // init callback lets you customize Prism
    init: function({ Prism }) {
      Prism.languages.myCustomLanguage = /* */;
    },

    // Added in 3.0, set to true to always wrap lines in `<span class="highlight-line">`
    // The default (false) only wraps when line numbers are passed in.
    alwaysWrapLineHighlights: false,

    // Added in 3.0.2, set to false to opt-out of pre-highlight removal of leading
    // and trailing whitespace
    trim: true,

    // Added in 3.0.4, change the separator between lines (you may want "\n")
    lineSeparator: "<br>",

    // Added in 3.1.1, add HTML attributes to the <pre> or <code> tags
    preAttributes: {
      tabindex: 0
    },
    codeAttributes: {},
  });
};
```

## Usage

This plugin provides the following syntax highlighters using PrismJS, all of which currently support individual line highlighting.

* Markdown Highlighter (triple backtick <code>```</code>)
* Liquid Custom Tag {% raw %}`{% highlight %}`{% endraw %}
* Nunjucks Paired Shortcode {% raw %}`{% highlight %}`{% endraw %}

### Markdown Highlighter

Optionally specify a language after the start of the markdown fenced code block.

* [List of supported PrismJS languages](http://prismjs.com/#languages-list)

````markdown
<!-- Markdown Template -->
``` js
function myFunction() {
  return true;
}
```
````

````markdown
<!--
  Line highlighting classes (single highlight)
  Wraps each line in `<span class="highlight-line">`
  Adds `highlight-line-active` class to lines 1,3,4,5 (for line highlighting)
-->

<!-- Markdown Template -->
<!-- Line highlighting numbers are zero-indexed. -->
``` js/1,3-5
function myFunction() {
  // …
  return true;
}
```
````

````markdown
<!--
  Line highlighting classes (add and remove mode)
  Wraps each line in `<span class="highlight-line">`
  Adds `highlight-line-add` class to lines 1,3
  Adds `highlight-line-remove` class to lines 5,6,7,8
-->

<!-- Markdown Template -->
<!-- Line highlighting numbers are zero-indexed. -->
``` js/1,3/5-8
function myFunction() {
  // …
  return true;
}
```
````

#### Plain text

Use `text` to use the line highlighting features without PrismJS.

````markdown
<!-- Line highlighting numbers are zero-indexed. -->

``` text/1-2
function myFunction() {
  let highlighted = true;
  return highlighted;
}
```
````

### Liquid Tag: Prism Syntax Highlighter

* [List of supported PrismJS languages](http://prismjs.com/#languages-list)

{% raw %}
```markdown
<!-- Liquid Template -->
{% highlight js %}
function myFunction() {
  return true;
}
{% endhighlight %}
```
{% endraw %}

{% raw %}
```markdown
<!--
  Line highlighting classes (single highlight)
  Wraps each line in `<span class="highlight-line">`
  Adds `highlight-line-active` class to lines 1,3,4,5 (for line highlighting)
-->

<!-- Liquid Template -->
<!-- Line highlighting numbers are zero-indexed. -->
{% highlight js 1,3-5 %}
function myFunction() {
  // …
  return true;
}
{% endhighlight %}
```
{% endraw %}

{% raw %}
```markdown
<!--
  Line highlighting classes (add and remove)
  Wraps each line in `<span class="highlight-line">`
  Adds `highlight-line-add` class to lines 1,3
  Adds `highlight-line-remove` class to lines 5,6,7,8
-->

<!-- Liquid Template -->
<!-- Line highlighting numbers are zero-indexed. -->
{% highlight js 1,3 5-8 %}
function myFunction() {
  // …
  return true;
}
{% endhighlight %}
```
{% endraw %}

#### Plain text

Use `text` to use the line highlighting features without PrismJS.

{% raw %}
```markdown
<!-- Liquid Template -->
<!-- Line highlighting numbers are zero-indexed. -->
{% highlight text 1-2 %}
function myFunction() {
  let highlighted = true;
  return highlighted;
}
{% endhighlight %}
```
{% endraw %}

### Nunjucks Paired Shortcode: Prism Syntax Highlighter

* [List of supported PrismJS languages](http://prismjs.com/#languages-list)

{% raw %}
```markdown
<!-- Nunjucks Template -->
{% highlight "js" %}
function myFunction() {
  return true;
}
{% endhighlight %}
```
{% endraw %}

{% raw %}
```markdown
<!--
  Line highlighting classes (single highlight)
  Wraps each line in `<span class="highlight-line">`
  Adds `highlight-line-active` class to lines 1,3,4,5 (for line highlighting)
-->

<!-- Nunjucks Template -->
<!-- Line highlighting numbers are zero-indexed. -->
{% highlight "js 1,3-5" %}
function myFunction() {
  // …
  return true;
}
{% endhighlight %}
```
{% endraw %}

{% raw %}
```markdown
<!--
  Line highlighting classes (add and remove)
  Wraps each line in `<span class="highlight-line">`
  Adds `highlight-line-add` class to lines 1,3
  Adds `highlight-line-remove` class to lines 5,6,7,8
-->

<!-- Nunjucks Template -->
<!-- Line highlighting numbers are zero-indexed. -->
{% highlight "js 1,3 5-8" %}
function myFunction() {
  // …
  return true;
}
{% endhighlight %}
```
{% endraw %}

#### Plain text

Use `text` to use the line highlighting features without PrismJS.

{% raw %}
```markdown
<!-- Nunjucks Template -->
<!-- Line highlighting numbers are zero-indexed. -->
{% highlight "text 1-2" %}
function myFunction() {
  let highlighted = true;
  return highlighted;
}
{% endhighlight %}
```
{% endraw %}

### Sample Line Highlighting CSS

```css
.highlight-line {
  display: inline-block;

  /* del, ins, mark default styles */
  text-decoration: none;
  color: inherit;
}

/* allow highlighting empty lines */
.highlight-line:empty:before {
  content: " ";
}

.highlight-line:not(:last-child) {
  min-width: 100%;
}
.highlight-line .highlight-line:not(:last-child) {
  min-width: 0;
}


/*
 * Dark theme
 */

.highlight-line-isdir {
  color: #b0b0b0;
  background-color: #222;
}
.highlight-line-active {
  background-color: #444;
  background-color: hsla(0, 0%, 27%, .8);
}
.highlight-line-add {
  background-color: #45844b;
}
.highlight-line-remove {
  background-color: #902f2f;
}
```
