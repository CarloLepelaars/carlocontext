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

`sqlite3.connect(path)` → `database(path)` (WAL on by default). `:memory:` still works. No `commit()` / `close()`.

```python
db = database("app.sqlite")
class User: id:int; name:str; email:str
users = db.create(User)  # table name is snake_case: user
```

- `PRAGMA table_info` / `sqlite_master` → `db.t` (tables), `db.v` (views), `tbl.c` (columns)
- `'user' in db.t` / `tbl.exists()` to test existence
- `db.create(Cls)` names the table `camel2snake(Cls)` (`User` → `user`, `PetFood` → `pet_food`). Keep the return value; do not look up `db.t.User`.

## Schema

`CREATE TABLE` SQL → `db.create(Cls)` (default `pk='id'`) or `tbl.create(...)`.

- `CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT)` → `db.t.users.create(id=int, name=str, pk='id')`
- multi-field pk → `db.create(PetFood, pk=['catid','food'])`
- `DROP TABLE` → `tbl.drop()`
- `CREATE VIEW` → `db.create_view("name", sql, replace=True)` then `db.v.name`
- `ALTER TABLE ... ADD COLUMN` → add the field on `Cls` and `db.create(Cls, transform=True)` (same class name). `insert(..., alter=True)` only if the table has no stored class; otherwise refresh with `tbl.dataclass()`.

## Read

`cursor.execute(...).fetchall()` → call the table, or `db.q(sql, params)` for joins.

- `SELECT * FROM user` → `users()`
- `SELECT * FROM user WHERE id=?` → `users[id]` / `users.get(id, default=None)` (`NotFoundError` if missing)
- `fetchone()` (exactly one) → `users.selectone(where="name = ?", where_args=["Alice"])`
- `WHERE name=?` → `users(where="name = ?", where_args=["Alice"])`
- `ORDER BY` / `LIMIT` / `OFFSET` → `users(order_by="name", limit=10, offset=20)`
- `SELECT name, email` → `users(select="name, email")`
- composite pk → `tbl[a, b]`

For SQL you still need (joins, aggregates), stringify tables/columns in f-strings:

```python
db.q(f"select * from {users} where {users.c.name} like ?", ["A%"])
```

## Write

- `INSERT` → `users.insert(name="Alice")` (dict, dataclass, or kwargs)
- many rows → `users.insert_all(records)` (returns the table)
- `UPDATE` → `users.update(id=1, name="Bob")`
- `DELETE` → `users.delete(1)` (composite: `tbl.delete((a, b))`; returns the table)
- `INSERT OR REPLACE` / `ON CONFLICT` → `users.upsert(...)` or `insert(..., replace=True)`
- get-or-create → `users.selectone(where="email = ?", where_args=[e])` or `insert` on `NotFoundError` (do not use `lookup` to create)

`insert` / `update` / `upsert` return the row.

## Dataclass

`sqlite3.Row` / hand-built `@dataclass` matching the schema → `tbl.dataclass()` (or `db.create(Cls)`, which already stores the class).

```python
AlbumDC = album.dataclass()          # stores album.cls; album() / album[1] return AlbumDC
album_obj = AlbumDC(**row_dict)
src = dataclass_src(AlbumDC)         # from fastcore.xtras; fields are nullable
```

- `conn.row_factory = sqlite3.Row` → `tbl.dataclass()` then `tbl()` / `tbl[pk]`
- `tbl(with_pk=1)` → `[(pk, row), ...]`
- editor / mypy stubs → `create_mod(db, "db_dc")` then `from db_dc import Track` (writes `db_dc.py`; class names are `table.title()`, e.g. `user` → `User`)
- `db.link_dcs(mod)` to attach an imported module back onto the tables

`dataclass()` is dynamic (Jupyter autocomplete only). Use `create_mod` when the editor needs real classes.

## Diagrams

Manual ERD / `PRAGMA foreign_key_list` sketches → `diagram(...)`. Needs [graphviz](https://pypi.org/project/graphviz/).

```python
diagram(db.t['Artist','Album','Track'], size=8, ratio=0.4)   # Jupyter
dot = diagram(db.t['Artist','Album'], render=False)          # DOT string; no render
```

- subset of tables → `db.t['Artist','Album']`
- `neato=True` for a force-directed layout

## Import

`csv.reader` / `csv.DictReader` + `CREATE TABLE` + insert loop → `db.import_file`.

```python
people = db.import_file("people", "id,name,age\n1,Alice,30\n")
```

- argument is str/bytes **content**, not a path string (path strings fail)
- `pk="id"` to set a primary key after import
- `alter=True` to add columns on an existing table
- format inferred (csv/tsv); pass `format="csv"` if needed

## Tenancy

repeated `AND uid=?` on every query → `users.xtra(uid=uid)` (applies to get/call/insert/update/delete). Clear with `users.xtra()`.
