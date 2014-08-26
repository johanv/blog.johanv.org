.. title: Migrating from Drupal 6 to Nikola
.. slug: drupal-nikola
.. date: 2014/08/26 20:55:31
.. tags: hpr,nikola,drupal
.. link: 
.. description: How to migrate from Drupal 6 to Nikola
.. type: text

Hello, this is johanv again for hacker public radio. I'm going to talk
about how I migrated my blog from Drupal 6 to Nikola.

First of all: why did I migrate? Mainly because I had some issues with my
Drupal blog. The way I want to write texts is not very compatible with the
way Drupal works.

I want to write texts when I'm offline, like e.g. on the train. I want to
use vim for writing text, because I really like that text editor. And I
want to use git to manage different revisons of my texts, because it often
takes a lot of editing before I am satisfied with the results.

I used to have a `git repository
<https://github.com/johanv/randomtexts>`__ to write my texts, and when I
was happy with a text, I copy-pasted it into the Drupal web interface.
Which is kind of clumsy.

So I saw the light when I heard `episode 1577 of HPR
<http://hackerpublicradio.org/eps.php?id=1577>`__, in which `guitarman
<http://stevebaer.com/>`__ talked about Nikola, a static website
generator. This was what I needed for my blog.

I migrated my blog from Drupal 6 to Nikola, and I will now tell you how I
did this; maybe this could be useful for othere HPR listeners.

First of all some details about the migration I did.

* I did not have node revisions on my Drupal site.
* My Drupal site had one 'vocabulary', which was used to assign tags to
  each post.
* I used pandoc 1.12.3.3 to convert the html of my nodes to
  RestructuredText, the format that nikola uses by default.
* I did not use page aliases, so every post I had, had an url like
  johanv.org/node/195.

First of all, I created a new subdomain for my blog: blog.johanv.org. I
used a script that creates a blog post for every published Drupal node on
my Drupal site.

This script was created with a lot of trial and error. It can probably use
some improvements, so I invite you to look at it, and to send me pull
requests.

It does the following for each node:

* get the timestamp, title and tags for the node.
* create a rst document with this metadata.
* do a lot of manipulations on the node content, and add the content to
  the rst document.

Those manipluations are the following

* convert html to restructured text (rst)
* fixe some line ending issues
* fix references to other blog posts
* fix blocks with literal code in the blog posts
* fix links to youtube

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

I also put a .htaccess file on my old blog, that redirects all requests
node/number to the correct page on the new blog.

With the combination of the script and the .htaccess file, 90% of the
migration was very easy. But - as always - the remaining 10% needs some
manual work. Like e.g. converting the youtube links containing
underscores. Those underscores were converted to ``\_``. Because there
weren't too many of them, I fixed those issues manually.

Another thing you should do manually, is migrate attachments and images to
your new site. Let's hope you don't have too many of them. And if so, you
can probably write a script as well.

OK, this was how I migrated my site from Drupal 6 to Nikola. I hope you
found it interesting. If you want to, you can comment on this HPR episode
on blog.johanv.org/drupal-nikola, using the disqus thingy over there. Or
you can find all of my website on github, and send me a pull request.

Finally I want to thank Hacker Public Radio. HPR introduced me to Nikola,
and HPR allows me to share how I did my migration. If you have something
interesting to tell, please submit an episode to HPR yourself. If I can do
this, you certainly can do this to.

This was johanv, signing off.

