# Indexing options #

When starting to "index" a site, you can adjust several parameters to limit the operation.

| Full | Indexing continues until there are no further (permitted) links to follow. |
|:-----|:---------------------------------------------------------------------------|
| To depth | Indexes to a given depth, where depth means how many "clicks" away the page can be from the starting page. Depth 0 means that only the starting page is indexed, depth 1 indexes the starting page and all the pages linked from it etc. |
| Reindex | By checking this checkbox, indexing is forced even if the page already has been indexed. |
| Spider can leave domain | By default, Sphider never leaves a given domain, so that links from mydomain.com pointing to evilcompetition.com are not followed. By checking this option Sphider can leave the domain, however in this case its highly advisable to define proper "must include / must not include" string lists to prevent the spider from going too far. |
| Must include / must not include | See the page [Keeping pages from being indexed](KeepOutOfIndex.md) for an explanation. |