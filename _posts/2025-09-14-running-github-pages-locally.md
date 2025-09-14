---
title: Running GitHub Pages locally
subtitle: How to run a github pages website locally using Docker on Windows
tags: docker jekyll
---

If, like me, you are building a personal site or project page with [GitHub Pages](https://docs.github.com/en/pages), you have probably wanted to preview it locally before pushing changes live. Luckily, with **Docker** and **Jekyll**, you can spin up a local version of your site in minutes, running in a clean, isolated container. Here is a step-by-step guide tested under Windows 11 Professional Edition.

# 1. Install Docker for Windows

If you haven’t already, download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/). This will let you run containerized services on your machine. By default, Docker Desktop will run with WSL 2 (Windows Subsystem for Linux).

Once installed, check that Docker is working by opening a terminal window and typing:

```bash
docker --version
```

# 2. Clone Your GitHub Pages repository

By convention, the GitHub Pages site for user **username** is hosted in a repo named **username.github.io**. For instance, the blog you're reading is hosted at [https://github.com/tlouarn/tlouarn.github.io](https://github.com/tlouarn/tlouarn.github.io). 

Clone your repo locally using:

```bash
git clone https://github.com/username/username.github.io
```

# 3. Add a docker-compose.yml File

At the root of your project, create a `docker-compose.yml` file. This file defines which containers to run. 

Paste the following:

```bash
version: '2.4'

services:
  jekyll:
    image: bretfisher/jekyll-serve
    volumes:
      - .:/site
    ports:
      - '80:4000'
```

This sets up a Jekyll service using the **bretfisher/jekyll-serve** image and maps port 4000 inside the container to port 80 on your machine. When the container starts, everything in the local project directory is accessible inside the container at `/site` (in particular, the generated website will be served from `/site/_site`).

# 4. Add a Gemfile

A **Gemfile** is a plain-text manifest that tells **Bundler** (Ruby’s dependency manager) what libraries (called *gems*) your project requires and which versions to use. It ensures that anyone who clones the repository can install exactly the same dependencies, keeping the development and production environments in sync.

Create a Gemfile with the following content:

```bash
source "https://rubygems.org"

gem 'github-pages', group: :jekyll_plugins
```

This ensures your local Jekyll environment matches GitHub Pages exactly. The **github-pages** gem pins versions of Jekyll and all supported plugins so what you see locally is what you’ll get when GitHub builds your site.

# 5. Update your .gitignore (Optional but Recommended)

Since Docker will generate extra files (like `_site`), you may want to add them to your `.gitignore` file so they don’t clutter your commits. Example:

```bash
_site/
```

# 6. Run the site locally

From the project root, start Docker with:

```bash
docker-compose up
```

This will spin up a Jekyll server inside Docker, generate your site into the `_site` folder and serve it at [http://localhost/](http://localhost/). The best part? **Live reload** is enabled, so changes you make to your files will instantly update in your browser.

---

References:

- [https://docs.docker.com/get-started/](https://docs.docker.com/get-started/)
- [https://blog.nimblepros.com/blogs/github-pages-with-dev-containers/](https://blog.nimblepros.com/blogs/github-pages-with-dev-containers/)
- [https://bjm.me.uk/blog/testing-github-pages-locally-docker/](https://bjm.me.uk/blog/testing-github-pages-locally-docker/)
