Sometimes you want to work with every row in a table, such as:
- Purging all data from a table used to stage new data warehouse feeds 
- Modifying all rows in a table after a new column has been added
- Retrieving all rows from a message queue table

In cases like these, your SQL statements won’t need to have a where clause, since you don’t need to exclude any rows from consideration. Most of the time, however, you will want to narrow your focus to a subset of a table’s rows. Therefore, all the SQL data statements (except the insert statement) include an optional where clause con‐ taining one or more filter conditions used to restrict the number of rows acted on by the SQL statement. Additionally, the select statement includes a having clause in which filter conditions pertaining to grouped data may be included.

