# Relocating a database #

Many companies and individuals who maintain a web site have two copies: a public web site which is there to see for everyone in the world, and a local "sand box" where they can develop new features, write new content and test the results without interfering with the public web site.

The "sand box" web site is essentially a copy of the public web site, including the search features. Therefore, the search database should be kept up to date on the sand box too. After the local web site has been thoroughly tested and the time comes to update the public web site with a copy of the local site, you might want to just copy the database from the local server. The alternative is to re-index Sphider on the public server after the update, but uploading will typically be (much) faster than re-indexing.

However,... the Sphider database keeps the full URL to the pages in its "links" tables. Therefore, if you upload the database, all search results will point to local addresses (which may not even be accessible from the outside). What you need to do before uploading, is to relocate the server.

# Limitations #

The database on the public server should be able to read the files/tables created on the "sandbox" server. In practice, it means that you need a self-contained database, such as SQLite, and that both the local and public servers need to have the same version of that self-contained database.

# Relocation: step by step #

In the Administrator interface, select the TAB "Sites" and for the site that you need to relocate, click on the button/link "options".

This will give you a screen with a table with the site's URL, its title, a description and the date that it was last indexed, with on the right a column with a set of buttons. From these buttons, click on the top one, "Edit".

In the new screen, change the URL to point to the public server. Make sure that you include "http://" (or "https://") because Sphider relies on it. Then click on "Update" near the bottom of the form.

Depending on the size of the database, relocation can take some time (but this is still on the local machine). After it has completed, the new database can be uploaded.

# Closing words #

This feature is mostly useful when using the SQLite database, because the entire database is just a single file. It requires no reconfiguration after uploading.

This feature is specific to this PDO port. The standard version of Sphider only adjusts the site URL _without_ changing the links.