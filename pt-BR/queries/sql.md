# SQL


## WPDB

It can be tempting for the uninformed to resort to a raw SQL query to grab posts. Only do this as a last resort.

But if you have to make an SQL query, use `WPDB` objects.

## dbDelta and Table Creation

The `dbDelta` function examines the current table structure, compares it to the desired table structure, and either adds or modifies the table as necessary, so it can be very handy for updates.

The `dbDelta` function is rather picky, however. For instance:

 - You must put each field on its own line in your SQL statement.
 - You must have two spaces between the words `PRIMARY KEY` and the definition of your primary key.
 - You must use the key word `KEY` rather than its synonym `INDEX` and you must include at least one KEY.
 - You must not use any apostrophes or backticks around field names.
 - `CREATE TABLE` must be capitalised.

With those caveats, here are the next lines in our function, which will actually create or update the table. You'll need to substitute your own table structure in the $sql variable.

## Further Reading

 - [Creating Tables With Plugins - Codex](http://codex.wordpress.org/Creating_Tables_with_Plugins#Creating_or_Updating_the_Table)
