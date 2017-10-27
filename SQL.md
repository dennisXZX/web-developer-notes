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

```
// create a table
CREATE TABLE todos (
  id            integer PRIMARY KEY AUTO_INCREMENT,
  description   text NOT NULL,
  completed     boolean NOT NULL
);
```

```
// insert a record into a table
INSERT INTO tableName (
  description, 
  completed
) VALUES (
  'Go to the store',
  false
)
```

```
// insert multiple values into the 'name' column
INSERT INTO test_directors (name) VALUES ('Zoe'), ('Dennis'), ('John');
```
