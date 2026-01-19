---
title: Updating Jekyll and GitHub
date: 2026-01-17
categories: [Webhosting]
tags: [jekyll,github,webhosting]
---

Commands needed to update Jekyll and Github to host this blog:

* Create a file in the _posts folder with the format YYYY-MM-DD-Title.md
* Build the site locally. The following command generates the _site folder and checks for any build errors before pushing to GitHub
* Locally serve the site to preview the changes.This is optional but displays the site locally to make sure its displayed as you want it to be
* Add your changes to Git
* Commit your changes to Git
* Push your changes to GitHub


Build the site locally:

```console
bundle exec jekyll build
```

Serve the site locally to preview changes. Visit http://localhost:4000 to view. You dont need to rebuild/re-serve the site if you make any changes to the page, just refresh the web browser:

```console
Bundle exec jekyll serve
```

Add changes to Git:

```console
git add .
```

Commit changes to Git:

```console
git commit -m "Your commit comments"
```

Push changes to GitHub. This will enable github to automatically regenerate your website:

```console
git push
```


Full list of commands below:

* Jekyll commands:

```console
bundle exec jekyll build
bundle exec jekyll serve
```

* Git commands: 

```console
git add .
git commit -m "Website update"
git push
```
