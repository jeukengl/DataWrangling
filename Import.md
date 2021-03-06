[Prereqs](Prereqs.md#installing-mysql) | [Importing Data](Import.md#import) | [Basics Queries](BasicQueries.md#basic) |  [Advanced](Advanced.md#advanced) | [Programming with Databases](Programming.md#programming)

# Import

Let's examine and then extend the import.sql to import more data!

### Getting all the files imported

Each developer has a csv file, but we want to be able to load all of them.
One simple strategy is to combine these files together:

```bash
cat BlazeData_Dev_* > AllEvents.csv
```

However, that will add the header line multiple lines. But this will work.

```bash
awk FNR-1 BlazeData_Dev_*.csv > AllEvents.csv
```

Using a bash script is an [alternative approach](https://stackoverflow.com/a/8539153/547112).

### Adding a new import.

This makes sure we delete old data before importing fresh.

```sql
DROP TABLE IF EXISTS Events;
```

This creates a new table. The syntax describes the table, and then a list of variable names and types. Depending on your database manager, the column types can vary.

```sql
CREATE TABLE Events
(
    eventTime TIMESTAMP, 
    userId int,
    eventType VARCHAR(255)
);
```

This code will load data from the CSV into the table. 
This basically maps the columns that appear in the CSV file to the columns in the database table.

```sql
LOAD DATA LOCAL INFILE 'data/AllEvents.csv' 
INTO TABLE Events
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n' -- Remember to use right line endings for your system.
(eventTime, userId, eventType);
SHOW warnings;
```

## Practice

* Import messages
