
| Clause name | Purpose                                                                           |
| ----------- | --------------------------------------------------------------------------------- |
| select      | Determines which columns to include in the query's result set                     |
| from        | Identifies the tables from which to retrieve data and how tables should be joined |
| where       | Filters out unwanted data                                                         |
| group by    | Used to group rows together by common column values                               |
| having      | Filters out unwanted groups                                                       |
| order by    | Sorts the rows of the final result set by one or more columns                     |


### The `select` Clause
```
mysql> select *
    -> from language
    -> ;
+-------------+----------+---------------------+
| language_id | name     | last_update         |
+-------------+----------+---------------------+
|           1 | English  | 2006-02-15 05:02:19 |
|           2 | Italian  | 2006-02-15 05:02:19 |
|           3 | Japanese | 2006-02-15 05:02:19 |
|           4 | Mandarin | 2006-02-15 05:02:19 |
|           5 | French   | 2006-02-15 05:02:19 |
|           6 | German   | 2006-02-15 05:02:19 |
+-------------+----------+---------------------+
6 rows in set (0.00 sec)
```
This query could be described in English as follows:
*Show me all the columns and all the rows in the **language** table.*

If you are just looking to retrieve non-duplicate rows, you can use command: `SELECT DISTINCT actor_id FROM film_actor ORDER BY actor_id`

### The `from` Clause
Thus far, you have seen queries whose from clauses contain a single table. Although most SQL books define the from clause as simply a list of one or more tables, I would like to broaden the definition as follows:
*The from clause defines the tables used by a query, along with the means of linking the tables together.*

### Tables
When confronted with the term table, most people think of a set of related rows stored in a database. While this does describe one type of table, I would like to use the word in a more general way by removing any notion of how the data might be stored and concentrating on just the set of related rows. Four different types of tables meet this relaxed definition:
- Permanent tables (i.e., created using the create table statement)
- Derived tables (i.e., rows returned by a subquery and held in memory)
- Temporary tables (i.e., volatile data held in memory)
- Virtual tables (i.e., created using the create view statement)


#### Derived (subquery-generated) tables
A subquery is a query contained within another query. A subquery serves the role of generating a derived table that is visible from all other query clauses and can interact with other tables named in the from clause. Here’s a simple example:
```
mysql> SELECT concat(cust.last_name, ', ', cust.first_name) full_name
    -> FROM
    -> (SELECT first_name, last_name, email
    -> FROM customer
    -> WHERE first_name = 'JESSIE'
    -> ) cust;
+---------------+
| full_name     |
+---------------+
| BANKS, JESSIE |
| MILAM, JESSIE |
+---------------+
2 rows in set (0.01 sec)
```

In this example, a subquery against the customer table returns three columns, and the containing query references two of the three available columns. The subquery is referenced by the containing query via its alias, which, in this case, is `cust`. The data in `cust` is held in memory for the duration of the query and is then discarded.


#### Temporary tables
Although the implementations differ, every relational database allows the ability to define volatile, or temporary, tables. These tables look just like permanent tables, but any data inserted into a temporary table will disappear at some point (generally at the end of a transaction or when your database session is closed). Here's a simple example:
```
mysql> CREATE TEMPORARY TABLE actors_j
    -> (actor_id smallint(5),
    -> first_name varchar(45),
    -> last_name varchar(45)
    -> );
Query OK, 0 rows affected, 1 warning (0.00 sec)


mysql> INSERT INTO actors_j
    -> SELECT actor_id, first_name, last_name
    -> FROM actor
    -> WHERE last_name LIKE 'J%';
Query OK, 7 rows affected (0.00 sec)
Records: 7  Duplicates: 0  Warnings: 0


mysql> SELECT * FROM actors_j;
+----------+------------+-----------+
| actor_id | first_name | last_name |
+----------+------------+-----------+
|      119 | WARREN     | JACKMAN   |
|      131 | JANE       | JACKMAN   |
|        8 | MATTHEW    | JOHANSSON |
|       64 | RAY        | JOHANSSON |
|      146 | ALBERT     | JOHANSSON |
|       82 | WOODY      | JOLIE     |
|       43 | KIRK       | JOVOVICH  |
+----------+------------+-----------+
7 rows in set (0.00 sec)
# These seven rows are held in memory temporarily and will disappear 
# after your session is closed
```