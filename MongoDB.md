## MongoDB

#### Install MongoDB on Mac

Use Homebrew to install MongoDB formula.

```
// update the Homebrew itself
brew update
brew install mongodb
```

This will install MongoDB in the following location (which can be verified by `brew info mongodb`).

```
/usr/local/var/Cellar/mongodb/
```

Now we need to create a folder in `/data/db` (default location) for MongoDB to store data.

```
// create nested directories using the `-p` flag
$ sudo mkdir -p /data/db
$ whoami
dennisxiao
$ sudo chown dennisxiao /data/db
```

Now you can start the MongoDB by running `mongod` in terminal.

```
[initandlisten] waiting for connections on port 27017
```

In another terminal you can use `mongo` to connect to the database.

#### Fundamentals

- `show dbs` to show all the database
- `use <database_name>` to use a database or create one if it doesn't exist
- `show collections` to show all collections
- `db.students.insert({ "name": "Jose", "mark": 100 })` to create a students collection and insert an entry
- `db.students.find({})` to list all entries in students collection
- `db.students.remove({ "price": 999 })` to remove entries that has a price value of 999

