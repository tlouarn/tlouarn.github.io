---
Title: Getting started with Github pages
---

Target:
- hosted on github
- blog 100% managed from github (i don't want to have to download the repo locally, edit, and push)
- using actions (hosted linux instances on github parameterized via yml files)
- 

# Getting started:
- static website generator
- yml format
- github actions

# Going further

## How to add a table of contents:
- add `markdown: kramdown` to `_config.yml`
- on each post, add a dummy ordered/unordered list item followed by `{:toc}` like so

```text
* Dummy line
{:toc}
```

This will generate an unordered table of contents.

## How to add LaTeX equations

Use MAthjax

Test
 {% raw %}
  $$a^2 + b^2 = c^2$$ --> note that all equations between these tags will not need escaping! 
 {% endraw %}


Step 1:

Create a new repository named {username}.github.io
Add 2 files:
```bash
_config.yml
main.md
```

Step 2:
Code and automation: Pages
under "build and deployment", under "source", select Github Actions
Github will suggest the workflow jekyll-gh-pages.yml
Validate
this will add a new folder at the root of the repository called .github/worklows inwhich there will be jekyll-gh-pages.yml

from now on: each new push to the main branch will trigger a website refresh.
the generated html is stored by Github in a special `_site` folder that can't be accessed from the repo.
