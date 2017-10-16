## MySQL

#### Using MySQL command line with MAMP

```
/Applications/MAMP/Library/bin/mysql --host=localhost -uroot -proot
```

Or you can use a MySQL management application, such as [Sequel Pro](https://www.sequelpro.com), [Querious](https://www.araelium.com/querious) and [Navicat](https://www.navicat.com).

#### MySQL administration

`show databases;` to list all the databases.

`show tables` to list all the tables in a database.

`use databaseName` to switch to a specific database.

`describe tableName` to show schema of the table.

`drop table tableName` to delete a table.

#### Table operations

```
// create a table
create table todos (
  id integer    PRIMARY KEY AUTO_INCREMENT,
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

