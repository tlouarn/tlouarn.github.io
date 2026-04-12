---
title: Moving to GitHub Pages
subtitle: Some tips and tricks I learnt while migrating my blog
tags: howto web
---

Over the years, I’ve experimented with various platforms and setups to host this blog: a WordPress site,   a custom Python MVC backend running on AWS Lambda, a Hugo static website, a [self-hosted Ghost instance]({% post_url 2023-07-12-hosting-ghost-on-aws-ec2 %}) etc. However, rising costs eventually led me to migrate to GitHub Pages, which is free and does the job.

# What is GitHub Pages

[GitHub Pages](https://docs.github.com/en/pages) is a free service provided by GitHub that allows you to host a static website directly from one of your GitHub repositories. It is based on a static site generator named [Jekyll](https://jekyllrb.com/), which serves the content located in a special repository named `{username}.github.io` (each GitHub user can create one such special repository). Thanks to GitHub Actions, each commit to this specific repository triggers a build of the static website. In other words, once the setup is in place, all you need to do is push a new post to the repository for your website to update.

# Getting started

Create a new repository named `{username}.github.io` 

Add 2 files:
```bash
_config.yml
main.md
```

Let's now setup GitHub Actions for this repository. A GitHub Action is hosted by GitHub on a Linux instance and can be parameterized using YAML files.

Go to the repository settings. Under `Code and automation`, select `Pages`. Under `Build and deployment`, select `Deploy from a branch` and choose `main`. From now on, each new push to the main branch will trigger a website refresh. The generated html is stored by Github in a special `_site` folder that can't be accessed from the repo.

The website is now publicly available at `https://{username}.github.io`, but it is also possible to configure a custom domain name. The process involves two steps:
1. securing the domain name within GitHub to prevent domain takeover (see details [here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages))
2. configuring the apex domain at your DNS provider

# Website structure

In the website repository, the folder structure looks like the below:

```bash
# The GitHub Actions
.github/workflows
    jekyll-gh-pages.yml

# A .gitignore is always welcome
.gitignore

# The _config.yml file contains all the parameters for Jekyll
_config.yml

# Posts are simple MarkDown files located in the _posts directory
_posts
    2020-01-01-example-post.md
    2020-02-01-example-post-2.md
    ...

# Images are contained within the assets/images directory
assets/images
    image1.jpeg
    image2.jpeg
    ...

# The main pages are located directly at the root (they can be either HTML or MarkDown)
404.html
archive.html
index.md

# A CNAME file can be required for domain name verification
CNAME
```

You can view the structure of this blog at [https://github.com/tlouarn/tlouarn.github.io](https://github.com/tlouarn/tlouarn.github.io).


# Some customization tips

**How to add a table of contents:**
- add `markdown: kramdown` to `_config.yml`
- on each post, add a dummy ordered/unordered list item followed by `{:toc}`

For instance, I use:
```markdown
**Table of Contents:**
* 
{:toc}
```

This will generate an unordered table of contents.

**How to add LaTeX equations**

I use [KaTeX](https://katex.org/) via a simple JavaScript insert in my header:

```html
<!-- KaTex -->
<link async rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.css" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.js" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/contrib/auto-render.min.js" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>
```

KaTeX expressions must be escaped using `\\[` and `\\]`. Here is how to write a simple equation:

```code
\\[ C - P= D \cdot (F - K) \\]
```
and how it is rendered:

\\[ C - P= D \cdot (F - K) \\]


**How to customize CSS**

GitHub Pages currently runs on minima v2.5.1. In order to overwrite or add custom css, the easiest way is to create `assets/main.scss` and import the minima style inside, then overwrite it.

```scss
---
# Only the main Sass file needs front matter (the dashes are enough)
---

@import "minima";

body {
    font: Helvetica,Arial,sans-serif;
    letter-spacing:  -1px;
}
```

A better way is to fork the [minima theme](https://github.com/jekyll/minima) into a custom repo (I chose `tlouarn/minima`), strip all the unnecessary files and customize it by overwriting styles in `_sass/minima/custom-variables.scss` and `_sass/minima/custom-styles.scss`. Back to the main repo, add the following line to `_config.yml`:

```yaml
remote_theme: tlouarn/minima
```

This way, content and presentation are cleanly separated. Also, the actual theme is now frozen (in case GitHub chooses to update the default theme at a later stage).


**How to add images**

Image files need to be uploaded into `assets/images` and can be referenced like so:

```markdown
![Image](/assets/images/image.jpg)
```

**How to link other posts**

To create a hyperlink to another post named "2020-01-01-example-post.md", enter the below:

```markdown
[link text]({% raw %}{% post_url 2020-01-01-example-post %}{% endraw %})
```
