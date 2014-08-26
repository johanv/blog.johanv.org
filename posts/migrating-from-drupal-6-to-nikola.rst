.. title: Migrating from Drupal 6 to Nikola
.. slug: drupal-nikola
.. date: 2014/08/26 20:55:31
.. tags: hpr,nikola,drupal
.. link: 
.. description: How to migrate from Drupal 6 to Nikola
.. type: text

Hello, this is johanv again for Hacker Public Radio. I'm going to talk
about how I migrated my blog from Drupal 6 to `Nikola
<http://getnikola.com>`__. You might have heard about Nikola on
`episode 1577 of HPR <http://hackerpublicradio.org/eps.php?id=1577>`__, it
is a static web site generator.

First of all: why did I migrate? Mainly because I had some issues with my
Drupal blog. The way I want to write texts is not very compatible with the
way Drupal works.

I want to write texts when I'm offline, like e.g. on the train. I want to
use vim for writing, because I really like that editor. And I
want to use git to manage different revisons of what I write, because it often
takes a lot of editing before I am satisfied.

I used to have a `git repository
<https://github.com/johanv/randomtexts>`__ to write my blog posts, and when I
was happy with a text, I copy-pasted it into the Drupal web interface.
Which is kind of tedious.

So I saw the light when I heard HPR 1577, in which `guitarman
<http://stevebaer.com/>`__ talked about Nikola. He convinced me that I
needed to use a static website generator for my blog.

I migrated my blog from Drupal 6 to Nikola, and I will now tell you how I
did this; maybe this is of interest for othere HPR listeners.

First of all some details about the Drupal site I migrated from.

* I did not have node revisions on my Drupal site.
* My Drupal site had one 'vocabulary', which was used to assign tags to
  each post.
* I did not use page aliases, so every post I had, had an url like
  johanv.org/node/195.

Also worth mentioning is that I used pandoc 1.12.3.3 to convert the html
of my nodes to RestructuredText, the format that Nikola uses by default.
If you have another version of pandoc installed, you will possibly need to
tweak the script I used.

First of all, I created a new subdomain for my blog: blog.johanv.org. I
used a script that creates a blog post for every published Drupal node on
my Drupal site.

This script was created with a lot of trial and error. It can probably use
some improvements, so I invite you to look at it, and to send me pull
requests.

The script queries the database of the Drupal instance, to do the
following for each node:

* get the timestamp, title and tags for the node.
* create a rst document with this metadata.
* do a lot of manipulations on the node content, and add the content to
  the rst document.

Those manipluations are the following

* conversion from html to restructured text (rst)
* fixing line ending issues
* conversion of references to other blog posts
* handling issues with blocks with literal code in the blog posts
* conversion of links to youtube videos

I won't read out the whole script, that wouldn't make interesting radio,
but I will put a link in the shownotes. It's an ugly script: you'll have
to edit the first lines, describing how you can connect to the database
(put your credentials in my.cnf), and where the output files should go.
(By default they go in /tmp/out.)

You probably have to tweak the script to adapt it to your needs, but hey,
you have a starting point.

The script converts each node e.g. johanv.org/node/195 to a blog post
blog.johanv.org/posts/node-195.html. This way I could easily convert
hyperlinks to other posts to the corresponding html page of the new blog.

On the location of my old blog, I put an .htaccess file, that redirects
all requests /node/number to the correct page on the new blog.

With the combination of the script and the .htaccess file, 90% of the
migration was very easy. But - as always - the remaining 10% needs some
manual work. Like e.g. converting the youtube links containing
underscores. Those underscores were prefixed with a backslash, which
wasn't correct. Because there weren't too many of those errors, I fixed
them manually.

Another thing you should do manually, is migrating attachments and images to
your new site. Let's hope you don't have too many of them. And if so, you
can probably write a script as well.

OK, this was how I migrated my site from Drupal 6 to Nikola. I hope you
found it interesting. If you want to, you can comment on this HPR episode
on http://blog.johanv.org/drupal-nikola, using the disqus thingy over there. Or
you can find `all of my website on github
<https://github.org/johanv/blog.johanv.org>`__, and send me a pull request.

Finally I want to thank Hacker Public Radio. HPR introduced me to Nikola,
and HPR allows me to share how I did my migration. If you have something
interesting to tell, please submit an episode to HPR yourself. If I can do
this, you certainly can do this to.

This was johanv, signing off.

