---
layout: post
title:  "Bootstrap and Jekyll: a guide for the perplexed"
category: compsci
tags: web-dev, html, css, bootstrap, jekyll
summary: "A not so quick guide on setting up a bootstrap + Jekyll site."
repo: https://github.com/vvveracruz/jekyll-bootstrap-example-site
repo_name: Github repos
---
This tutorial will walk you through how to create and maintain a Jekyll site using Bootstrap. Some of this tutorial is an updated and revised version of [this blog post on experimentingwithcode.com][4], and the rest has been hacked together by me. I am not a web dev expert, so I _will_ explain everything in excruciating detail. If you are, this is going to be too much information. Sorry.

## 1. How to Jekyll 101

### Installation

If you've never used Jekyll before it might be worth creating a Jekyll site using their [step by step tutorial][2] to familiarise yourself with the format.

On macOS, if you've installed the developer tools that come with Xcode you should already have Ruby, RubyGems, gcc and g++ on your machine. You will need at least Ruby version 2.5.0 to work with Jekyll. Check your version `ruby -v` and update if necessary.

An installation guide for different OS is available [on the Jekyll site][3].

### New site

Now that we're all set up with Jekyll we can create a new site directly from the terminal. I usually like to keep these things in the `documents` folder:

```
cd documents
jekyll new test-site
```

If you now navigate to the `test-site` directory you will see something like this:

```
├── config.yml
├── _posts
|   ├── ...
├── 404.html
├── about.markdown
├── Gemfile
├── Gemfile.lock
└── index.markdown
```

> 🎉 This is a fully functional Jekyll site.

In fact, you don't need to use bootstrap at all. If you're just looking to host a nice easy blog this might just be enough for you. Of course if you'd followed [the tutorial I linked to earlier][2] you would know this...

### What does it look like? 👀

If you want to see what this looks like right now you can serve it. First, navigate to the root directory (`test-site` in my case), then run:

```
bundle install
bundle exec jekyll serve
```

The first command makes sure any dependencies stated in the Gemfile are taken care of. The second command will serve your site on `localhost:4000`. This is the url address you need to type into your browser to see the site.

This is all incidental though, because we're getting rid of all of it.

## 2. Stylesheets

The default jekyll site above has the terrible flaw of being built on a theme, which will interfere with our plan to bootstrap it. So the first thing we need to do is get rid of all references to this theme.

The theme in question is `minima` and was installed on your computer when you got Jekyll. In fact, there is [a very cool guide][5] on bringing all the theme files into the root domain so you can modify them to your liking.

We're going to go a few steps further though, and get rid of `minima` all together.

### The big delete

You'll need to get rid of the `_posts` folder and the default post that comes with it, `404.html` and both `.markdown` pages.

Next we need to update the `config.yml` and `Gemfile` to no longer depend on `minima`.

This is what your `config.yml` file should look like:

{% highlight yml %}
# Site settings
title: My Very Cool Site
email: me@myverycool.site
author: beep boop I'm a human
description: Welcome to the tutorial site.
url: myverycool.site

# Build settings
permalink: :title/
{% endhighlight %}

The `permalink` command is optional, if you're not sure what it does do not include it. Leave `url` blank if you don't know where you're going to be hosting this page yet.

This is what your `Gemfile` should look like:
{% highlight ruby %}
source "https://rubygems.org"

gem "jekyll"
gem "github-pages", group: :jekyll_plugins
{% endhighlight %}

If you're going to be hosting your page on [Github Pages][6] you **must** include the the second gem. Otherwise it doesn't really matter.

Now we need to update the bundle (rememer earlier I said something about Gemfiles making sure all your dependencies are up to date?). You can do this step even if you don't understand what's happening. Navigate to your site directory in the terminal and type `bundle update`. You should end up with a reassuring `bundle updated!` message. If you don't, double check that you have deleted and copied and pasted everything properly.

### Shiny new directories

You also need to add some directories for later use. This is what your site looks like now:

```
├── config.yml
├── Gemfile
└── Gemfile.lock
```

We want to have an `assets` directory where we can keep all our precious bootstrap and custom bits and bobs. We also need an Index, to like, have a website.

A good top tip with these directory visualisations is that if it has a `.something` extension at the end it's a file, if it doesn't, it's a directory (or folder, they're the same thing.)

Now create files and directories until your site looks like this:

```
├── config.yml
├── Gemfile
├── Gemfile.lock

👇    new stuff    👇

├── assets
│   ├── css
│   │   ├── bootstrap
│   │   ├── custom
│   │   └── main.scss
│   └── js
└── index.html
```

We'll also start with a very basic Index file. You can just copy and paste this into it. (PS this is totally a slightly modified version of [the beginner example on the bootstrap website.][7])

```html
{% raw %}
<!doctype html>
<html lang="en">
  <head>

    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1 class="text-primary">Hello, world!</h1>


  </body>
</html>
{% endraw %}
```

### Downloading bootstrap source files

> 🔴 It is very important that you donwload **source files** and not compiled files. [This link will take you to the right section of getboostrap.com](https://getbootstrap.com/docs/4.5/getting-started/download/#source-files). The heading should say **Source Files** not compiled whatever. You have been warned.

Now, these come in a nice little `.zip` file. When you open it, you will see a lot of things that might be confusing. We only care about the `scss/` directory (for now). You can delete all the others.

Open the directory, select all the files and click copy. Then go back to your site and paste them into `assets/css/boostrap`. Or drag and drop. Nice.

### Integration: boostrap framework side

Sadly, this isn't a section about calculus. Instead we are integrating the bootstrap stylesheets into our Jekyll website.

Now, Nick Riebeek from experimentingwithcode.com goes about this next step in a different way, so if you don't like how I'm doing it maybe [check that out][9].

In our new `css/boostrap` directory, there is a file called `bootstrap.scss`. This file basically imports all the other components of bootstrap. I like to use this as my `main.scss` file.

The first thing to do is to drag the file outside of `boostrap/` and into `css/`. Now you can open it, and it will look something like this:

{% highlight scss %}
/*!
 * Bootstrap v4.5.3 (https://getbootstrap.com/)
 * Copyright 2011-2020 The Bootstrap Authors
 * Copyright 2011-2020 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/main/LICENSE)
 */

@import "functions";
@import "variables";
...
@import "spinners";
@import "utilities";
@import "print";
{% endhighlight %}

What this is doing is importing all the other files in the `bootstrap/` folder so that they are ready to use. The problem now is, though, that the `bootstrap.scss` file is no longer in the `bootstrap/` directory, so it's looking for these files to be imported somewhere they are not. This has an easy fix, just edit it to have the correct file address:

{% highlight scss %}
@import "bootstrap/functions";
@import "bootstrap/variables";
...
@import "bootstrap/spinners";
@import "bootstrap/utilities";
@import "bootstrap/print";
{% endhighlight %}
> 💡 You need to edit **all** the imports.

This seems super annoying and pointless but it has the advantage that it clearly separates our custom code (we'll come to this) from the bootstrap framework, and they both come together in this one single file. You will see.

We need to make two more changes to this file before it's ready to take over the role of Main.

Firstly, Jekyll requires that the `main.scss` file has with [front matter][10]. All you need to do is add this to the **top** of your file:

{% highlight scss %}
---

---

/*!
 * Bootstrap v4.5.3 (https://getbootstrap.com/)
 * Copyright 2011-2020 The Bootstrap Authors
 * Copyright 2011-2020 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/main/LICENSE)
 */

@import ...
{% endhighlight %}

Now you can delete `main.scss` and rename `bootstrap.scss` to `main.scss`.

> 🔴 It is important this is a `.scss` file and not `.css`.

### Integration: Jekyll configuration side

Our site exists within the Jekyll framework so we need to make sure it too knows what's happening. This isn't completely transparent because Jekyll expects a `_sass` directory by default, we just want this to be `assets/css` instead. Update your config file to look like this:

{% highlight yml %}
# Site settings
title: My Very Cool Site
email: me@myverycool.site
author: beep boop I'm a human
description: Welcome to the tutorial site.
url: myverycool.site

# Build settings
permalink: :title/
sass:
  sass_dir: /assets/css/
{% endhighlight %}

The last step is to link our Index to our stylsheet so it can use it. Simply add
{% highlight html %}
<link rel="stylesheet" href="assets/css/main.css">
{% endhighlight %}
to the head of the Index file. (Just above the `<title>` tags will work.)

### Ok, but why?

You can `bundle exec jekyll serve` to see what the site looks like now, using the bootstrap defaults.

No one likes defaults though, so we can test out what kinds of changes we can make to this. You will se the "Hello there!" heading in `index.html` comes with a class: `text-primary`. This is one of the (many) classes defined by bootstrap. Now would be a good time to peruse all the files in the `bootstrap` folder to get a better idea of what sorts of things we can do with it.

If you open the `_variables.scss` file you can see all sorts of things are defined in it. A quick search for `primary` finds this:

{% highlight scss %}
$primary:       $blue !default;
$secondary:     $gray-600 !default;
$success:       ...
{% endhighlight %}

It seems like, if nothing else, we can change the colour of the heading text. So let's try this.

First create your own `_variables.scss` file but this time in the `custom/` directory. You can choose this [colour picker tool][11] to decide on a colour. Don't choose blue though – that's the default, and you won't see any change.

Open your variables file and simply copy the format we've just seen:

{% highlight scss %}
$primary: #997300;
{% endhighlight %}

That's all you need for that file.

We do need to make sure the boss (`main.scss`) knows about this change, so we need to include this file too. Open it up and import your variables:

{% highlight scss %}
@import "custom/variables";

@import "bootstrap/functions";
@import ... ;
@import "bootstrap/variables";
@import ...
{% endhighlight %}

**Do not** delete the `bootstrap/variables` file, since this is defining a bunch of other variables we haven't, and you'd lose them completely.

By inclduing your variables file **before** the boostrap one, you are overwriting the variables you have custom defined, but the rest of them will still be used.

If you serve your site once more you should see this colour change 🎉

## 3. Javascript

Some of bootstrap's framework depends on jQuery and Popper.js. There's [a very handy list of these dependencies][13] on the Bootstrap website. To make sure it all runs smoothly, it's worth importing them both now.

There's about ten thousand ways to go about this but I'm going to use CDNs. Although I'm interested in having the source code for the bootstrap framework for snooping around and playing with later I'm not too bothered about jQuery or Popper.js so CDNs are a good option.

Both these links can be found on the bootstrap website but you can just copy and paste this

{% highlight html %}
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
{% endhighlight %}

just above the `<\body>` tag in the Index. Copying and pasting from getboostrap.com instead of the jQuery and Popper.js directly has the added advantage of having `integrity` and `crossorigin` tags. This help verify that the javascript you are importing into your website hasn't been manipulated on the way there.

Using another CDN for the bootstrap Javascript itself is definitely an option to consider (there is a link to this on the bootstrap website) but since I like being able to tinker with these things, I'm going to import it into my `assets`.

To do this we need two files: `javascript.js` and `javascript.map.js`. You can find both of these in the `dist/js` directory. Copy and paste them into your `assets/js` directory, and then add this line **under** the jQuery and Popper CDNs. It is very important this is loaded **after** them.

{% highlight html %}
<script src="assets/js/bootstrap.js"></script>
{% endhighlight %}

> 💡 We don't need any of that integrity/crossover stuff since we own this file locally.

### Did it work?!

We now have all the different pieces of the puzzle all together but no way to tell whether it works or not. We're going to test this out by building [a dismissable alert][15]. Let's try adding the following code to our `index.html` file:

{% highlight html %}
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
    <span aria-hidden="true">&times;</span>
  </button>
</div>
{% endhighlight %}

You can put this just above the imported scripts. You should get something like this:

![Disappearing alarm gif](/assets/id/{{ page.id }}/img.gif)

Nice. There's a bunch other components like this you can test out with instructions [on their website][16].

## 4. Make it Jekyll

Now if you've used the Internet before you will be able to tell this site is not usable. There is nothing going on and it's not exactly clear how you make things happen. We must Jekyllise.

If you feel up to it, this would be a good time to snoop in on how sites you like are doing things. A good one to look through is the [default template minima][12].

In short though, we need some new directories. Jekyll's magic ✨ really shines when we have templates (layouts), and snippets (includes) we can reuse to make our posts. With this in mind create three new directories: `_layouts`, `_includes` and `_posts`. It's important you give them these specific names otherwise Jekyll will not be happy with you. Your root directory should look something like this:

```
├── config.yml
├── index.html
├── ...
├── assets
│   └── ...
├── _layouts
├── _includes
└── _posts
```

Now we can start [refractoring][17] our code into something we can build upon. First we can create a reusable snippet for everything we consistenly want to have in the `head` of our pages. These are things like the stylesheet information as well as some useful meta tags.

### Includes

Create a file called `head.html` in the `_includes` directory:
{% highlight html %}
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

<link rel="stylesheet" href="assets/css/main.css">
{% endhighlight %}

Now we can delete all this from the Index and replace it with some [liquid][18] code:

{% highlight html %}
{% raw %}
---
---
<!doctype html>
<html lang="en">
  <head>

    <!-- Liquid pointing to the head -->
    {% include head.html %}

    <title>Hello, world!</title>

  </head>
  <body>
    ...
  </body>
</html>
{% endraw %}
{% endhighlight %}

The dashes at the top of the code are necessary for Jekyll to know how to interpret it. Without them it wont work. These dashes and the stuff we write in between them (you will see) are called the Front Matter.

> You can call these files whatever you want, by the way. I'm sure people have tons of opinions on what the optimal solution is, but if you're going to be the one maintaining the site, chose something that makes sense to you.

Now do the same for the bootstrap scripts we included at the end of the body. Create an includes file called `foot-js.html`:

{% highlight html %}
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
<script src="assets/js/bootstrap.js"></script>
{% endhighlight %}
and make sure to include it in the same place we had the scripts originally. You should be able to serve this and see that literally nothing has changed. This is a good thing.

### Default layout

Next we are creating a default layout based on the Index file. If we stop and think we can see there are a few things all pages on our website will definitely have in common:
- a title
- some form of content
- `head.html`
- `foot-js.html`
- `head` and `body` tags
- ...

The first two of these are special because Jekyll has a way to reference them by default, using liquid: `{%raw%}{{ page.title }}{%endraw%}` and `{%raw%}{{ content }}{%endraw%}`. The content of a page is anything written below the front matter (outside the dashed lines), and the page title is defined _within_ the front matter, along with the template you want to use. For our index:

```
---
title: "Hello World!"
layout: default
---
```

Now we just need to copy the structure of the index over to the default template:

{% highlight html %} {% raw %}
<!doctype html>
<html lang="en">
  <head>
    {% include head.html %}

    <title>{{ page.title }}</title>
  </head>
  <body>

    <h1 class="text-primary">{{ page.title }}</h1>

    {{ content }}

    {% include foot-js.html %}

  </body>
</html>
{% endraw %}{% endhighlight %}

And the simplified index file now becomes:

{% highlight html%}
---
title: "Hello World!"
layout: default
---

<div class="alert alert-warning alert-dismissible fade show" role="alert">
<strong>Holy guacamole!</strong> You should check in on some of those fields below.
<button type="button" class="close" data-dismiss="alert" aria-label="Close">
  <span aria-hidden="true">&times;</span>
</button>
</div>
{% endhighlight %}

## 5. Where to next?

You can download the site we've built here [on my Github][19]. The next few steps would be to start creating posts and to figure out where to host your site. For the first, you can follow [the step by step tutorial on the Jekyll site][2]. For the second, I cannot recommend [Github Pages][6] enough.

I might make posts on these in the future, so [follow me on Twitter][20] if you're interested 😉

---

## All links
- [CDNs – Wikpedia][1]
- [How to jekyll step by step][2]
- [Jekyll installation guide][3]
- [Reference tutorial on experimentingwithcode.com][4]
- [Jekyll: converting a gem theme into a regular theme][5]
- [Github Pages][6]
- [Bootstrap index starter template][7]
- [Download bootstrap source files][8]
- [Slightly different way of integrating bootstrap stylesheets][9]
- [Jekyll: what is front matter?][10]
- [Colour picker for html][11]
- [Minima theme on Github][12]
- [Bootstrap dependencies list][13]
- [jQuery downloads][14]
- [Making alerts with bootstrap][15]
- [Bootstrap instructions for different types of components][16]
- [Refractoring – Wikipedia][17]
- [Jekyll: liquid][18]
- [Sample site on my Github][19]
- [Link to my Twitter][20]

[1]: https://en.wikipedia.org/wiki/Content_delivery_network "CDNs -- Wikipedia"
[2]: https://jekyllrb.com/docs/step-by-step/01-setup/ "How to Jekyll step by step"
[3]: https://jekyllrb.com/docs/installation/ "Jekyll installation guide"
[4]: https://experimentingwithcode.com/creating-a-jekyll-blog-with-bootstrap-4-and-sass-part-1/ "experimentingwithcode.com tutorial"
[5]: https://jekyllrb.com/docs/themes/#converting-gem-based-themes-to-regular-themes "Converting a gem theme into a regular theme"
[6]: https://pages.github.com/ "Github Pages site"
[7]: https://getbootstrap.com/docs/4.5/getting-started/introduction/#starter-template "Bootstrap starter template"
[8]: https://getbootstrap.com/docs/4.5/getting-started/download/#source-files "Download source files"
[9]: https://experimentingwithcode.com/creating-a-jekyll-blog-with-bootstrap-4-and-sass-part-1/#integrating-bootstrap "Alternative integrating bootstrap"
[10]: https://jekyllrb.com/docs/front-matter/ "Jekyll front matter"
[11]: https://www.w3schools.com/colors/colors_picker.asp "Colour picker"
[12]: https://github.com/jekyll/minima "Minima on Github"
[13]: https://getbootstrap.com/docs/4.5/getting-started/introduction/#components "Bootstrap dependencies list"
[14]: https://jquery.com/download/ "jQuery downloads"
[15]: https://getbootstrap.com/docs/4.5/components/alerts/ "Alert tutorial on getbootstrap.com"
[16]: https://getbootstrap.com/docs/4.5/components/ "Bootstrap instructions for components"
[17]: https://en.wikipedia.org/wiki/Code_refactoring "Refractoring -- Wikipedia"
[18]: https://jekyllrb.com/docs/step-by-step/02-liquid/ "Jekyll – liquid"
[19]: https://github.com/vvveracruz/jekyll-bootstrap-example-site "Sample site on Github"
[20]: https://twitter.com/vvveracruz "My Twitter account"
