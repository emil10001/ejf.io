+++
date = "2015-08-14T10:03:04-07:00"
title = "How to Build a Hugo Site"

+++

<img alt="hugo logo" width="75%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/hugo-logo.png">

> [Image Source](http://gohugo.io/)

When I decided to build a new website, I knew that I wanted to use a static site generator that could take in Markdown, and spit out HTML. I tried a handful of different options, none of which worked out of the box as advertised. Some were very difficult to install, some would crash when running, some would generate something, but not enough to really get going, and the documentation across the various options was all over the spectrum. Then, I found [Hugo](http://gohugo.io/).

I used homebrew to install it, but since it's written in Go Lang, it is pretty simple to run. The other nice thing that it does for you is to generate a very basic structure, where it is very obvious where the content lives, and you're one command from running a dev server.

The following instructions are from the [Hugo Quickstart Guide](http://gohugo.io/overview/quickstart/).

To install with homebrew (on a Mac):

    brew install hugo

To create a new site:

    mkdir new_site
    cd new_site
    hugo new site .

To create your first page:

    hugo new about.md

The above post will live in `./content/about.md`, and will be accessible at `http://my_site.com/about`.

Or, to create a post in a subdirectory:

    hugo new post/first.md

The above post will live in `./content/post/first.md`, and will be accessible at `http://my_site.com/post/first`.

Install all the themes:

    git clone --recursive https://github.com/spf13/hugoThemes themes

Run the dev server:

    hugo server --theme=hyde --buildDrafts

Now, all of your static content has been generated in the `./public` directory. You can copy the URL printed out into your browser to view the site.

If all you want to do is rebuild the site with new content (to be published to production):

    hugo -t hyde

I don't like committing the `./public` directory to git, so I added the following lines to my `.gitignore`:

    public
    public/*

This way, on your production server, you can pull changes from the source, and run the hugo command to generate the new public content.

From there, you can play with trying out different themes, and once you pick a theme, hacking on the HTML/CSS in the theme to customize it to exactly what you want.
