# xk6-sql-driver-clickhouse

Database driver extension for [xk6-sql](https://github.com/grafana/xk6-sql) k6 extension to support ClickHouse database.

## Example

```JavaScript file=examples/example.js
import sql from "k6/x/sql";
import driver from "k6/x/sql/driver/clickhouse";

const db = sql.open(driver, "clickhouse://127.0.0.1:19000");

export function setup() {
  db.exec(`CREATE TABLE IF NOT EXISTS hits_by_user_url
  (
      UserID UInt32,
      URL String,
      EventTime DateTime
  )
  ENGINE = MergeTree
  PRIMARY KEY (UserID, URL)
  ORDER BY (UserID, URL, EventTime)
  SETTINGS index_granularity = 8192, index_granularity_bytes = 0;`);
}

export function teardown() {
  db.close();
}

export default function () {
  db.exec(`INSERT INTO hits_by_user_url 
    (UserID, URL, EventTime)
    SELECT * FROM generateRandom('UserID UInt32, URL String, EventTime DateTime')
    LIMIT 100;`);
}
```

## Usage

Check the [xk6-sql documentation](https://github.com/grafana/xk6-sql) on how to use this database driver.

---

> [!IMPORTANT]
>
> ## TODO
>
> This is a repository template for creating an xk6-sql driver repository.
>
> After creating the driver repository, remember the following:
>
> - replace `ClickHouse` with the database name in:
>   -  `README.md`
> - replace `clickhouse` with the database driver name in:
>   - `README.md`
>   - `register.go`
>   - `register_test.go`
>   - `examples/example.js`
> - update SQL statements to match the database's SQL dialect in:
>   -  `testdata/script.js`
>   -  `examples/example.js`
>   -  `README.md`
> - change the go package and module name:
>   - `go.mod`
>   - `register.go`
>   - `register_test.go`
>   - `Makefile`
> - remove this alert blockquote from `README.md`

