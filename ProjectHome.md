**Note:** the current release is **version 1.3.8**. Please get it from the source repository.

To download the project without installing a Subversion client, on Linux you can use
```
wget -m -np http://sphider-pdo.googlecode.com/svn/trunk/
```
Microsoft Windows users can use the [Download SVN](http://downloadsvn.codeplex.com/) utility.

---

The standard distribution of Sphider, a search engine in PHP, requires MySQL. This version is ported to use PHP Data Objects (PDO), so that it can be used with many database back-ends, including SQLite.

In general, Sphider-PDO is just a basic port of Sphider version 1.3.5 to use the [PDO interface](DatabaseSetup.md) to talk to databases. Most of the Sphider documentation on configuration and indexing options still applies; please see the [original Sphider site](http://www.sphider.eu/) for that documentation. Documentation specific to this port is in the Wiki of this project.

Instructions for [installation and configuration](Installation.md) are on a separate page.

Notable changes from Sphider 1.3.5:

  * The biggest change is invisible to the user: the database back-end now uses PDO and is no longer hard-linked to MySQL.

  * The section for the database maintenance and backup in the "Admin" interface is available _only_ for MySQL, though. The SQL queries are highly database-specific in this area. (PDO provides _data-access_ abstraction, not _database_ abstraction.)

  * Support for non-English languages has improved; accented characters are now supported. However, Sphider-PDO is still dependent on the Latin-1 encoding. There is limited support for [multilingual sites](MultilingualSupport.md).

  * The interface for ["Did you mean?" search terms](DidYouMean.md) has changed and been extended.