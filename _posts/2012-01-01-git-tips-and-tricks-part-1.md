---
title: "Git tips & tricks - Part 1"
layout: post
guest: "Loïc Giraudel"
comments: false
preview: true
---
<section>
<p class="explanation">Hey guys! The following is the first post of a series written by my dear brother Loïc, Git expert at Best Of Media (Grenoble, France). I'll release the next parts in the next few weeks, so be sure to stay tuned for more Git awesomeness!</p>

Hi guys! Today, I'm gonna share with you some Git tips & tricks I've already shared with my workmates at Best Of Media. But before even starting let's never forget the more important laws of Git.

**Law #1**: **each commit must let the branch into a stable state**.
You must be able to checkout any commit in the project and still have a working application to play with. A functionality shouldn't be splitted into several commits. For example, don't put the HTML, CSS and JS of a new functionality in three commits: the functionality requires all of this to works so put all this stuff in a single commit. If you have to stop your work (time to go home, switch to another thing, etc.), create a temporary commit which will be enhanced later.

**Law #2**: **each commit has only one purpose**.
If you see a bug while you're working on a new functionality, try to fix this bug in a separate commit to be able to revert (or cherry-pick) one of both commit if needed.

<blockquote class="pull-quote--right">Each commit should leave the branch stable.</blockquote>

Ok, now let's start with the real tips & tricks...
</section>
<section id="custom-prompt">
## Use an awesome prompt [#](#custom-prompt)

If you have to often switch from one branch to another (like a Git monkey), having a great prompt is quite useful to know what is the current branch you're working on, if you have modified some files, if you have some commits to push or to pull from the server, and so on.

My favorite so far has been created by [Tung Nguyen](https://gist.github.com/tung) and you can found it [right here](https://gist.github.com/tung/409780).

This prompt displays:
* username@host
* your current path
* the current branch
* a lightning if you have some modified files in your working directory or in your staging area, waiting to be packed in a commit
* an up, down or up-and-down arrow if you have commits to push or pull from remote repository

![Example of git promp](/images/git-tips-and-tricks-part-1__git-prompt.png)

In this image, I'm working on the "myFork" branch and I have modified and/or staged some files but I don't have any commit to push or to pull.

To install this prompt in a linux environment, just download it somewhere and update your ~/.bashrc file to add this line:

<pre class="language-bash"><code>. /path/to/gitprompt</code></pre>

That's it. Just re-open your terminal and go to a Git project directory.
</section>
<section id="find-a-commit">
## Find a commit [#](#find-a-commit)

This is the very basic when working with Git.

<blockquote class="quote">How am I supposed to find a specific commit relative to a specific part of code?</blockquote>

There are many ways to do this.

<blockquote class="pull-quote--right">Each commit has only one purpose.</blockquote>

### With `git log -p`

The simplest is to use `git log`. If you add `-p` (or `-u` or `--patch`), you will have the modifier code of each commit, there's nothing for it but to search in the output to find a specific string.

### With `git log -S`

A better method is to use the `-S` parameter to search for a specific string: `git log -S console.log` will search all commit which contains the string "console.log" in the patch content. It's better than the previous method because it doesn't search in the commit message or information (username, date, ...) and it's only searching in the patch content and not in the lines added before and after the patch content.

You can add several parameters to reduce the commits related to the search:

<pre class="language-bash"><code>git log -S console.log --author lgiraudel --before="2013-10-01 00:00" --after="2013-06-01 00:00" -- web/js</code></pre>

### With `git blame`

`git blame` displays each line of a file and the last commit which has modified the line. It's the better way to find who, when and why a specific line has been added to a file.

It requires a filepath to works:

<pre class="language-bash"><code>$ git blame Gruntfile.js
15b95608 (Loic 2013-10-08 14:21:51 +0200  1) module.exports = function(grunt) {
15b95608 (Loic 2013-10-08 14:21:51 +0200  2)
15b95608 (Loic 2013-10-08 14:21:51 +0200  3)   // Project configuration.
15b95608 (Loic 2013-10-08 14:21:51 +0200  4)   var gruntConfig = {
15b95608 (Loic 2013-10-08 14:21:51 +0200  5)     pkg: grunt.file.readJSON('package.json'),
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 42)     jshint: {
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 43)       all: [
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 44)         'Gruntfile.js',
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 45)         '/web/js/**/*.js'
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 46)       ],
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 47)       options: {
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 48)         jshintrc: '.jshintrc'
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 49)       }
0a1d4d7b (Alex 2013-10-25 18:15:36 +0200 50)     },
15b95608 (Seb  2013-10-08 14:21:51 +0200  6)     jasmine: {
df9b1c21 (Seb  2013-10-11 11:50:08 +0200  7)       src: 'web/js/**/*.js',
df9b1c21 (Seb  2013-10-11 11:50:08 +0200  8)       options: {
df9b1c21 (Seb  2013-10-11 11:50:08 +0200  9)         vendor: [
[...]</code></pre>

It's possible to limit the output to specific lines with the parameter `-L`: `git blame -L 10,20` will only output lines 10 to 20.
</section>
<section id="improve-diff-output">
## Improve diff output [#](#improve-diff-output)

`git diff` is one of the most used git command before adding changes to the stage areas to avoid pushing mistakes to repository. The diff command can be customized to avoid some inconvenience.

### See changes in one line

In the diff output, each change is displayed like this :

<pre class="language-bash"><code>$ git diff
diff --git a/Gruntfile.js b/Gruntfile.js
index 74d58f9..569449c 100755
--- a/Gruntfile.js
+++ b/Gruntfile.js
@@ -41,7 +41,7 @@ module.exports = function(grunt) {
     },
     jshint: {
       all: [
-        'Gruntfile.js',</span>
+        'gruntfile.js',</span>
         '/web/js/**/*.js'
       ],
       options: {</code></pre>

But if you use the `--color-words` parameter, it will write the old and new text in the same line with red and green colors, which can be more readable in some cases.

### Disable space changes

When spaces are added in a line, the `git diff` command displays the line as changed. When you read your changes before creating a commit, this can be annoying to understand the diff, especially when the spaces have been added/removed by your IDE (useless spaces, fix indentation, replace tab by spaces, etc).

To avoid this pollution in the git diff, you can add the `-w` option to omits spaces (and tabs) changes.

Let's take an explicite example:

<pre class="language-bash"><code>$ git diff
diff --git a/web/js/testedJs/lazy.js b/web/js/testedJs/lazy.js
index b2185a2..887387f 100755
--- a/web/js/lazy.js
+++ b/web/js/lazy.js
@@ -427,28 +427,30 @@
         return;
       }
-      if (url !== null && url !== '' && typeof url !== 'undefined') {
-        jQuery.ajax({
-          url: url,
-          type: 'GET',
-          context: this,
-          dataType: $elem.data('tmpl') !== undefined ? 'json' : 'html',
-          success: self.injectData,
-          error: oConf.onError || self.error,
-          beforeSend: function () {
-            $elem.addClass('loading');
-            if (oConf.onLoad && 'function' === typeof oConf.onLoad) {
-              oConf.onLoad();
-            }
-          },
-          complete: function () {
-            $elem.removeClass('loading');
-            if (oConf.onComplete && 'function' === typeof oConf.onComplete) {
-              oConf.onComplete();
-            }
-          }
-        });
+      if (url === null || url === '' || typeof url === 'undefined') {
+        return;
       }
+
+      jQuery.ajax({
+        url: url,
+        type: 'GET',
+        context: this,
+        dataType: $elem.data('tmpl') !== undefined ? 'json' : 'html',
+        success: self.injectData,
+        error: oConf.onError || self.error,
+        beforeSend: function () {
+          $elem.addClass('loading');
+          if (oConf.onLoad && 'function' === typeof oConf.onLoad) {
+            oConf.onLoad();
+          }
+        },
+        complete: function () {
+          $elem.removeClass('loading');
+          if (oConf.onComplete && 'function' === typeof oConf.onComplete) {
+            oConf.onComplete();
+          }
+        }
+      });
     };
     /**</code></pre>

What's the important change in this piece of code? It's not quite easy to check what have been done with a diff like this. But with the `-w` option:

<pre class="language-bash"><code>$ git diff -w
diff --git a/web/js/testedJs/lazy.js b/web/js/testedJs/lazy.js
index b2185a2..887387f 100755
--- a/web/js/lazy.js
+++ b/web/js/lazy.js
@@ -427,7 +427,10 @@
         return;
       }
-      if (url !== null && url !== '' && typeof url !== 'undefined') {
+      if (url === null || url === '' || typeof url === 'undefined') {
+        return;
+      }
+
         jQuery.ajax({
           url: url,
           type: 'GET',
@@ -448,7 +451,6 @@
             }
           }
         });
-      }
     };
     /**</code></pre>

Now, this is really understandableé: I've replaced the test wrapping my ajax call by a 3-lines test checking just before, which reduces the indentation level of the ajax call.
</section>
<section id="final-thoughts">
## Final thoughts [#](#final-thoughts)

I hope those little tricks will help. In the next part, I'll continue with other small smart tricks before tackling some advanced Fit useful features.

<blockquote class="quote">
<img src="http://m.c.lnkd.licdn.com/mpr/pub/image-BuKiTUmt49Y4eE_bkOrrlmdwEAiHjpWWXurflnl3E-MLjUHUBuKfzCKtEGCxjpCb0ioX/loic-giraudel.jpg" alt="Loic Giraudel" class="pull-image--left">
<p>Loïc Giraudel works as a front-end developer at BestOfMedia (Grenoble, France). He is a JavaScript and Git expert. You can catch him on Twitter: <a href="http://twitter.com/l_giraudel">@l_giraudel</a>.</p>
</blockquote>
</section>