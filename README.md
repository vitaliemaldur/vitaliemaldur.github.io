<h1 align="center">
    <img alt="vitalie.xyz" title="vitalie.xyz" src="https://github.com/vitaliemaldur/vitalie.xyz/blob/master/static/avatar.png" width="140"> </br>
    vitalie.xyz
</h1>

This is a copy of [Lumen Gatsby starter](https://github.com/alxshelepenok/gatsby-starter-lumen) used for [vitalie.xyz](https://vitalie.xyz) webiste.

## Local setup
```sh
yarn install
yarn develop # -> http://localhost:8000
```

## Deployment
To deploy the changes to [vitalie.xyz](https://vitalie.xyz) webiste, just push the changes to the `master` branch and the `deploy` action configured will take care of the rest.

## Adding new posts or pages
To add a new post, add a new `markdown` file to the folder `/content/posts`. Frontmatter fields to be considered are:
```
title: Title of the post
date: "2016-09-01T23:46:37.121Z"
template: "post"
draft: false
slug: "some-slug"
category: "Category 1"
tags:
  - "tag1"
  - "tag2"
description: "some description that will appear on the index page"
socialImage: "/media/image-2.jpg"
```

To add a new page, add a new `markdown` file to the folder `/content/pages`. Frontmatter fields to be considered are:
```
title: "Page title"
template: "page"
socialImage: "/media/image-4.jpg"
```
