## SQL

#### Using MySQL command line with MAMP

```
/Applications/MAMP/Library/bin/mysql --host=localhost -uroot -proot
```

#### Database management application

MySQL: [Sequel Pro](https://www.sequelpro.com) | [Querious](https://www.araelium.com/querious) | [Navicat](https://www.navicat.com)

PostgreSQL: [pgAdmin](https://www.pgadmin.org/)


#### Database administration

`show databases;` to list all the databases.

`show tables` to list all the tables in a database.

`use databaseName` to switch to a specific database.

`describe tableName` to show schema of the table.

`drop table tableName` to delete a table.

#### Database operations

```sql
/* create a table named todos */
CREATE TABLE todos (
  id            integer PRIMARY KEY AUTO_INCREMENT,
  description   text NOT NULL,
  completed     boolean NOT NULL
);
```

__SELECT__

```sql
/* select the unique first name whose last name is Jon */
/* it's a good practice to always table qualify your columns */
SELECT DISTINCT p.first_name as FirstName
FROM person p
WHERE p.last_name = 'Jon';
```

```sql
/* select contact list that are named Jon or Fritz */
SELECT p.first_name as FirstName, p.last_name as LastName
FROM person p
WHERE p.first_name IN ('Jon', 'Fritz');
```

```sql
/* select people that don't have a last name */
SELECT p.first_name as FirstName, p.last_name as LastName
FROM person p
WHERE p.last_name IS NULL;
```

```sql
/* select name that begins with the letter J */
SELECT p.first_name as FirstName, p.last_name as LastName
FROM person p
WHERE p.last_name LIKE 'J%';
```

```sql
/* order the results */
SELECT p.first_name as FirstName, p.last_name as LastName
FROM person p
WHERE p.last_name LIKE 'J%'
ORDER BY p.last_name;
```

```sql
/* get the total number of time I've contacted my contacts */
SELECT SUM(p.contacted_number)
FROM person p
```

```sql
/* list the number of customers in each country */
SELECT COUNT(customer_id), country
FROM customers
GROUP BY country;
```

```sql
/* list the number of customers in each country, only include countries with more than 5 customers */
SELECT COUNT(customer_id), country
FROM customers
GROUP BY country
HAVING COUNT(customer_id) > 5;
```

__JOIN__

```sql
/* list my contacts' email addresses */
SELECT p.first_name, p.last_name, e.email_address
FROM person p
INNER JOIN
email_address e
ON
p.person_id = e.email_address_person_id
```

__INSERT__

```sql
/* insert a record into a table */
INSERT INTO tableName (description, completed) VALUES ('Go to the store', false);
```

```sql
/* insert multiple values into the 'name' column */
INSERT INTO test_directors (name) VALUES ('Zoe'), ('Dennis'), ('John');
```

__UPDATE__

```sql
/* update a record */
UPDATE contacts SET last_name = 'Dennis' WHERE id = 1;
```

__DELETE__

```sql
/* delete a record */
DELETE FROM contacts WHERE id = 1;
```
