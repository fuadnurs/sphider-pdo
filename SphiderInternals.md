# Introduction #

This page is a "work in progress" on some of the internal operations of the Sphider search engine. These notes may clarify some to the limitations of the script.

# Character encodings #

There are various character encodings in play with each other:
  * The database stores text as Latin-1.
  * In HTML pages, special characters are encoded as HTML entities (`&eacute` for é). This applies both the pages scanned by the spider and to the pages generated as "search results" by the search engine.
  * When the user types a word with an accented character in the search field, this word is passed to the search engine in the encoding specified in the page. Historically, this is Latin-1, but currently UTF-8 is common (see also the section on characters sets in the [instructions for integrating search on your site](IntegratingSearchOnYourSite.md).

The Sphider script need to convert one encoding into the other. The supported character set is the lowest common denominator, meaning Latin-1 (alias ISO 8859-1). This is sufficient for Western and West European languages.

# Keyword collection #

The "spidering" operation builds tables with URLs ("links"), keywords, and a set of cross-reference tables that associate each keyword to a set of URLs.

The first step in collecting the keywords on a page is to clean the text up. First, HTML comment blocks, style sheets and script blocks (e.g. Javascript) are removed. Parts in the HTML file flagged as `sphider_noindex` are removed as well. HTML tags are removed next, but a space is inserted at that place if needed (to prevent words that are separated by a tag to become concatenated).

This becomes the "full text" as stored in the database.

Then, the spider converts the HTML to Latin-1, translating HTML entities for accented characters to those characters. Any HTML entities that are left in the text, are replaced by space characters. That is, any character that cannot be represented in Latin-1 encoding, is stripped from the text. Punctuation is also replaced by spaces, as well as parentheses and brackets. (Multiple consecutive spaces get combined to a single space.) What is left is a list of space-separated words. The spider runs through them and adds each as a keyword.

In removing the punctuation, there is one exception. A period between two digits is not removed. So in a text like "Version: 1.3.6", the colon will be removed, but the two periods are not.

# Database and database-interface limitations #

The port of Sphider uses the "PHP Data Objects" interface (PDO). PDO abstracts _access_ to a database. Basically, using PDO, you can talk to any database witout changing the code, with the exception of the [database specification string](DatabaseSetup.md).

It also means that PDO restricts itself to providing a common denominator for database access. PHP also includes a MySQL binding, with functions that are very specific to MySQL. A few of these functions are used in the database optimization and backup pages. Since there is no PDO-equivalent for these functions, Sphider only presents the page for database optimization and backup when the database configured for PDO happens to be MySQL.

Next to a common demonitator for the database access, the database queries also need to be a common denominator for what databases accept. In practice, this means: standard SQL with no (or very few) extensions. Even with that, complete portability cannot be guaranteed, simply because a database may have limitations.

One known issue in this area is SQLite version 2: querying the "Top Keywords" in the "Statistics" page is so slow that the PHP script usually aborts on time-out before returning results. No "fix" for this is planned, moving to SQLite 3 solves the problem.