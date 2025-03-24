---
title: A few Jekyll tricks
tags: jekyll
---

This post is a collection of Jekyll tips and trics


# A basic config file

# First, how to show Jekyll code in a post as text and not have it interpreted

https://stackoverflow.com/questions/47984843/jekyll-how-to-not-interpret-jekyll-ruby-lines-in-a-code-snippet-rouge


# Display the tags

```ruby
{% raw %}
    {{ post.tags | sort | join: ", " }}
{% endraw %}
```

[Reference](https://www.jflh.ca/2016-01-23-adding-and-displaying-tags-on-jekyll-posts)


# Add a table of contents



# Add a caption under images

https://stackoverflow.com/questions/19331362/using-an-image-caption-in-markdown-jekyll/30366422#30366422


# Add support for math formulas

Use KatEx

# A few changes to the CSS

Very easy with Firefox Inspect. Check what properties are used to render a specific element, then change the css style.

# Changing the font size



# customizing the logo



# Which image format to use


# What is vertical rythm


# Custom code highlighting

I like the GitHub style and I want to reproduce it. Now the page is hosted on GitHub Page, that doesn't mean the GitHub default highlighting is available! We need to recreate the style as well as the copy button. Not a fan of Gists.

# Custom archive page by year

https://stuntbox.com/blog/jekyll-archives-group-posts-by-year/#solution

Make sure to rename the page "archive.html" to avoid it being processed by the markdown parser.

# Custom 

# How to add a subtitle

add a "subtitle" property in the front matter and reuse it in the template like so:

```md
---
title: Hello World
subtitle: This is the subtitle
```
```

and in `_layouts/post.html':

```html
{% raw %}
{%- if page.subtitle -%}<span class="post-subtitle"?>{{ page.subtitle }}</span>{%- endif -%}
{% endraw %}
``

# Making callout boxes

https://github.com/just-the-docs/just-the-docs/issues/171#issuecomment-538794741

# Auto hyphens