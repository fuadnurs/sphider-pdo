# Keeping pages from being indexed #

There are three ways to avoid pages to be indexed, and one additional way to keep sections from a page to be indexed.

  * robots.txt
  * must include / must not include
  * the `nofollow` link attribute
  * `sphider_noindex` comments

## robots.txt ##

The most common way to prevent pages from being indexed is using the robots.txt standard, by either putting a robots.txt file into the root directory of the server, or adding the necessary meta tags into the page headers.


## Must include / must not include ##

A powerful option Sphider supports is defining a must include / must not include string list for a site (click on Advanced options in Index screen for this). Any url containing a string in the "must not include" list is ignored. Any url that does not contain any string in the "must include" list is likewise ignored.

All strings in the string list should be separated by a newline enter). For example, to prevent a forum in your site from being indexed, you might add `www.yoursite.com/forum` to the "must not include" list. This means that all URLs containing the string will be ignored and wont be indexed. Using Perl-style regular expressions instead of literal strings is also supported.

Every string starting with a "**" in front is considered as a regular expression, so that "**/[a](a.md)+/" denotes a string with one or more a's in it.


## Ignoring links (nofollow) ##

Sphider respects the `rel="nofollow"` attribute in `<a href..>` tags, so for example the link foo.html in `<a href="foo.html" rel="nofollow>` is ignored.


## Ignoring parts of a page ##

Sphider includes an option to exclude parts of pages from being indexed. This can for example be used to prevent search result flooding when certain keywords appear on certain part in most pages (like a header, footer or a menu). Any part of a page between
```
<!--sphider_noindex-->
```
and
```
<!--/sphider_noindex-->
```
tags is not indexed. However, links in such a section are still followed.