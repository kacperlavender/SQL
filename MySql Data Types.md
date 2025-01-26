## Character Data

In MySQL, character data can be stored in either fixed-length or variable-length strings:
- **Fixed-length (`char`)**: Always consumes the same number of bytes and is right-padded with spaces. For example, `char(20)` stores strings up to 20 characters.
- **Variable-length (`varchar`)**: Only uses the necessary number of bytes, and does not pad with spaces. For example, `varchar(20)` can store strings up to 20 characters in length.
## When to Use:
- Use `char` for columns with fixed-length strings (e.g., state abbreviations).
- Use `varchar` for columns where string lengths vary (e.g., names, emails).

To choose a character set other than the default when defining a column, simply name one of the supported character sets after the type definition, as in: `varchar(20) character set latin1`

With MySQL, you may also set the default character set for your entire database: `create database european_sales character set latin1;`


## Numeric Data

MySQL provides various numeric data types to store integers and real numbers. These can be broadly classified as **exact** and **approximate** types:

### Exact Numeric Types:
- **Integer Types**: Used to store whole numbers.
  - `TINYINT`: 1 byte, range -128 to 127 (signed) or 0 to 255 (unsigned).
  - `SMALLINT`: 2 bytes, range -32,768 to 32,767 (signed).
  - `MEDIUMINT`: 3 bytes, range -8,388,608 to 8,388,607 (signed).
  - `INT` or `INTEGER`: 4 bytes, range -2,147,483,648 to 2,147,483,647 (signed).
  - `BIGINT`: 8 bytes, range -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (signed).

- **Decimal Types**: Used for precise decimal numbers.
  - `DECIMAL(p, s)` or `NUMERIC(p, s)`: Stores numbers with precision `p` (total digits) and scale `s` (digits after the decimal point). Ideal for financial and monetary calculations.

### Approximate Numeric Types:
- Used for floating-point numbers where precision may vary.
  - `FLOAT(p)`: 4 bytes, approximate precision.
  - `DOUBLE` or `REAL`: 8 bytes, higher precision than `FLOAT`.

## When to Use:
- **Integer Types**: Use for whole numbers (e.g., IDs, counters).
- **Decimal Types**: Use for calculations requiring precision (e.g., financial data).
- **Floating-point Types**: Use for scientific calculations or when precision is less critical.



## Temporal Data (dates and/or times)
This type of data is referred to as temporal, and some
examples of temporal data in a database include:
• The future date that a particular event is expected to happen, such as shipping a customer’s order
• The date that a customer’s order was shipped
• The date and time that a user modified a particular row in a table
• An employee’s birth date
• The year corresponding to a row in a yearly_sales fact table in a data warehouse
• The elapsed time needed to complete a wiring harness on an automobile assembly line



### SQL temporal types

| Type      | Default format      | Allowable values                                            |
| --------- | ------------------- | ----------------------------------------------------------- |
| date      | YYYY-MM-DD          | 1000-01-01 to 9999-12-31                                    |
| datetime  | YYYY-MM-DD HH:MI:SS | 1000-01-01 00:00:00 to 9999-12-31 23:59:59.999999           |
| timestamp | YYYY-MM-DD HH:MI:SS | 1970-01-01 00:00:00.000000<br>to 2038-01-18 22:14:07.999999 |
| year      | YYYY                | 1901 to 2155                                                |
| time      | HHH:MI:SS           | −838:59:59.000000 to 838:59:59.000000                       |


## Date format components

| Component | Definition              | Range                         |
| --------- | ----------------------- | ----------------------------- |
| YYYY      | Year, including century | 1000 to 9999                  |
| MM        | Month                   | 01 (January) to 12 (December) |
| DD        | Day                     | 01 to 31                      |
| HH        | Hour                    | 00 to 23                      |
| HHH       | Hours (elapsed)         | -838 to 838                   |
| MI        | Minute                  | 00 to 59                      |
| SS        | Second                  | 00 to 59                      |
