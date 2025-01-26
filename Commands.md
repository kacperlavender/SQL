To change the database in MySQL, you use the command: `USE database_name;`

This will switch you to the `sakila` database, and you can execute queries within it. If you want to check which database is currently selected, you can use the following command: `SELECT DATABASE();`

If you want to see the tables available in your database, you can use the show tables command, as in:
`show tables;`

If you want to look at the columns in a table, you can use the describe command. `desc customer;`

If you are just looking to retrieve non-duplicate rows, you can use command: `SELECT DISTINCT actor_id FROM film_actor ORDER BY actor_id`

