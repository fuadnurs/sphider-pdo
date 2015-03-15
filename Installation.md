# Acknowledgement #

This page is adapted from the instructions on the [original Sphider site](http://www.sphider.eu/docs.php).

Most of the original Sphider documentation still applies to this variant, so you may want to look through it for customization options.


# Installation #

  1. Unpack the files somewhere on your drive. On Microsoft Windows, you can create a folder "sphider" in your "My Documents" folder. On Linux, make a subdirectory in your home directory.<p>Alternatively, you can unpack the files directly on the server, if that is convenient to you.<br>
<ol><li>In the server, create a database to hold Sphider data (in MySQL, SQLite or another supported format).<p>If you have <b>direct access to the server</b> (e.g. with a secure shell login), the steps are:<br>
<ul><li>At command prompt type (to log into MySQL): <code>mysql -u &lt;your username&gt; -p</code><br>(enter your password when prompted)<br>
</li><li>At the mysql prompt, type: <code>CREATE DATABASE sphider_db;</code><br>(of course you can use some other name for database instead of sphider_db).<br>
</li><li>Leave the mysql client with <code>exit</code><p>For more information on how to create a MySQL database and give it the necessary permissions, check <a href='http://www.MySQL.com'>http://www.MySQL.com</a>
</li></ul><blockquote>When you use <b>DirectAdmin</b>:<br>
</blockquote><ul><li>Log-on to your DirectAdmin account<br>
</li><li>Click on "MySQL Management"<br>
</li><li>In the next page, click on "Create new Database"<br>
</li><li>Fill in the database name (e.g. "sphider_db"), the user name and the password. Note that DirectAdmin may prefix the account name to the database name (and/or the user name).<br>
</li></ul><blockquote>When using <b>SQLite</b>, make sure that the folder in which the database is stored, as well as the file itself, have read/write permissions for the web server pseudo-user.<br>
</blockquote></li><li>In the "settings" directory, edit the "database.php" file and change the definition of the database (in variable <code>$db</code>); See the page <a href='DatabaseSetup.md'>Database configuration</a> for examples and notes, and see the PDO documentation technical details.<p>For <b>MySQL</b>, constant <code>DATABASE_NAME</code> also needs to be set, and possibly <code>TABLE_PREFIX</code> as well.<p>For <b>SQLite</b>, constant <code>AUTOINCREMENT</code> has to be defined as an empty string, for example:<p><code> define("AUTOINCREMENT", ""); </code>
</li><li>In the "admin" directory, edit "auth.php" to change the administrator user name and password (default values are "admin" and "admin"). In this file, you can also disable the web interface for changing the configuration.<br>
</li><li>Upload everything to your web server (not needed if you had unpacked all files on the server directly).<br>
</li><li>Open "install.php" script ("admin" directory) in your browser, which will create the tables necessary for Sphider to operate.<p>So, when your web server is at "www.mydomain.com" and you uploaded Sphider in directory "search" at that domain, you need to type the following address in your browser:<p><code> http://www.mydomain.com/search/admin/install.php </code><p>The install page just lists a number of names for tables (25 in total) and it should end with a line that all is fine. If there is an error message, or if the table names do not appear, the installation failed.<br>
</li><li>Still in the web browser, open admin/admin.php in browser. Again, the full address would be:<p><code> http://www.mydomain.com/search/admin/admin.php </code><p>You need to fill in your name and password (the ones in "auth.php"). Note that these are both case-sensitive.<br>
</li><li>Now you can add your site to the search engine. A "site" refers to the first page where the spidering starts. For technical reasons, this must be a true file; it may be a PHP file, but not a redirect. So, continuing the example that your site is at "www.mydomain.com", you could add:<p><code> http://www.mydomain.com/index.html </code><p>(This assumes that there is a file "index.html" on the site.)<br>
</li><li>After adding the site, click on the button "options" next to the site and then click on "index". The default spidering depth is 2, which is fine for a test, but see the page <a href='IndexingOptions.md'>Indexing options</a>.<br>
</li><li>"search.php" is the default search page. You can how test the search engine. Assuming that you uploaded Sphider in the directory "search" at the root of the domain, the full address would be:<p><code> http://www.mydomain.com/search/search.php </code>
</li><li>If the test succeeds, it is recommended to delete the file install.php from the "admin" directory. This is because this script is not password-protected.</li></ol>


<h1>Configuration</h1>

The Sphider search engine can be customized either through the administrator interface or by editing the configuration file directly.<br>
<ul><li>To use the administrator interface, open <code>admin/admin.php</code> in your browser.<br>
</li><li>To change the configuration directly, edit the file <code>settings/conf.php</code>.</li></ul>

Note that the administrator interface can be disabled, for changing the configuration. in the file <code>admin/auth.php</code>, you can set the definition of <code>CONFIGSET</code> to 1 to disable the administrator interface. This increases the security of the search engine (but reduces the convenience).<br>
<br>
To change the look of the search page to fit your site, modify or add a template in the templates directory. It is often enough to modify the search.css file and header and footer templates (header.html and footer.html). Heavier modifications can be made through editing the rest of template files.<br>
<br>
The list of file types that are not checked for indexing are given in<br>
admin/ext.txt. The list of common words that are not indexed are given in include/common.txt.