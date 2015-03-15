# Multilingual support #

Most sites use only a single language. Although the original Sphider supports multiple languages, it considers that a site will choose only one. This derivative, Sphider-PDO, has improved support for multilingual sites, although it is still limited.

## The language of a page is stored in the database ##

Starting with version 1.3.6, Sphider now also stores the language that is set in a web page into the database. You can set a language in the "`<HTML>`" tag. For example, when the page is written in German, you can start the HTML code with "`<html lang='de'>`".

When you have an existing Sphider database of version 1.3.5 or earlier, you must rebuild the database. The easiest way to do this is to erase the database and then run ["install.php"](Installation.md) again. After that, create the site definitions and index them (full index).

## Override the default language for the search form ##

The default language for a search is set in the file config.php. As explained in the page on [integrating search on your site](IntegratingSearchOnYourSite.md), you can override the language with a hidden field on your search form. So, if a user has chosen to view your site in German (explicitly or based on the language of his/her browser), the matching search form can contain the following code:
```
<input type="hidden" name="lang" value="de">
```
The language code from the above hidden field, is passed onto the search results template as a global variable, called `$language`.

## Display the language ##

In the "search result" template, the language of each URL/page is passed in the `$qry_results[n]['lang']` (where `n` is an index). The templates provided with Sphider-PDO do not take advantage of this field, but if you have a multilingual site, you can use this field to show the language of the page.

## Sort order ##

When a language is set for an URL/page and this language is not the same as the user language (as passed through the form), the weight of the match is decreased. This way, if a page exists in multiple languages, the one in the user's language comes on top.