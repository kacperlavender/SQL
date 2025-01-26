
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

