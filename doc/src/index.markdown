---
title: Torch Documentation Template
layout: doc
---

#Torch Documentation Template

This template allows you to document your Torch package using [Github Flavoured Markdown](https://help.github.com/articles/github-flavored-markdown), and automatically generate nicely styled HTML with automatic table of content, to be distributed along with the source and published online.

##Examples

Four live examples:

* this very page!
* the rendering of [torch.Tensor documentation](tensor.html) and [its raw source](https://github.com/jucor/torch-doc-template/blob/master/doc/src/tensor.md)
* the [cephes package](http://jucor.github.io/torch-cephes)
* the [randomkit package](http://jucor.github.io/torch-randomkit)

##Add to your package

###Pre-requisites

* [Ruby](https://www.ruby-lang.org/en/downloads/) and [RubyGems](http://rubygems.org/pages/download/): they are installed by default on OSX, and are packaged with all major distributions.
* [bundler](http://bundler.io/#getting-started), which ensures that all the Ruby packages needed are automatically installed in their adequate version:

    ```bash
    gem install bundler
    ```
* [Jekyll](http://jekyllrb.com/docs/installation/), which does the site building and uses Redcarpet to do the rendering from Github Flavoured Markdown to HTML:

    ```
    gem install jekyll
    ```

###Merge this template in your repository

From the root of your repository, in the `master` branch:

```bash
git remote add template https://www.github.com/jucor/torch-doc-template
git fetch template
git merge -X ours template
```

This will create a folder `doc/src`, that you can edit, and `doc/html`, that will be automatically built by Jekyll and that you will then publish.

##Generate HTML

After editting your Markdown files in `doc/src`, you need to convert it to HTML locally, with automatic Table of Contents, styles and layouts, using Jekyll: 

```bash
cd doc/src
bundle exec jekyll build --destination ../html
```

Your users can now browse your documentation with their browser, opening [doc/src/index.html], and you can distribute it along with your package.

> **Optional but convenient**: while you edit, Jekyll can dynamically build your pages, refresh them, and serve them to [http://localhost:4000](http://localhost:4000):

> ```bash
cd doc/src
bundle exec jekyll serve --destination ../html --watch
```


##Publish on Github Pages

It is recommended to have the pages published online, too. If your project is on Github, using Github Pages facility is easy.

**Note**: even though Github Pages runs a version of Jekyll on their server, it
forbid plugins, i.e. no automatic Table of Contents, and would not allow you to distribute the HTML rendering of your documentation along with your source for local browsing. This is why this template recommends to use your own Jekyll locally and only push HTML pages to GH-Pages.

###First time only: create gh-pages branch

Create a branch named `gh-pages`, where [Github Project Pages for content](https://help.github.com/articles/user-organization-and-project-pages). The `git subtree` command moves all files from `doc/html` to the root of the `gh-pages` branch.

```bash
git subtree split --prefix doc/html --branch gh-pages
```

###Merge and push online

Now, everytime you have modified `doc/src`, updated locally the `doc/html` with Jekyll, <b>and committed both to your `master` branch</b>, you just:

1. Commit to your `master` branch the changes you just made:

    ```bash
    git commit -a -m "Regenerate HTML pages"
    ```

2. Update the `gh-pages` with only the HTML part, automatically moved to the root, by:

    ```bash
    git checkout gh-pages
    git merge -X theirs -X subtree=doc/html master
    ```
3. And push online:

    ```bash
    git push -u origin gh-pages
    ```

Your files will now be accessible at [http://yourlogin.github.io/yourrepository](http://jucor.github.io/torch-randomkit).

##Personalize

To modify your documentation:

* Change `index.markdown` and add any markdown or HTML files:

    **Important**: Jekyll only converts and adds ToC to Markdown files that start with the header:

    ```yaml
    --------
    title: My title
    layout: doc
    --------
    ```

    If your Markdown files are not converted, make sure that you have added this header at the beginning. HTML files do not need this header.

* `_layouts`: contains the template rendering, including the call to the table of contents plugin.
* `Gemfile`: specifies which version of Jekyll and redcarpet to use, to ensure maximum rendering reproducibility.


##Going further

* The [Torch team](http://torch.ch), for recently moving to MarkDown with their new distribution!
* [Jekyll documentation](http://jekyllrb.com/docs/home/)
* [Using Jekyll with Github pages](https://help.github.com/articles/using-jekyll-with-pages)
* [FancyToC with Redcarpet plugin](http://jekyll.alphavice.com/source/_plugins) -- smart way to call Redcarpet's ToC generator from Jekyll
* [Github Flavoured Markdown Syntax](https://help.github.com/articles/github-flavored-markdown)
