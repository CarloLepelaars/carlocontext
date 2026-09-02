---
name: fastlite
description: >
  Apply fastlite substitutions when writing, editing, or reviewing Python.
  Use whenever the task involves sqlite, sqlite3, SQL tables, or fastlite.
when-to-use: sqlite, sqlite3, fastlite, database
---

# fastlite

When writing Python that talks to SQLite, prefer fastlite over `sqlite3` and raw SQL. Use `from fastlite import *`. Apply these substitutions on write and review.

## Connect

`sqlite3.connect(path)` → `database(path)` (WAL on by default). `:memory:` still works.

```python
db = database("app.sqlite")
users = db.t.User
```

- `PRAGMA table_info` / `sqlite_master` → `db.t` (tables), `db.v` (views), `tbl.c` (columns)
- `'User' in db.t` to test existence

## Schema

`CREATE TABLE` SQL → `db.create(Cls)` or `tbl.create(...)`.

```python
class User: id:int; name:str; email:str
users = db.create(User)
```

- `CREATE TABLE t (id INTEGER PRIMARY KEY, name TEXT)` → `db.t.t.create(id=int, name=str, pk='id')`
- multi-field pk → `db.create(PetFood, pk=['catid','food'])`
- `DROP TABLE` → `tbl.drop()`
- `CREATE VIEW` → `db.create_view("Name", sql, replace=True)` then `db.v.Name`
- `ALTER TABLE ... ADD COLUMN` → `tbl.insert(..., alter=True)`
- csv + inserts → `db.import_file("people", csv_or_path)`

## Read

`cursor.execute(...).fetchall()` → call the table, or `db.q(sql)` for joins.

- `SELECT * FROM users` → `users()`
- `SELECT * FROM users WHERE id=?` → `users[id]` / `users.get(id, default=None)`
- `WHERE name=?` → `users(where="name = ?", where_args=["Alice"])`
- `ORDER BY` / `LIMIT` / `OFFSET` → `users(order_by="name", limit=10, offset=20)`
- `SELECT name, email` → `users(select="name, email")`
- composite pk → `tbl[a, b]`

For SQL you still need (joins, aggregates), stringify tables/columns in f-strings and run `db.q`:

```python
db.q(f"select * from {users} where {users.c.name} like 'A%'")
```

## Write

- `INSERT` → `users.insert(name="Alice")` (dict, dataclass, or kwargs)
- many rows → `users.insert_all(records)`
- `UPDATE` → `users.update(id=1, name="Bob")`
- `DELETE` → `users.delete(1)` (composite: `tbl.delete((a, b))`)
- `INSERT OR REPLACE` / `ON CONFLICT` → `users.upsert(...)`
- get-or-create → `users.lookup({"email": e}, extra_values={"name": n})`

Inserts/updates return the row.

## Types / tenancy

- `row_factory = sqlite3.Row` → `users.dataclass()` so `users()` / `users[1]` return dataclasses
- editor types → `create_mod(db, "db_dc")` then `from db_dc import User`
- repeated `AND user_id=?` on every query → `users.xtra(user_id=uid)` (applies to get/insert/update/delete)

Do not open `sqlite3` connections or write `CREATE TABLE` SQL when `db.create` / `tbl.create` will do.
