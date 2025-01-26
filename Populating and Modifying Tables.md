### Inserting Data
Since there is not yet any data in the person and favorite_food tables, we have to add them. There are three main components to an `insert` statements:
- The name of the table into which to add the data
- The names of the columns in the table to be populated
- The values with which to populate the columns

### Auto-incrementing `key_value`
`ALTER TABLE person MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT`

```
If you are running these statements in your database, you will first
need to disable the foreign key constraint on the favorite_food
table, and then re-enable the constraints when finished. The pro‐
gression of statements would be:

set foreign_key_checks=0;
ALTER TABLE person
	MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT;
set foreign_key_checks=1;
```


### The insert statement
It’s time to add some data. 
```
mysql> INSERT INTO person
    -> (person_id, fname, lname, eye_color, birth_date)
    -> VALUES (null, 'William', 'Turner', 'BR', '1972-05-27');
Query OK, 1 row affected (0.00 sec)
```

Ideally, we should have this:
```
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person;
+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
+-----------+---------+--------+------------+
1 row in set (0.00 sec)
```

*Explaining:*
```
Select: is for chosing data from table; then you have to specify, what do you want to see

person_id, fname, lname, birth_date 
are the informations, which we want to see 
``` 


### Filtering `select` function
```
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person
    ->  WHERE lname = 'Turner';



+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
+-----------+---------+--------+------------+
1 row in set (0.00 sec)
```