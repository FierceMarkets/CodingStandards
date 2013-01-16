
SQL coding conventions

Last updated November 8, 2011. Created by mr.baileys on August 10, 2003.
Edited by jhodgdon, eporama, davidfells, ywarnier. Log in to edit this page.

Don't use Reserved Words
Don't use (ANSI) SQL / MySQL / PostgreSQL / MS SQL Server / ... Reserved Words for column and/or table names. Even if this may work with your (MySQL) installation, it may not with others or with other databases. Some references:

(ANSI) SQL Reserved Words
MySQL Reserved Words: 5.1, 5.0, 3.23.x, 4.0, 4.1
PostgreSQL Reserved Words
Oracle Reserved Words (in particular UID is a problem in our context)
MS SQL Server Reserved Words
DB2 Reserved Words
Some commonly misused keywords: TIMESTAMP, TYPE, TYPES, MODULE, DATA, DATE, TIME, ...
See also [bug] SQL Reserved Words.

Capitalization and user-supplied data
Make SQL reserved words UPPERCASE. This is not a suggestion. Drupal db abstraction commands will fail if this convention is not followed.
Make column and constraint names lowercase.
Enclose each table name with {} (this allows Drupal to prefix table names).
Variable arguments (which are often user-supplied) should be moved out of the query body and passed in as separate parameters to db_query(), db_query_range(), and db_query_temporary(), etc. The query body should instead contain placeholders specifying the type of each argument (%d|%s|%%|%f|%b). This ensures that the data will be properly escaped and prevents SQL injection attacks.
Preventing SQL injection is easy; db_query provides a way to use parametrized queries. Drupal's database functions replace the sprintf-like placeholders with the properly escaped arguments in order of appearance:
%d - integers
%f - floats
%s - strings, enclose in single quotes
%b - binary data, do not enclose in single quotes
%% - replaced with %
Read more about Database Access
Literal (constant) arguments can either be included in the query body or handled in the same way as variable arguments.
Any string literal or %s placeholder must be enclosed by single quotes: ' . Never use double quotes.
Example:
<?php
db_query("INSERT INTO {filters} (format, module, delta, weight) VALUES (%d, 'php', 0, 0)", $format);
?>
NOTE: as of Drupal 6.x, table definitions and constraints (e.g. primary keys, unique keys, indexes) should be always handled by the Schema API, which solves cross-database compatibility concerns automatically.

Naming
Use singular nouns for table names since they describe the entity the table represents. Drupal 6.x mixed singular/plural usage and this convention changed for Drupal 7.x.
It is a good practice to prefix table names with the module name to prevent possible namespace conflicts.
Name every constraint (primary, foreign, unique keys) yourself. Otherwise, you'll see funny-looking system-generated names in error messages. This happened with the moderation_roles table which initially defined a key without explicit name as KEY (mid). This got mysqldump'ed as KEY mid (mid) which resulted in a syntax error as mid() is a mysql function (see [bug] mysql --ansi cannot import install database).
Index names should begin with the name of the table they depend on, eg. INDEX users_sid_idx.
NOTE: as of Drupal 6.x, table definitions and constraints should be always handled by the Schema API.

Configure your Database server for standard compliance
Most Database Servers use extension to standard SQL. However, many of them can be configured to run in a (more) standard compliant mode. Every developer is encouraged to use the mode most standard compliant to avoid sloppy coding and compatibility problems.

MySQL
Enable ANSI and Strict Mode
Please help growing this list for other database servers!

References
Joe Celko - Ten Things I Hate About You
Joe Celko - SQL for Smarties: Advanced SQL Programming
RDBMS Naming conventions
Indentation
Drupal does not have a standard method for indentation or formating of longer SQL queries on multiple lines. Some competing strategies include:
<?php
if (!(db_query(
  "
    INSERT INTO {mlsp_media_file_type}
    SET extension   = '%s',
    attributes      = '%s'
  ",
  $file_type_entry['extension'],
  $selected_attributes
))) {
  $errors = TRUE;
}
?>

or
<?php
$sql = "SELECT t.*, j1.col1, j2.col2"
. " FROM {table} AS t"
. " LEFT JOIN {join1} AS j1 ON j1.id = t.jid"
. " LEFT JOIN {join2} AS j2 ON j2.id = t.jjid"
. " WHERE t.col LIKE '%s'"
. " ORDER BY %s"
;
$result = db_query($sql, 'findme', 't.weight');
?>

or, using the concept of "rivers" and HEREDOC syntax
<?php
$sql = <<<SQL
 SELECT t.*
      , j1.col1
      , j2.col2 
   FROM {table} AS t 
   LEFT 
   JOIN {join1} AS j1 
     ON j1.id = t.jid 
   LEFT
   JOIN {join2} AS j2
     ON j2.id = t.jid
  WHERE t.col LIKE '%s%' 
  ORDER 
     BY %s
SQL;
$result = db_query($sql, 'findme', 't.weight');
?>


