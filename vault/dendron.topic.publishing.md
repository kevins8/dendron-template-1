---
id: 73d395c9-5041-4d0d-9db7-080d9586136e
title: Publishing
desc: ''
updated: 1595170096361
created: 1595170096361
---

# Publishing 🚧

Dendron lets you publish the contents of your vault, either in its entirety or only a subset. Notes are published under the [dendron-jekyll theme](https://github.com/dendronhq/dendron-jekyll). 

## Features

### Nested Hierarchies

dendron-jekyll supports the same nested hierarchies as your notes and allows you to navigate via the sidebar.

<img style="max-width: 720px;" src="https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/site-hierarchy.gif" />

### Lookup

If you'd rather not click, dendron-jekyll also supports path based lookup.

<img style="max-width: 720px;" src="https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/site-lookup.gif" />

### Permanent Ids

Every page is published using its unique ID which means that urls will never change, even if the filenames do.

<img style="max-width: 720px;" src="https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/site-ids.jpg" />

### Free hosting, custom domain names and SSL Certs

If you have a free github account, then you can host your Dendron notes for free using [github pages](https://pages.github.com/) 

<img style="max-width: 720px;" src="https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/site-domain.jpg" />

### Jekyll Liquid Tags and Variables 

You can find the docs on this under [[liquid| dendron.topic.liquid]]
- NOTE: this will only be "compiled" in the published site but won't be rendered in the regular markdown 


<img style="max-width: 720px;" src="https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/site-liquid.gif" />

### Selective Publication

You can choose to publish your whole vault, a single domain, or multiple domains within your vault.

- ~~NOTE: It is not currently possible to publish multiple domains. If you would like this feature, you can vote for it [here](https://github.com/dendronhq/dendron/issues/64)~~

## dendron.yml

This config file controls what gets published. It is located at the root of your workspace. Below is the config that is used to publish `dendron.so` from the contents of this [repo](https://github.com/dendronhq/dendron-template)


```yml
site:
  siteHierarchies: [dendron]
  siteRootDir: docs
```

## Properties

### siteHierarchies: str[]

List of hierarchies to publish

### siteIndex?: str
- optional, path of your index (home page)
- defaults to the first element of `siteHierarchies`

### siteRootDir: str

Location of the directory where site will be build. Relative to your workspace

### config

Per hierarchy specific config. 

```yml
config:
  {hierarchy name}: {hierarchy options}
```

The list of possible options:
- publishByDefault: boolean, default: true
  - if set to false, dendron will only publish notes within the hierarchy that have `published: true` set in the frontmatter

As an example, below is the config for [my website](https://kevinslin.com). It publishes everything under the `home` and `blog` hierarchies but will only publish notes under `books` if `published: true` is set on the frontmatter. 

- dendron.yml
```yml
site:
  siteHierarchies: [home, blog, books]
  siteRootDir: docs
  config:
    books:
      publishByDefault: false
```

### Example publishing entire vault
- vault
```
.
├── root.md
├── dendron.md
├── dendron.quickstart.md
├── dendron.zen.md
├── flowers.md
└── flowers.bud.md
```
- dendron.yml
```yml
publish:
  siteHierarchies: [root]
```
- what gets published

```
.
└── root.md
    ├── dendron
    │   ├── dendron.quickstart
    │   └── dendron.zen
    └── flowers
        └── flowers.bud
```

### Example publishing just one domain
- vault
```
.
├── root.md
├── dendron.md
├── dendron.quickstart.md
└── dendron.zen.md
```
- dendron.yml
```yml
publish:
  siteHierarchies: [dendron]
```
- published:

```
.
└── dendron
    ├── dendron.quickstart
    └── dendron.zen
```

## Steps

### Pre-requisites
- Make sure you have a `docs` folder inside `dendron.rootDir`.
    - If you've created your workspace using `Dendron: Initialize Workspace` on 0.4.2 or later, this is already done 
    - If you've created your workspace before `0.4.2` or have manually created your workspace, you can run `Dendron: Doctor` to create this folder
/pages/)
- Setup a github repo with [github pages enabled](https://guides.github.com/features/pages/)
  - when selecting the source, make sure to pick `master branch/docs folder`
![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/gh-page-docs.jpg)
- Create an initial commit and push your vault to your github page

### Guide
1. Depending on what notes you want to publish, you will want to update your `dendron.yml` file. By default, everything will be published
1. Update the contents of `_config.yml` inside your `docs` folder to specify site wide configuration like logo and title.
    - NOTE: this is located relative to `dendron.rootDir`
    - if you do not have a `docs` folder, you can run `Dev:Dendron: Doctor` to create one
2. When you are ready to publish, run the `> Dendron: Build Pod` command to prepare your site for publication. This builds your notes into the `siteRootDir` directory which defaults to `docs`. `Build Pod` does the following when it builds your site:
    - copies over the notes specified in `dendron.yml`
    - creates a copy of your notes with the ids in place of the file names 
    - updates all `wiki-links` to `markdown-links` so Jekyll can process it, preserving titles if there are any
3. Commit your changes.
4. Push your repository using `> Git: Push`

<a href="https://www.youtube.com/watch?v=VOZJxKg0-js">![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/dendron-publishing.jpg)</a>

## Advanced

### Exclude from publication

To exclude a page from publication, you can add the following to the frontmatter. If you set `publishByDefault: false` for a hierarchy, this needs to be set to `true` to publish

```yml
...
published: false
```

### Exclude line from publication

Sometimes, you just want to keep a few lines private while publishing the rest of your vault. You can do that with `Local only`. In order to mark a line as `Local Only`, add the following markdown comment at the end of the line: `<!--LOCAL_ONLY_LINE-->`

```markdown
Hello World!  <!-- Will be published -->

This is a secret <!--LOCAL_ONLY_LINE--> <!-- won't be published -->
```

![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/pod-local.gif)