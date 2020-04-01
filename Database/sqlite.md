# SQLite

- [SQLite](#sqlite)
  - [Exploring Databases](#exploring-databases)
  - [SELECT](#select)
  - [DELETE](#delete)
  - [Creating Tables](#creating-tables)
  - [Datatypes](#datatypes)
  - [Deleting Tables](#deleting-tables)
  - [Interfacing with Python](#interfacing-with-python)

Most SQL databases work with a client/server model. Contrary to this, SQLite databases operate from single, cross-platform, portable files. They can be stored on various storage devices and can be transferred across different computers.

## Exploring Databases

Creating a database from scratch and reading a pre-existing database file works the same way:

    sqlite3 mydatabse.db

**Meta Commands** in SQLite always start with a dot (`.`), and don't require a closing semicolon (`;`).

| Action                    | Meta Command             |
| :------------------------ | :----------------------- |
| Help                      | `.help`                  |
| Display tables            | `.tables`                |
| Display table schema      | `.schema <table>`        |
| Quit                      | `.quit`                  |
| Set mode for output table | `.mode`                  |
| Import data to a table    | `.import <file> <table>` |

## SELECT

``` sql
-- Select everything
SELECT * FROM my_table;

-- Select columns
SELECT column_a, column_b, column_d FROM my_table;

-- Select rows
SELECT * FROM my_table WHERE column_a = x;

-- Select top 5 rows
SELECT * FROM my_table LIMIT 5;
```

## DELETE

Delete data from a table:

``` sql
DELETE FROM my_table WHERE column_a = x;
```

## Creating Tables

SQLite uses **Manifest Typing**.
Manifest Typing releases many restrictions on the type of value that can be entered for a particular field. This allows you to enter any value of any datatype into a column, irrespective of the declared type of the column (except for INTEGER PRIMARY KEY).

``` sql
CREATE TABLE comments ( 
	post_id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
	name TEXT NOT NULL, 
	email TEXT NOT NULL, 
	website_url TEXT NULL, 
	comment TEXT NOT NULL );
```

You can also create a table from a csv file. If the table that you are importing into already exists, then the first row of the csv is interpreted as data. If it does not exist, it is created, and the first row of the csv is interpreted as a header.

```sql
-- Set import mode to csv
.mode csv
.import my_file.csv my_table
```

## Datatypes

There are five datatypes in SQLite3:

- NULL
- INTEGER
- REAL
- TEXT
- BLOB

SQLite3 does not have Boolean or Date and Time datatypes. Booleans are commonly stored as integers (1 or 0) and Date/time is stored as either text (ISO8601 strings), real numbers (Julian day numbers), or integers (UNIX time), using the built-in  date and time functions.

## Deleting Tables

Delete a table if it exists:

```sql
DROP TABLE IF EXISTS my_table
```

## Interfacing with Python

We can use the sqlite3 Python module to connect to a database, and read a table into a Pandas dataframe. 

```python
# Connect to SQLite database
conn = sqlite3.connect("my_database.db")
c = conn.cursor()

# Read table into Pandas
df = pd.read_sql("select * from my_table", conn)
```