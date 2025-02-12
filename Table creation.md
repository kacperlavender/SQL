### Step 1: Design
A good way to start designing a table is to do a bit of brainstorming to see what kind of information would be helpful to include. Here’s what I came up with after thinking for a short time about the types of information that describe a person:
- Name
- Eye color
- Birth date
- Address
- Favorite foods


| Column         | Type         | Allowable values |
| -------------- | ------------ | ---------------- |
| name           | varchar(40)  |                  |
| eye_color      | char(2)      | BL, BR, GR       |
| birth_day      | date         |                  |
| address        | varchar(100) |                  |
| favorite_foods | varchar(200) |                  |


### Step 2: Refinement
In looking at the columns in the person table a second time, the following issues arise:
- The name column is actually a compound object consisting of a first name and a last name.
- Since multiple people can have the same name, eye color, birth date, and so forth, there are no columns in the person table that guarantee uniqueness. 
- The address column is also a compound object consisting of street, city, state/province, country, and postal code.
- The favorite_foods column is a list containing zero, one, or more independent items. It would be best to create a separate table for this data that includes a foreign key to the person table so that you know to which person a particular food may be attributed.


After taking these issues into consideration, here is normalized version of `person` table

`person table, second pass`

| **Column Name** | **Data Type**       | **Allowable Values**      |
| --------------- | ------------------- | ------------------------- |
| `person_id`     | `SMALLINT UNSIGNED` | Positive integers only    |
| `first_name`    | `VARCHAR(20)`       | Any string up to 20 chars |
| `last_name`     | `VARCHAR(20)`       | Any string up to 20 chars |
| `eye_color`     | `CHAR(2)`           | `BR`, `BL`, `GR`          |
| `birth_date`    | `DATE`              | Valid date (YYYY-MM-DD)   |
| `street`        | `VARCHAR(30)`       | Any string up to 30 chars |
| `city`          | `VARCHAR(20)`       | Any string up to 20 chars |
| `state`         | `VARCHAR(20)`       | Any string up to 20 chars |
| `country`       | `VARCHAR(20)`       | Any string up to 20 chars |
| `postal_code`   | `VARCHAR(20)`       | Any string up to 20 chars |

Now that the person table has a primary key (person_id) to guarantee uniqueness,
the next step is to build a favorite_food table that includes a foreign key to the
person table.

`favourite food`

| Column    | Type               |
| --------- | ------------------ |
| person_id | smallint(unsigned) |
| food      | varchar(20)        |
The person_id and food columns comprise the primary key of the favorite_food
table, and the person_id column is also a foreign key to the person table.

```sql
mysql> CREATE TABLE person
    -> (person_id SMALLINT UNSIGNED,
    -> fname VARCHAR(20),
    -> lname VARCHAR(20),
    -> eye_color CHAR(2),
    -> birth_date DATE,
    -> street VARCHAR(30),
    -> city VARCHAR(20),
    -> state VARCHAR(20),
    -> country VARCHAR(20),
    -> postal_code VARCHAR(20),
    -> CONSTRAINT pk_person PRIMARY KEY (person_id)
    -> );
Query OK, 0 rows affected (0.03 sec)
```

When you define your table, you need to tell the database server what column or columns will serve as the primary key for the table. You do this by creating a constraint on the table. You can add several types of constraints to a table definition. This constraint is a primary key constraint. It is created on the person_id column and given the name pk_person.

While on the topic of constraints, there is another type of constraint that would be
useful for the person table. Another type of constraint called a check constraint constrains the allowable values for a particular column. MySQL allows a check constraint to be attached to a column definition, as in the following: `eye_color CHAR(2) CHECK (eye_color IN ('BR','BL','GR')),`. However, MySQL does provide another character data type called `enum` that merges the check constraint into the data type definition. Here’s what it would look like for the `eye_color` column definition: `eye_color ENUM('BR','BL','GR'),`. 

So finally, we achieve: 
```sql
CREATE TABLE person
(person_id SMALLINT UNSIGNED,
fname VARCHAR(20),
lname VARCHAR(20),
eye_color ENUM('BR','BL','GR'),
birth_date DATE,
street VARCHAR(30),
city VARCHAR(20),
state VARCHAR(20),
country VARCHAR(20),
postal_code VARCHAR(20),
CONSTRAINT pk_person PRIMARY KEY (person_id)
);
```

Ideally, we should achieve:
```sql
mysql> CREATE TABLE person(person_id SMALLINT UNSIGNED,fname VARCHAR(20),lname VARCHAR(20),eye_color ENUM('BR','BL','GR'),birth_date DATE,street VARCHAR(30),city VARCHAR(20),state VARCHAR(20),country VARCHAR(20),postal_code VARCHAR(20),CONSTRAINT pk_person PRIMARY KEY (person_id));
Query OK, 0 rows affected (0.02 sec)

mysql> desc person
    -> ;
+-------------+----------------------+------+-----+---------+-------+
| Field       | Type                 | Null | Key | Default | Extra |
+-------------+----------------------+------+-----+---------+-------+
| person_id   | smallint unsigned    | NO   | PRI | NULL    |       |
| fname       | varchar(20)          | YES  |     | NULL    |       |
| lname       | varchar(20)          | YES  |     | NULL    |       |
| eye_color   | enum('BR','BL','GR') | YES  |     | NULL    |       |
| birth_date  | date                 | YES  |     | NULL    |       |
| street      | varchar(30)          | YES  |     | NULL    |       |
| city        | varchar(20)          | YES  |     | NULL    |       |
| state       | varchar(20)          | YES  |     | NULL    |       |
| country     | varchar(20)          | YES  |     | NULL    |       |
| postal_code | varchar(20)          | YES  |     | NULL    |       |
+-------------+----------------------+------+-----+---------+-------+
10 rows in set (0.00 sec)
```

Now that you have created the `person` table, your next step will be to then create the
`favorite_food` table:
```sql
mysql> CREATE TABLE favorite_food
    -> (person_id SMALLINT UNSIGNED,
    -> food VARCHAR(20),
    -> CONSTRAINT pk_favorite_food PRIMARY KEY (person_id, food),
    -> CONSTRAINT fk_fav_food_person_id FOREIGN KEY (person_id)
    -> REFERENCES person (person_id)
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> desc favorite_food;
+-----------+-------------------+------+-----+---------+-------+
| Field     | Type              | Null | Key | Default | Extra |
+-----------+-------------------+------+-----+---------+-------+
| person_id | smallint unsigned | NO   | PRI | NULL    |       |
| food      | varchar(20)       | NO   | PRI | NULL    |       |
+-----------+-------------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

```