## PostgreSQL

#### Create a table

```
CREATE TABLE timezone (
  id_timezone         serial PRIMARY KEY,
  timezone_offset     int NOT NULL,
  abbr                varchar(40) NOT NULL,
  name                varchar(60) NOT NULL,
  display_name        varchar(100) NOT NULL,
  modified timestamp  with time zone NULL,
  created timestamp   with time zone DEFAULT NOW() NOT NULL,
  deleted timestamp   with time zone NULL,
);
```

#### Insert values into a table

```
// insert multiple values into the name column
INSERT INTO test_directors (name) VALUES ('Zoe'), ('Dennis'), ('John');
```
