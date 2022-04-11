---
title: "How to create a personal blog website using Doks 👋"
description: "A guide on using Doks and github pages to create an asthetic blogging website."
lead: "A guide on using Doks and github pages to create an asthetic blogging website."
date: 2022-04-11T09:19:42+01:00
lastmod: 2022-04-11T09:19:42+01:00
draft: false
weight: 50
contributors: ["Ahsan Barkati"]
---

I have been thinking of getting started with writing for a long time. I used to
procrastinate it to the threshold of creating a blogging website and hosting it.
I am not a web developer, and for me, even thinking of tinkering with HTML and CSS
creates a strong resistance. Also, I was clear that I'll write on my personal website and not on some of the online platforms (BTW, platforms like medium are great), but I wanted flexibility. If you are going through similar feelings, this article should help.

Let's get started quickly.

1. First, create a fork of [doks-gh-pages](https://github.com/h-enk/doks-gh-pages) on your github and mark it as a template repository. To do it, open the forked repo in github, go to settings and select the `Template repository` option.
{{< img-simple src="template-repo.jpg" alt="Final" class="border-0" >}}

2. Create a new repository using this template repository with repository name as `username.github.io`. To do so, click on `Use this template` option in the forked repo.
{{< img-simple src="create-repo.jpg" alt="Final" class="border-0" >}}

3. Clone your new repository (`username.github.com`) and add the following config in `.github/workflows/deploy-github.yml` in order to trigger gh-page build on code push.

```
# Deploy your Hyas site to GitHub Pages

name: GitHub Pages

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '16'
          cache: 'npm'

      - name: Install dependencies
        run: npm install

      - name: Check for linting errors
        run: npm test

      - name: Build production website
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

4. Push the changes, and follow the steps mentioned in this [doc](https://getdoks.org/docs/recipes/deployment/) to make github serve your page. Your final result should look something like below.
{{< img-simple src="final.jpg" alt="Final" class="border-0" >}}

5. Personalize it. Go to `layouts/index.html` and add the below code to have a landing page similar to mine. Do the required edits to make sure that you don't mention Ahsan Barkati on your blog's landing page.

```
{{ define "main" }}
<section class="section container-fluid mt-n3 pb-3">
  <div class="row justify-content-center">
    <div class="col-lg-12 text-center">
      <h1 class="mt-0">{{ .Title }}</h1>
    </div>
    <div class="col-lg-9 col-xl-8 text-center">
      <p class="lead">{{ .Params.lead | safeHTML }}</p>
    </div>
  </div>
  <div class="row justify-content-center">
    <div class="col-lg-9 col-xl-8 text-center">
      <img style="border-radius: 50%" src="{{ .Site.BaseURL | absURL }}/ahsan.jpg" alt="Ahsan" height="200">
    </div>
  </div>
  <div class="row justify-content-center">
    <div class="col-lg-9 col-xl-10 text-center">
      <br><br>
      Hello! I am Ahsan Barkati, a software engineer and an 
      aspiring writer from India. I love everything
      about technology, specially related to computation.
    </div>
  </div>
</section>
{{ end }}


{{ define "sidebar-footer" }}
<section class="section section-sm container-fluid">
  <div class="row justify-content-center text-center">
    <div class="col-lg-9">
      {{- .Content -}}
    </div>
  </div>
</section>
{{ end }}
```