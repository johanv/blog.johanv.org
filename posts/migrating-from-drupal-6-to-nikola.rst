.. title: Migrating from Drupal 6 to Nikola
.. slug: drupal-nikola
.. date: 2014/08/26 20:55:31
.. tags: hpr,nikola,drupal
.. link: 
.. description: How to migrate from Drupal 6 to Nikola
.. type: text

(I recorded an episode for Hacker Public Radio about this subject, which
will be 'on air' on 2014-09-30. `HPR1507 <http://hackerpublicradio.org/eps.php?id=1607>`__)

As promised, some details
about how I :doc:`migrated my blog <first-post>` from Drupal 6 to `Nikola
<http://getnikola.com>`__.

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

I created a new subdomain for my blog: blog.johanv.org. I
used a script that creates a blog post for every published Drupal node on
my Drupal site.

This script was created with a lot of trial and error. It can probably use
some improvements, so I invite you to look at it, and to send me pull
requests ::

  #!/bin/bash
  
  # migrate articles and stories from drupal 6 to nikola
  # Copyright 2014 Johan Vervloet
  # You can use and distribute this script under the terms of the
  # GNU General Public License version 3 or later.
  
  # Notes: 
  #  * my drupal site has no multiple revisions of posts.
  #  * I had one vocabulary that I used for tagging posts.
  #  * you need to have pandoc installed for this script to work.
  
  
  # please change the two variables below
  # according to your needs
  
  # mysql command to connect to your drupal database
  MYSQL_CMD="mysql -N -s -u root johanv6"
  # directory to save the files
  OUT_DIR="/tmp/out"
  
  mkdir -p $OUT_DIR
  
  nodes=$(echo "
        SELECT nid
        FROM node
        WHERE status > 0 
        " | $MYSQL_CMD);
  
  for nid in $nodes
  do
        out_file=$OUT_DIR/$nid.rst
  
        details=$(echo "
                SELECT FROM_UNIXTIME(created),title 
                FROM node
                WHERE nid=$nid
                " | $MYSQL_CMD | sed 's/\t/;/g');
  
        created=`echo $details | cut -f 1 -d\;`
        title=`echo $details | cut -f 2 -d\;`
  
        tags=$(echo "
                SELECT GROUP_CONCAT(td.name)
                FROM term_node tn JOIN term_data td ON tn.tid=td.tid
                WHERE tn.nid=$nid
                " | $MYSQL_CMD);
  
        cat > $out_file << EOF
  .. title: $title
  .. slug: node-$nid
  .. date: $created
  .. tags: $tags
  .. link:
  .. description: 
  .. type: text
  
  EOF
  
  
        echo "SELECT body FROM node_revisions WHERE nid=$nid" | \
        $MYSQL_CMD | \
  # convert node from html to rst
        pandoc --from=html --to=rst | \
  # some trial and error for newlines
        sed 's/\\\\n/\n/g' | \
  # convert references to other posts
        perl -p -000 -e 's;`((\s|[^<])*)</node/([0-9]*)>`__;:doc:`\1<node-\3>`;g' | \
  # lots of trial and error to convert inline code
          perl -p -000 -e 's/``([^`]*\\n[^`]*)``/\n\n::\n\n\1\n\n/g' | \
        sed 's/\\n/\n  /g' | sed 's/\\t/\t/g' | sed 's/\\ / /g' | \
  # convert video-links to youtube-links
  # I did the conversion of \_ to _ manually
        sed 's/\[video:.*[/=]\([^/=]*\)\]/.. youtube:: \1/g' >> $out_file
  
  # some output to show progress
        echo -n .
  
  done

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
all requests /node/number to the correct page on the new blog ::

  RewriteEngine On
  RewriteCond %{HTTP_HOST} !^blog\.
  RewriteRule ^(.*)node/(.*)$ http://blog.johanv.org/posts/node-$2 [R=301,L]
  RewriteRule ^(.*)$ http://blog.johanv.org/$1 [R=301,L]

You will have to 
With the combination of the script and the .htaccess file, 90% of the
migration was very easy. But - as always - the remaining 10% needs some
manual work. Like e.g. converting the youtube links containing
underscores. Those underscores were prefixed with a backslash, which
wasn't correct. Because there weren't too many of those errors, I fixed
them manually.

Another thing you should do manually, is migrating attachments and images to
your new site. Let's hope you don't have too many of them. And if so, you
can probably write a script as well.

