.. title: How to apply a patch?
.. slug: how-to-apply-a-patch
.. date: 2016-02-25 20:05:07 UTC+01:00
.. tags: linux,commandline,patch
.. link:
.. description: I have a patch file, and I don't know what to do with it. Help!
.. type: text

Suppose that you want to use e.g. a CiviCRM extension like
`CDNTaxReceipts <https://github.com/jake-mw/CDNTaxReceipts/archive/1.3.1.zip>`_ , but
you want some extra functionality, and someone told you that there is
`a patch <https://patch-diff.githubusercontent.com/raw/jake-mw/CDNTaxReceipts/pull/39.diff>`_
available.

Now suppose you don't use git, and you don't have a clue about how a patch
works. Then this is the easy way to apply it.

1. You download the patch file, and you save it on your file system, e.g.
   as ``~/Downloads/39.diff``

2. You use the command line. Supposing you unpacked the archive in
   ``~/dev/CDNTaxReceipts-3.1`` ::

    cd ~/dev/CDNTaxReceipts-3.1
    patch -p1 < ~/Downloads/39.diff

Here you are. You patched the extension.

If you use Windows, the hardest part will be finding a patch executable. On
almost every other OS, patch will be available by default.
