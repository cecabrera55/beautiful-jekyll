# Camilo Cabrera

> *Copyright 2016 [Dean Attali](http://deanattali.com). Licensed under the MIT license.*

As mentioned previously, you can use [prose.io](http://prose.io/) to add or edit files instead of doing it directly on GitHub, it can be a little easier that way.

#### Page types

- **post** - To write a blog post, add a markdown or HTML file in the `_posts` folder. As long as you give it YAML front matter (the two lines of three dashes), it will automatically be rendered like a blog post. Look at the existing blog post files to see examples of how to use YAML parameters in blog posts.
- **page** - Any page outside the `_posts` folder that uses YAML front matter will have a very similar style to blog posts.
- **minimal** - If you want to create a page with minimal styling (ie. without the bulky navigation bar and footer), assign `layout: minimal` to the YAML front matter.
- If you want to completely bypass the template engine and just write your own HTML page, simply omit the YAML front matter. Only do this if you know how to write HTML!

#### YAML front matter parameters

These are the main parameters you can place inside a page's YAML front matter that **Beautiful Jekyll** supports.

Parameter   | Description
----------- | -----------
title       | Page or blog post title
subtitle    | Short description of page or blog post that goes under the title
bigimg      | Include a large full-width image at the top of the page.  You can either give the path to a single image, or provide a list of images to cycle through (see [my personal website](http://deanattali.com/) as an example).
comments    | If you want do add Disqus comments to a specific page, use `comments: true`. Comments are automatically enabled on blog posts; to turn comments off for a specific post, use `comments: false`. Comments only work if you set your Disqus id in the `_config.yml` file.
show-avatar | If you have an avatar configured in the `_config.yml` but you want to turn it off on a specific page, use `show-avatar: false`. If you want to turn it off by default, locate the line `show-avatar: true` in the file `_config.yml` and change the `true` to `false`; then you can selectively turn it on in specific pages using `show-avatar: true`.
share-img   | If you want to specify an image to use when sharing the page on Facebook or Twitter, then provide the image's full URL here.
social-share | If you don't want to show buttons to share a blog post on social media, use `social-share: false` (this feature is turned on by default).
layout      | What type of page this is (default is `blog` for blog posts and `page` for other pages. You can use `minimal` if you don't want a header and footer)  
js          | List of local JavaScript files to include in the page (eg. `/js/mypage.js`)
ext-js      | List of external JavaScript files to include in the page (eg. `//cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.2/underscore-min.js`)
css         | List of local CSS files to include in the page
ex-css      | List of external CSS files to include in the page
googlefonts | List of Google fonts to include in the page (eg. `["Monoton", "Lobster"]`)

#### Project/company websites

| Website | Description |
| :------ |:----------- |
| [teampass.net](http://teampass.net) | Collaborative Passwords Manager |
| [derekogle.com/fishR](http://derekogle.com/fishR/) | Using R for Fisheries Analyses |
| [bigdata.juju.solutions](http://bigdata.juju.solutions) | Creating Big Data solutions Juju Solutions |
| [joecks.github.io/clipboard-actions](http://joecks.github.io/clipboard-actions/) | Clipboard Actions - an Android app |
| [embedded.guide](http://embedded.guide) | Writing an Embedded OS |
| [blabel.github.io](http://blabel.github.io) | Library for canonicalising blank node labels in RDF graphs |
| [reactionic.github.io](http://reactionic.github.io) | Create iOS and Android apps with React and Ionic |
| [ja2-stracciatella.github.io](http://ja2-stracciatella.github.io) | Jagged Alliance 2 Stracciatella |
| [PatientOutcomeFunding.org](http://www.patientoutcomefunding.org/) | Patient Outcome Funding |

#### Personal websites

| Website | Who | What |
| :------ |:--- | :--- |
| [deanattali.com](http://deanattali.com) | Dean Attali | Creator of Beautiful Jekyll |
| [ouzor.github.io](http://ouzor.github.io) | Juuso Parkkinen | Data scientist |
| [derekogle.com](http://derekogle.com/) | Derek Ogle | Professor of Mathematical Sciences and Natural Resources |
| [trappmartin.github.io](http://trappmartin.github.io) | Martin Trapp | Machine learning researcher |
| [melyanna.github.io](http://melyanna.github.io/) | Melyanna | Shows off her nice art |
| [chauff.github.io](http://chauff.github.io/) | Claudia Hauff | Professor at Delft University of Technology |
| [kootenpv.github.io](http://kootenpv.github.io/) | Pascal van Kooten | Data analytics |
| [sjackman.ca](http://sjackman.ca) | Shaun Jackman | PhD candidate in bioinformatics |
| [epwalsh.fyi](https://epwalsh.fyi) | Evan Pete Walsh | PhD candidate (Statistics and Mathematics) at Iowa State University |

### Very advanced: Local development

Beautiful Jekyll is meant to be so simple to use that you can do it all within the browser. However, if you'd like to develop locally on your own machine, that's possible too if you're comfortable with command line. Follow these simple steps to do that with Vagrant:

1. Install [VirtualBox](http://virtualbox.org) and [Vagrant](https://www.vagrantup.com)
2. Clone your fork `git clone git@github.com:yourusername/yourusername.github.io.git`
3. Inside your repository folder, run `vagrant up`
4. View your website at `http://0.0.0.0:4000` on *nix or `http://127.0.0.1:4000` on Windows. 
5. Commit any changes and push everything to the master branch of your GitHub repository. GitHub Pages will then rebuild and serve your website automatically.
