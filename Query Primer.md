
| Clause name | Purpose                                                                           |
| ----------- | --------------------------------------------------------------------------------- |
| select      | Determines which columns to include in the query's result set                     |
| from        | Identifies the tables from which to retrieve data and how tables should be joined |
| where       | Filters out unwanted data                                                         |
| group by    | Used to group rows together by common column values                               |
| having      | Filters out unwanted groups                                                       |
| order by    | Sorts the rows of the final result set by one or more columns                     |


### The `select` Clause
```sql
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
```sql
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
```sql
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

#### Views 
A view is a query that is stored in the data dictionary. It looks and acts like a table, but there is no data associated with a view (this is why I call it *virtual table*). When you issue a query against a view, your query is merged with the view definition to create a final query to be executed. 
```sql
mysql> CREATE VIEW cust_vw AS
-> SELECT customer_id, first_name, last_name, active
-> FROM customer;
Query OK, 0 rows affected (0.12 sec)
```
When the view is created, no additional data is generated or stored: the server simply tucks away the select statement for future use.
```sql
mysql> SELECT first_name, last_name
    -> FROM cust_vw
    -> WHERE active=0;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| SANDRA     | MARTIN    |
| JUDITH     | COX       |
| SHEILA     | WELLS     |
| ERICA      | MATTHEWS  |
| HEIDI      | LARSON    |
| PENNY      | NEAL      |
| KENNETH    | GOODEN    |
| HARRY      | ARCE      |
| NATHAN     | RUNYON    |
| THEODORE   | CULP      |
| MAURICE    | CRAWLEY   |
| BEN        | EASTER    |
| CHRISTIAN  | JUNG      |
| JIMMIE     | EGGLESTON |
| TERRANCE   | ROUSH     |
+------------+-----------+
15 rows in set (0.01 sec)
```

### Table links
The second deviation from the simple from clause definition is the mandate that if more than one table appears in the from clause, the conditions used to link the tables must be included as well.


### The where Clause
In some cases, you may want to retrieve all rows from a table, especially for small tables such as language. Most of the time, however, you will not want to retrieve every row from a table but will want a way to filter out those rows that are not of interest. This is a job for the where clause
`The where clause is the mechanism for filtering out unwanted rows from your result set.`
```sql
mysql> select title
    -> from film
    -> where rating = 'G' and rental_duration >= 7;
+-------------------------+
| title                   |
+-------------------------+
| BLANKET BEVERLY         |
| BORROWERS BEDAZZLED     |
| BRIDE INTRIGUE          |
...
| WAKE JAWS               |
| WAR NOTTING             |
+-------------------------+
29 rows in set (0.00 sec)
```

So, what should you do if you need to use both and and or operators in your where clause?
```sql
mysql> select title, rating, rental_duration
    -> from film
    -> where (rating = 'G' and rental_duration >=7) or (rating = 'PG-13' and rental_duration < 4);
+-------------------------+--------+-----------------+
| title                   | rating | rental_duration |
+-------------------------+--------+-----------------+
| ALABAMA DEVIL           | PG-13  |               3 |
| BACKLASH UNDEFEATED     | PG-13  |               3 |
| BILKO ANONYMOUS         | PG-13  |               3 |
| BLANKET BEVERLY         | G      |               7 |
| BORROWERS BEDAZZLED     | G      |               7 |
...
| WAIT CIDER              | PG-13  |               3 |
| WAKE JAWS               | G      |               7 |
| WAR NOTTING             | G      |               7 |
| WORLD LEATHERNECKS      | PG-13  |               3 |
+-------------------------+--------+-----------------+
68 rows in set (0.00 sec)
```


### The group by and having Clauses
let’s say you wanted to find all of the customers who have rented 40 or more films. Rather than looking through all 16,044 rows in the rental table, you can write a query that instructs the server to group all rentals by customer, count the number of rentals for each customer, and then return only those customers whose rental count is at least 40.
```sql
mysql> select c.first_name, c.last_name, count(*)
    -> from customer c
    -> inner join rental r
    -> on c.customer_id = r.customer_id
    -> group by c.first_name, c.last_name
    -> having count(*) >= 40;
+------------+-----------+----------+
| first_name | last_name | count(*) |
+------------+-----------+----------+
| TAMMY      | SANDERS   |       41 |
| CLARA      | SHAW      |       42 |
| ELEANOR    | HUNT      |       46 |
| SUE        | PETERS    |       40 |
| MARCIA     | DEAN      |       42 |
| WESLEY     | BULL      |       40 |
| KARL       | SEAL      |       45 |
+------------+-----------+----------+
7 rows in set (0.03 sec)
```


### The order by Clause
In general, the rows in a result set returned from a query are not in any particular order. If you want your result set to be sorted, you will need to instruct the server to sort the results using the order by clause: `The order by clause is the mechanism for sorting your result set using either raw col‐ umn data or expressions based on column data.`
```sql
mysql> select c.first_name, c.last_name,
    -> time (r.rental_date) rental_time
    -> from customer c
    -> inner join rental r
    -> on c.customer_id = r.customer_id
    -> where date(r.rental_date) = '2005-06-14'
    -> order by c.last_name;
+------------+-----------+-------------+
| first_name | last_name | rental_time |
+------------+-----------+-------------+
| DANIEL     | CABRAL    | 23:09:38    |
| CATHERINE  | CAMPBELL  | 23:17:03    |
| HERMAN     | DEVORE    | 23:35:09    |
| AMBER      | DIXON     | 23:42:56    |
...
| JEFFERY    | PINSON    | 22:53:33    |
| MINNIE     | ROMERO    | 23:00:34    |
| TERRANCE   | ROUSH     | 23:12:46    |
+------------+-----------+-------------+
16 rows in set (0.03 sec)
```