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

### Inserting data into `favorite_food`
```
mysql> INSERT INTO favourite_food (person_id, food)
    -> VALUES (1, 'pizza');
ERROR 1146 (42S02): Table 'sakila.favourite_food' doesn't exist
mysql> INSERT INTO favorite_food (person_id, food)
    -> VALUES (1, 'pizza');
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO favourite_food (person_id, food)
    -> VALUES (1, 'cookies');
ERROR 1146 (42S02): Table 'sakila.favourite_food' doesn't exist
mysql> INSERT INTO favorite_food (person_id, food)
    -> VALUES (1, 'cookies');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO favorite_food (person_id, food)
    -> VALUES (1, 'nachos');
Query OK, 1 row affected (0.00 sec)

```

then, we should have:
```
mysql> SELECT food
    -> FROM favorite_food
    -> WHERE person_id = 1
    -> ORDER BY food;
+---------+
| food    |
+---------+
| cookies |
| nachos  |
| pizza   |
+---------+
3 rows in set (0.00 sec)
```


### Lets execute another insert statement to add Susan Smith to the person table:
```
mysql> INSERT INTO person
-> (person_id, fname, lname, eye_color, birth_date,
-> street, city, state, country, postal_code)
-> VALUES (null, 'Susan','Smith', 'BL', '1975-11-02',
-> '23 Maple St.', 'Arlington', 'VA', 'USA', '20220');
Query OK, 1 row affected (0.01 sec)
```

```
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person;
+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
|         2 | Susan   | Smith  | 1975-11-02 |
+-----------+---------+--------+------------+
2 rows in set (0.00 sec)
```

Now, it is time to update previous `person` with information about address:
```
mysql> UPDATE person
    -> SET street = '1225 Tremont St.',
    -> city = 'Boston',
    -> state = 'Ma',
    -> country = 'USA',
    -> postal_code = '02138'
    -> WHERE person_id = 1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```