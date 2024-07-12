## Finally I installed my github pages and blog today

I had experimented with githup pages a bit since some time ago,
but today I decided to do it finally, instead (or, alas, after)
starting yet another blog on blogger... I never go there and
forget about the blogs I have started there (pepmath.blogspot.com
and til-this.blogspot.com) and elsewhere (make the list ...)

I first created my m-f-h.github.io repo from scratch and put some content there,
mainly to experiment with layout, responsive sidebars, etc.

But then I figured it would be tiresome to re-install the jekyll stuff
as I already had done for another project (our journal JNEEA).
So I googled (resp. braved) and found some "template" for a github + Jekyll
powered blog, which I decided to give a try. (It had enough features to inspire
me for the first "extensions" I will do, yet not too much as the earlier
try I had given with a nice looking, but somewhat bloated Jekyll theme.)

### Some remarks (for my own records...)

I'll try to write down what I understand (possibly incorrectly) about 
the structure of this site.

#### The basic Jekyll config file `/_config.yml`
In this file, we find:
```yml
title: "MFH's github pages and blog" #etc.
#(goes on with author, description, email, twittername etc.)

theme: "minimal" # is that even a theme? TODO: link to docs
defaults:
    - scope:
        path: ""
        type: post
      values:
        tags: Other
```
I think the  [/_config.yml](/_config.yml) file *could* define a default layout (TODO: check & link the docs), but doesn't, so far. (The only default it defines is the tag "Other" for "nodes" (what is the correct term? pages and posts...?) of type `post`)

#### page layout
The page layouts are defined in [/_layouts](/_layouts), but for the moment, `post.html` is  the only entry there.

These "layout" HTML files include some additional header stuff, but not the basic HTML header (`<head><title>...`) 

Essentially it starts with a `<header>` section which has `<h1>{{ page.title ...` inside.

Then it has two `<div>` sections,
* one with
`{% include sharelinks.html %}` (these are the social media links defined in `/_config.yml` and shown in the footer, I think),
* the other with `{{ content }} {% include navlinks.html %}` (the latter makes a navbar "[<- prev.post next post->]" below the contents.)
  
* These two include files are specified in `/_includes`. 
* *There* is another file `head.html` which contains the HTML `<HEAD>` (with several `<LINK>` sections etc., but nothing else.)

#### posts and pages
* The posts are the `.md` files in `/_posts`
* Where is that subdir specified?
* Where else does Jekyll look for other pages?
  * Obviously it does look for `.md` pages at the root.
  * Can we put static pages in the `/_posts` subdir?
  * Can we make arbitrary folders, and Jekyll will look everywhere? If we want to "hide" a file but not delete it, can/must we create a special folder and list it in `.gitignore`?
  
  
  [Is that a feature of `jekyll-sitemap` listed in `/_config.yml` ?])
* the static homepage (`/index.md`) has no YAML header at all. Does it also use the  `post.html` layout ?
* the "Blog archive" page (`/archive.md`) does have a YAML header that has `layout: default`. Does that mean that it should : 
  * use a `default.html` layout which however is not defined?
  * use a default layout that should be defined in `/config.yml` but isn't ?
  * use as layout the only one that is defined (or if it weren't the only one, then the alphabetically first one?)
  * the blog posts (`.md` files in `/_posts`) do use the `post.html` layout, even without a YML header. (How do they know?)

Some open questions/TODOs:

* I will have to create a second "layout" for static pages that are not part of the blog.
* In order that both, these pages and the blog posts, keep a uniform appearance, I guess I'll put the common stuff in a new `_includes` file

