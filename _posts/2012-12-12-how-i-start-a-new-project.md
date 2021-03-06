---
title: How I start a new project
keywords:
  - frontend
edits:
  - date: 2014/11/16
    md: This post is outdated. Note that I do not work like this anymore.
---

Hi people! I recently asked on Twitter a topic to blog about because I had frankly no idea. Christoph Rumpel ([@christophrumpel](https://twitter.com/christophrumpel)) proposed something about the way I start a new project. I thought it could be a good idea, plus I’m currently working on a new project at work so it may help me as well!

I’ll probably talk a lot about my blog in this article because it’s the latest project I ran, and I really tried to make things right. So I may do some parallelisms as examples. Anyway, let’s go!

## The thinking

The very first thing I do when starting a new web project is **thinking**. I spend like days boggling my mind about the way I should do this and this, and this… Jumping on the IDE to start coding isn’t a good idea. I used to do this in the past, and it often results in costly mistakes.

So, let’s think about it. And when I think I have a decent idea of what it’s going to look like, I’m sketching some sort of schedule. In most cases, this is as follow:

- **Write the objectives**. Who’s the target? What’s the point? What are the main features?
- **Find the content**. What do I have? What is my content, what will fit the blank pages?
- **Think about the arborescence**, the structure of the site / application.
- **Make a sketch**, then a mockup. I start large then I go to the details.
- **Develop**. Yaaay, let’s run the IDE and write some lines of code!

Things may vary depending on what’s the project, what’s its scale and such but in most cases this is the process I try to follow. I think it’s very important to make things in the right order; **the objectives define the content which defines the architecture, which defines the design, which defines the way to code**.

Please note this only my way of doing, do not take this as the Holy Bible. And please be sure to tell if you feel like something is wrong here. Okay so now we have some kind of way to go, let’s dig in each part a little deepier, shall we?

## The objectives

This is the very first thing you have to do. Actually, it should even be implicit since you need a goal in order to achieve something. When you eat, it’s because you’re hungry. When you open a new tab on your browser, it’s because you want to make a search or visit a website. If you want to make a website or an application, it’s to suit some needs, to fill a goal.

When I redesigned my website because the old one was just a single page with 5 lines of text on it, I tried to think about what would this site be for. Here is what I came with:

- Have a landing page for people looking for me for any reason
- Put my resume online
- Talk about things on what we can call a blog

Even if it’s implicit, it’s important to write things down. I often found myself in the past coding something I hadn’t think about at first, which resulted in refactoring large chunks of code. Writing things down from the beginning is a good way to know where you’re going.

## The content

Now we have thought about the goal of the site/application, it’s time to **think about the content** which will fill that goal. **There can be no site without content**, it’s lame and completely useless. Sometimes I see people asking for a website without anything to put in and I wonder what is the point? Having a place on the internet with your name on it? Okay actually that’s kind of cool, but that’s even better if you have things to tell.

My way to do is to find the maximum of content for each of the objectives of the site/application. Then, if there is too much, I filter to keep only the best ideas. Too much is better than too few. You might want to readnotes from Luke Wroblewski about Jeffrey Zeldman talk at AEA Washington DC about [Content first](https://www.lukew.com/ff/entry.asp?1598).

For this website, the content was pretty straight forward: the landing page should have some informations about me (like contact stuff). The resume should be, well, my resume. And I had already 2 ideas of articles for the new blog ([one about the redesign](redesign-blog.html), and one about [CSS preprocessors](less-to-sass.html)).

## The arborescence

So we managed to find some general content for our website/application. We can now think about the architecture of the whole thing. How will it be divided? In most cases, this step is pretty easy to do. You can think of it as “what my main navigation will look like?”.

Maybe it’ll have a contact page, a products page, a shop section and so on. Then, ask yourself if there will be another level of structure in a section. Shop may contain various other sections. All of this will give you a structure for your site.

This site’s structure is very easy:

- Home
- Blog
  - Articles
- Resume

Plus, it occured to me doing that helps me creating the development environment during when I come to the coding part.

## The design

This is when things are getting fun. Yeah cause the previous steps were pretty boring, right? Who want to spend hours thinking about something? WE WANT TO MAKE THINGS! So here it is. From now on, it’s like doing a bunch of stuff.

But there is still no code. We first have to **draw a sketch** of the website/application. What will it look like? Physically I mean. This is where I’m facing some troubles as far as I am concerned. If like me you never know how to start, then I figured out asking some very basic questions often help finding out where to start. Things like:

- What layout do I want (number of columns, structure, etc.)?
- What will be the [color scheme](https://tympanus.net/codrops/2012/09/17/build-a-color-scheme-the-fundamentals/) and how many colors will compose it?
- What do I want to put [above the fold](https://css-tricks.com/responsive-web-above-the-fold/) (main content, important stuff)?

**I like starting on paper**. I think it’s easier because you can’t hide behind visual effects like gradients, shadows and such. It’s all about structure and where will be positioned the various elements.

When I’m finally happy with a sketch, I try to redo it on my computer. Sometimes I go straight to CSS when things look quite simple, but it occured to me there are better ways to go. I think one of the best would be to use an online tool like [WireFrame.cc](https://wireframe.cc/) to make a nicer sketch than the one you drew on paper but still focused on structure, not visual effects.

![WireFrame.cc](/assets/images/how-i-start-a-new-project/wcc.jpg)

When it looks good, I personally go for CSS. Some people prefer working a detailed mockup on PhotoShop first but since I’m shitty as hell when it comes to this software and I’m quite confident with CSS, I prefer go straight with CSS and design in the browser. Plus, designing in the browser is very trend those days!

Although I know some designers simply can not design in the browser because they turn their brain to “technical mode”, losing their creative mind. I don’t blame at all since I’m not a designer, browser or not browser. Anyway, if you plan on using Photoshop, I’d suggest not doing too much on visual effects but it’s more like a matter of opinion here. I think this stuff can be figured out afterwards, and not necessarly during the design process but I could be wrong.

## The development

Yaaaaay! Here comes the code! The funny part. But before coding, I like to set up my workflow in order to make everything right for the whole development process. Setting up the working environment can mean a lot of things depending on the project:

- Building the arborescence (creating folders and such)
- Setting up the development environment (EasyPHP/Mamp, JDK, Sass & Compass, etc.)
- Creating some default files
- Whatever you think is good

Here is the way I go.

### Creating the project

I really like when things are well ordered plus I don’t want to keep messing with relative paths, so I like to make the whole arborescence at first to not bother about it anymore. Here is the way I do it:

- `sass/` for SCSS stylesheets if I use Sass
- `stylesheets/` for CSS stylesheets
- `scripts/` for JavaScript scripts
- `images/` for images
- `fonts/` for both web and icon fonts
- `feeds/` for RSS
- `resources/` for various stuff

This site also uses a `blog/` folder and a `resume/` folder to keep things clean.

A project involving some kind of back-office would also include a fake `admin/` folder (with a fake login form) and a real admin folder with some random name.

### Setting up the development environment

Depending on the project, the development environment may vary. A PHP project requires EasyPHP or Mamp to work, a Java-based project would require JDK, and so on. Even for the frontend side, you may want to have some tools like pre-processors (Markdown for HTML, Sass for CSS, CoffeeScript for JS or whatever).

Speaking of preprocessors, I build most of my projects on Sass so there are a few things I do before coding. I don’t know about you, but I don’t use any tool to manage Sass stuff (like Compass.app or CodeKit). Not because I’m a command-line nerd but because I work on different computers with different OS, meaning I would have to install everything all over again. So I tend to do Sass stuff with command lines through Ruby Command Prompt.

Depending on what I need for the project, I might also need some scripts like jQuery, Modernizr, Prefixfree, Prism or other things. I may also want to include an icon font I don’t know, so I download files before going any further, just in case.

### Creating default files

To speed up the development time, I like to do all the dirty stuff at the beginning of a project to be able to focus on the cool stuff without being interrupted. So I often create a bunch of files I know I will have to do.

#### HTML

The following code is a valid HTML document which can be used to create any page in the site. Its content may vary according to the needs of the project but in most cases it includes conditional HTML classes, IE stylesheet, commented scripts, jQuery call, Google Analytics snippet and so on. Everything’s ready!

```html
<!DOCTYPE html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="viewport" content="width=device-width" />
    <title>New title</title>
    <link rel="stylesheet" href="stylesheets/styles.min.css" />
    <!--[if lte IE 7]>
      <link rel="stylesheet" href="stylesheets/ie.css"
    /><![endif]-->
    <!--[if lt IE 9]>
      <script src="https://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
    <!-- <script src="scripts/modernizr-2.6.2.min.js"></script> -->
  </head>

  <body>
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js"></script>
    <script>
      window.jQuery ||
        document.write('<script src="js/jquery-1.8.2.min.js"><\/script>')
    </script>

    <!-- <script src="js/functions.js"></script> -->
    <!--
      <script>
      var _gaq=[['_setAccount','UA-XXXXX-X'],['_trackPageview']];
      (function(d,t){var g=d.createElement(t),s=d.getElementsByTagName(t)[0];
      g.src=('https:'==location.protocol?'//ssl':'//www')+'.google-analytics.com/ga.js';
      s.parentNode.insertBefore(g,s)}(document,'script'));
      </script>
    -->
  </body>
</html>
```

#### Stylesheets

What’s cool with Sass it’s it gives you the option to use a bunch of stylesheets and concatenate all of them into one single file without any performance loss. The following is my `styles.scss` file.

```scss
@import 'mixins';
@import 'variables';
@import 'default';
@import 'reset';
@import 'typography';
@import 'grid';
@import 'font-awesome';
@import 'main';
```

When compiled, it takes all the following SCSS stylesheets and concatenate and compress all of them into one single `styles.min.css` file.

- `_variables.scss` contains things like colors, default margins and such
- `_default.scss` has stuff like border-box, clearfix and useful classes
- `_reset.scss` contains Eric Meyer’s CSS reset
- `_typography.scss` comes from Twitter Bootstrap and includes a bunch of typographic rules
- `_grid.scss` is the [1140px Grid](https://www.ramotion.com/agency/web-design/cssgrid/) stylesheet
- `_font-awesome.scss` is the FA stylesheet, now replaced by the CDN from Tim Pietrusky [WeLoveIconFonts](https://weloveiconfonts.com)
- `_main.scss` contains stuff relative to the current project, the actual CSS (empty at first)

#### JavaScript

Most projects require at least a little bit of JavaScript. Especially since I’m really not good at JavaScript, meaning I feel like I have to use jQuery in order to achieve anything. Anyway, I use to have a few things in the scripts/ folder to begin with:

- The last and minified version of jQuery in case Google’s CDN is unavailable (currently `jquery-1.8.2.min.js`)
- The last and minified version of Modernizr to do some feature detection for graceful degradation (currently `modernizr-2.6.2.min.js`)
- `functions.js` where I put my stuff to run various things

In the past, I used to use Prefix-free as well for CSS prefixing but I don’t anymore since Compass does it for me.

Anyway, once I’ve done all this stuff, I can go further into the actual development.

## Final words

I guess I’ve covered pretty much everything I’m dealing with when starting a new project. Some things may vary depending on the project, but here is the general process.

What about you? How do you deal with another project? What’s your workflow?
