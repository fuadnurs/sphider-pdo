# Database configuration strings #

PDO requires a driver for the database. You can query whether a suitable PDO driver is installed with phpinfo.

The PDO definition must be in a file called "database.php" which is in the "settings" directory.

Below are a few examples.

## MySql ##

```
define('DATABASE_NAME', 'sphider');
define('TABLE_PREFIX', '');
$db = new PDO('mysql:host=hostname; dbname='.DATABASE_NAME, 'username', 'password');
```

The hostname, username and password need to be filled in in this query. The database name must be in a separate constant (called `DATABASE_NAME`) for the table optimization and backup functionality.

If you have only a single database for multiple purposes (some shared-hosting accounts only give you a single MySQL database), you can avoid naming conflicts in the table names by setting the `TABLE_PREFIX` to some unique string (e.g. "sphider"). All tables that are used by Sphider will get this prefix.


## SQLite 3 ##

```
define('TABLE_PREFIX', '');
define('AUTOINCREMENT, '');
$db = new PDO('sqlite:'.dirname(__FILE__).'/../sphider.sqlite');
```

The `AUTOINCREMENT` constant must be set to an empty string (because it is non-empty by default). SQLite does not support the `autoincrement` keywords (but implicitly defines a primary key to be automatically incremented).

## SQLite 2 ##

```
define('TABLE_PREFIX', '');
define('AUTOINCREMENT, '');
$db = new PDO('sqlite2:'.dirname(__FILE__).'/../sphider.sqlite');
```

See the case for SQLite 3 for the `AUTOINCREMENT` constant.