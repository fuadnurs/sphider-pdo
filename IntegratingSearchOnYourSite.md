# A typical lay-out #

What the user sees of Sphider is the form to enter the search query and the list of results. These pages/forms are based on templates. Sphider comes with two example templates.

The features that may be present on the forms also depend on the settings in the administration section (or in the file `conf.php`). For example, the setting `$advanced_search` determines whether the "advanced search" options (with AND and OR relations) are presented.

It is also typical that a site has a small search field on a fixed location of every page. This is a "mini form" through which you jump to the Sphider search page.

# Details #

An example search "mini form" is:
```
<form action="sphider/search.php" method="post">
<input type="text" name="query" size="18" value="Keywords">
<input type="submit" name="search">
</form>
```
A text field and a submit button are sufficient. The form should redirect to the `search.php` script in the directory where Sphider is installed.

When using a graphic submit button instead of the standard text button, also add a hidden field with the `name` parameter set to "search" and the value set to "1", because Sphider uses other values for different operations. So:
```
<input type="submit" name="search">
```
becomes:
```
<input type="image" src="images/submit.png" name="btn" alt="search">
<input type="hidden" name="search" value="1">
```

If you have a [multilingual site](MultilingualSupport.md), you may want to pass the language of the active page to Sphider. This way, when the user is reading a French page and types in a search query, the query results will also be presented in French. To do so, add a hidden element in the form with the two-letter language code.
```
<input type="hidden" name="lang" value="fr">
```

Any language passed through the form overrules the default language that is configured in the administration section (or `conf.php`).

The order of the search results is also affected by the language: pages in French will get higher priority when passing "fr" as the active language.

# Character sets #

Internally, web browsers typically use Unicode, but the pages may use different character sets. The character set that a page uses should be set in the header. For Sphider, the character set for the page determines in what encoding the search queries will be passed. The original version of Sphider relied on Latin-1, Sphider-PDO uses UTF-8.

The line below tells the browser that a page uses the UTF-8 character set:
```
<meta http-equiv="content-type" content="text/html; charset=utf-8">
```
It is suggested that this header line (or an equivalent one) is added to all pages.