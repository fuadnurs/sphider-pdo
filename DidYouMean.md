# "Did you mean" in the original Sphider #

When a search query returns zero results, Sphider goes through each word in the query and looks for the closest alternative in the list of keywords. The goal is to suggest alternative spellings (American English versus British English) or hint at typing mistakes.

To find the closest words, Sphider first uses a query with the SOUNDEX function, to limit the word list to those that are at least somewhat similar. From this short-list, it picks the term with the lowest Levenshtein distance.

Once all words have gone through this, Sphider constructs a new search phrase from them en presents this as the "Did you mean?" alternative.

# Modifications in Sphider-PDO #

The first modification is that Sphider now uses a [double-metaphone function](http://en.wikipedia.org/wiki/Metaphone) instead of SOUNDEX. SOUNDEX is widely available, but also widely considered deficient. Since the double-metaphone is much costlier to calculate (and since most databases do not support it internally), Sphider calculates the double-metaphone for all keywords when spidering a site, and stores the values along with the keywords.

When searching for alternative spellings or close names, Sphider first limits the list to all keywords that have a matching double-metaphone. From this list, Sphider then picks the closest words using three heuristics: the Levenshtein distance, the Levenshtein distance on the words after removing the accents, and the PHP `similar_text` function (which uses an algorithm published in _Programming Classics_ by Ian Oliver).

Another modification is that Sphider may now also suggest alternatives even if there are results. That is, the original Sphider only showed a "Did you mean?" line if a search query had zero results. The Sphider-PDO release can be configured to also show this prompt when there are results. The rationale for this change is that alternative spellings of the same word may be used on different pages of a site. Although this option of always suggesting alternatives has its merits, it also has disadvantages, see further down on this page.

Furthermore, Sphider now proposes multiple new search queries in its "Did you mean?" prompt, one for each (significant) word in the original query. So a search query for "specter of the synagogue" would suggest both "specter of the synagog" and "spectre of the synagogue" (assuming that all of these spellings are used on the site). The rationale is that when the user does not find what he/she is looking for, he/she may want to use one of the suggested alternative spellings or close words, but typically not _all_ alternatives at the same time. (The original Sphider would suggest only "spectre of the synagog".)

There is an exception to the above procedure: when one or more of the words (or phrases) in the search query are not found, Sphider will _only_ list alternatives for the words that are _not found_. For example, when the search query is for "specter of the synagog", but "synagog" is only spelled as "synagogue" on the site, there will be a suggestion for "specter of the synagogue", but not for "spectre of the synagog" (still assuming that "specter" and "spectre" are both used at the site). The rationale is that, since "synagog" does not exist as a keyword, there is no sense in suggesting "spectre of the synagog" as an alternative for "specter of the synagog", because it will not produce any results either.

Finally, this release of Sphider supports with another word list, `nonpareil.txt`, which holds words that will be skipped when looking for alternatives. When the user searches for the word "LED", you do not want to suggest the word "let" instead. The same goes for brand names: if the user searches for a (correctly spelled) brand name, you will want to avoid suggesting alternative words for the name. Words like LED and (brand) names can then be added to the `nonpareil.txt` file.

The file `nonpareil.txt` should have a single word per line, just like the `common.txt` file (with the words that are excluded from search because they are too common).

The requirement to _tweak_ the keyword suggestion using a _nonpareil_ word list is the drawback of always suggesting alternatives. If you disable that extension of this Sphider-PDO port (in the `conf.php` file), no `nonpareil.txt` is needed.

## Explicit equivalents/synonyms ##

Sphider-PDO allows you to add a file that contain explicit equivalents. For example, you can add the word "kosmonaut" as an explicit equivalent to "astronaut". When the user types in "astronaut" on the search line, "kosmonaut" will then also be suggested.

The file `pareil.txt` has one record per line, with the format `keyword=alternative`. The keyword must be a single word, the alternative may be two words (or more). Each definition of equivalents is one-way only. If you want to have "astronaut" suggested on the search term "kosmonaut", _as well as_ vice versa, you will need two lines in the `pareil.txt` file:
```
astronaut=kosmonaut
kosmonaut=astronaut
```

## Word combinations ##

Sphider splits the text in keywords and searches for individual words. However, word combinations are sometimes written as two separate words, sometimes as a single word and sometimes with a dash in between.

For example, a concept like "role play" should probably (officially) be written exactly like that, as two separate words. However, it is also commonly written as a single word ("roleplay") and with a dash between the words ("role-play"). The same applies to word combinations like "full colour" (commonly written as "fullcolour" as well as "full-colour"), and many others.

For Sphider, "role play" is stored as two separate keywords, "role" and "play", and "roleplay" and "role-play" are stored as single keywords. Yet, when the user types it in one way in the search box, we will want to suggest the other way(s), if applicable. So... what Sphider-PDO does when the user types several words on the search line, it also searches for combinations of two words glued together or with a dash between them. That is, when the user types "role play", Sphider-PDO searches whether the keywords "roleplay" and "role-play" exist (and suggests them if they do).

When the user types "roleplay" and "role-play" exists too, that form will be suggested via the similar-text search described in the first section of this page. Similarly, a search for "role-play" will suggest "roleplay". There is no automatic way to have a word like "roleplay" bring up a suggestion for "role play" (two words), but these can be added as explicit equivalents (in the `pareil.txt` file).

The word combination search and the explicit equivalent list are new to Sphider-PDO (they are not in the original Sphider).