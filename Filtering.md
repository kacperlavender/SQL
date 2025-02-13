Sometimes you want to work with every row in a table, such as:
- Purging all data from a table used to stage new data warehouse feeds 
- Modifying all rows in a table after a new column has been added
- Retrieving all rows from a message queue table

In cases like these, your SQL statements won’t need to have a where clause, since you don’t need to exclude any rows from consideration. Most of the time, however, you will want to narrow your focus to a subset of a table’s rows. Therefore, all the SQL data statements (except the insert statement) include an optional where clause con‐ taining one or more filter conditions used to restrict the number of rows acted on by the SQL statement. Additionally, the select statement includes a having clause in which filter conditions pertaining to grouped data may be included.


### (In)Equality conditions
```sql
mysql> select c.email
    -> from customer c
    -> inner join rental r
    -> on c.customer_id = r.customer_id
    -> where date(r.rental_date) = '2005-06-14'; #or <>  
+---------------------------------------+
| email                                 |
+---------------------------------------+
| CATHERINE.CAMPBELL@sakilacustomer.org |
| JOYCE.EDWARDS@sakilacustomer.org      |
...
| TERRANCE.ROUSH@sakilacustomer.org     |
| TERRENCE.GUNDERSON@sakilacustomer.org |
+---------------------------------------+
16 rows in set (0.02 sec)
```


### Range Conditions
```sql
mysql> select customer_id, rental_date
    -> from rental
    -> where rental_date < '2005-05-25';
+-------------+---------------------+
| customer_id | rental_date         |
+-------------+---------------------+
|         130 | 2005-05-24 22:53:30 |
|         459 | 2005-05-24 22:54:33 |
|         408 | 2005-05-24 23:03:39 |
|         333 | 2005-05-24 23:04:41 |
|         222 | 2005-05-24 23:05:21 |
|         549 | 2005-05-24 23:08:07 |
|         269 | 2005-05-24 23:11:53 |
|         239 | 2005-05-24 23:31:46 |
+-------------+---------------------+
8 rows in set (0.00 sec)
```


### The between operator
```sql
mysql> select customer_id, payment_date, amount
    -> from payment
    -> where amount between 10.0 and 11.99;
+-------------+---------------------+--------+
| customer_id | payment_date        | amount |
+-------------+---------------------+--------+
|           2 | 2005-07-30 13:47:43 |  10.99 |
|           3 | 2005-07-27 20:23:12 |  10.99 |
|          12 | 2005-08-01 06:50:26 |  10.99 |
...
|         591 | 2005-07-07 20:45:51 |  11.99 |
|         592 | 2005-07-06 22:58:31 |  11.99 |
|         595 | 2005-07-31 11:51:46 |  10.99 |
+-------------+---------------------+--------+
114 rows in set (0.01 sec)
```
_Make sure that you specify the lower amount first._

### String ranges
While ranges of dates and numbers are easy to understand, you can also build condi‐ tions that search for ranges of strings, which are a bit harder to visualize. Say, for example, you are searching for customers whose last name falls within a range.
```sql
mysql> select last_name, first_name
    -> from customer
    -> where last_name between 'FA' and 'FR';
+------------+------------+
| last_name  | first_name |
+------------+------------+
| FARNSWORTH | JOHN       |
| FENNELL    | ALEXANDER  |
| FERGUSON   | BERTHA     |
| FERNANDEZ  | MELINDA    |
...
| FOWLER     | JO         |
| FOX        | HOLLY      |
+------------+------------+
18 rows in set (0.00 sec)
```


```sql
mysql> select title, rating
    -> from film
    -> where rating = "G" or rating = "PG";
+---------------------------+--------+
| title                     | rating |
+---------------------------+--------+
| ACADEMY DINOSAUR          | PG     |
| ACE GOLDFINGER            | G      |
| AFFAIR PREJUDICE          | G      |
...
| WORDS HUNTER              | PG     |
| WORST BANGER              | PG     |
| YOUNG LANGUAGE            | G      |
+---------------------------+--------+
372 rows in set (0.00 sec)

#OR 

mysql> select title, rating
    -> from film
    -> where rating in ('G', 'PG');
+---------------------------+--------+
| title                     | rating |
+---------------------------+--------+
| ACADEMY DINOSAUR          | PG     |
| ACE GOLDFINGER            | G      |
| AFFAIR PREJUDICE          | G      |
...
| WORDS HUNTER              | PG     |
| WORST BANGER              | PG     |
| YOUNG LANGUAGE            | G      |
+---------------------------+--------+
372 rows in set (0.00 sec)
```


### Matching Conditions
You have been introduced to conditions that identify an exact string, a range of strings, or a set of strings; the final condition type deals with partial string matches. You may, for example, want to find all customers whose last name begins with Q.
```sql
mysql> select last_name, first_name
    -> from customer
    -> where left(last_name, 1) = 'Q';
+-------------+------------+
| last_name   | first_name |
+-------------+------------+
| QUALLS      | STEPHEN    |
| QUINTANILLA | ROGER      |
| QUIGLEY     | TROY       |
+-------------+------------+
3 rows in set (0.00 sec)
```


Using wildcards When searching for partial string matches, you might be interested in: 
- Strings beginning/ending with a certain character 
- Strings beginning/ending with a substring 
- Strings containing a certain character anywhere within the string 
- Strings containing a substring anywhere within the string 
- Strings with a specific format, regardless of individual characters


| Wildcard character | Matches                                |
| ------------------ | -------------------------------------- |
| __                 | Exactly one character                  |
| %                  | Any number of characters (including 0) |

```sql
mysql> select last_name, first_name
    -> from customer
    -> where last_name like '_A_T%S';
+-----------+------------+
| last_name | first_name |
+-----------+------------+
| MATTHEWS  | ERICA      |
| WALTERS   | CASSANDRA  |
| WATTS     | SHELLY     |
+-----------+------------+
3 rows in set (0.03 sec)
```
#### **Wildcard Analysis:**
1. **`_` (underscore)** – Represents exactly **one** character before "A".
    - Matches: "M**A**TTHEWS", "W**A**LTERS", "W**A**TTS".
    - Doesn't match: "AATTS" (because there's no character before "A").
2. **`A`** – The second character **must be "A"**.
3. **`_` (underscore)** – Represents exactly **one** character between "A" and "T".
    - Matches: "MATTHEWS" (T comes after "A" with "T" in between), "WALTERS", "WATTS".
4. **`T`** – The fourth character **must be "T"**.
5. **`%` (percent)** – Represents **any number of characters (including none)** after "T".
    - Matches: "MATTHEWS", "WALTERS", "WATTS".
6. **`S`** – The last character **must be "S"**.
    - Matches: "MATTHEWS", "WALTERS", "WATTS".
    - Doesn't match: "WALTER" (no "S" at the end).


| Search Expression  | Interpretation |
|--------------------|---------------|
| `F%`              | Strings beginning with **F** |
| `%t`              | Strings ending with **t** |
| `%bas%`           | Strings containing the substring **'bas'** |
| `__t_`            | Four-character strings with **t** in the third position |
| `___-__-____`     | 11-character strings with dashes in the **fourth** and **seventh** positions |

```sql
mysql> select rental_id, customer_id
    -> from rental
    -> where return_date is null;
+-----------+-------------+
| rental_id | customer_id |
+-----------+-------------+
|     11496 |         155 |
|     11541 |         335 |
...
|     15894 |         168 |
|     15966 |         374 |
+-----------+-------------+
183 rows in set (0.01 sec)
```

