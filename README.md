Beego ORM Adapter [![Go](https://github.com/casbin/beego-orm-adapter/actions/workflows/ci.yml/badge.svg)](https://github.com/casbin/beego-orm-adapter/actions/workflows/ci.yml) [![Coverage Status](https://coveralls.io/repos/github/casbin/beego-orm-adapter/badge.svg?branch=master)](https://coveralls.io/github/casbin/beego-orm-adapter?branch=master) [![Godoc](https://godoc.org/github.com/casbin/beego-orm-adapter?status.svg)](https://godoc.org/github.com/casbin/beego-orm-adapter)
====

#### Note: the last version that supports Beego v1 is: https://github.com/casbin/beego-orm-adapter/releases/tag/v2.0.2 . Beego v2 is supported starting from v3.0.0

Beego ORM Adapter is the [Beego ORM](https://beego.me/docs/mvc/model/overview.md) adapter for [Casbin](https://github.com/casbin/casbin). With this library, Casbin can load policy from Beego ORM supported database or save policy to it.

Based on [Beego ORM Support](https://beego.me/docs/mvc/model/overview.md), The current supported databases are:

- MySQL: [github.com/go-sql-driver/mysql](https://github.com/go-sql-driver/mysql)
- PostgreSQL: [github.com/lib/pq](https://github.com/lib/pq)
- Sqlite3: [github.com/mattn/go-sqlite3](https://github.com/mattn/go-sqlite3)

## Installation
```bash
go get github.com/casbin/beego-orm-adapter/v2
```

## Simple Example

```go
package main

import (
    "log"

    beegoormadapter "github.com/casbin/beego-orm-adapter/v3"
    "github.com/casbin/casbin/v2"
    _ "github.com/go-sql-driver/mysql"
)

func main() {
    // Initialize a Beego ORM adapter and use it in a Casbin enforcer:
    a, err := beegoormadapter.NewAdapter("casbin", "mysql", "mysql_username:mysql_password@tcp(127.0.0.1:3306)/dbname") // Your driver and data source. 
    if err != nil {
        log.Fatalln(err)
    }

    e, err := casbin.NewEnforcer("examples/rbac_model.conf", a)
    if err != nil {
        log.Fatalln(err)
    }
    // Check the permission.
    e.Enforce("alice", "data1", "read")

    // Modify the policy.
    // e.AddPolicy(...)
    // e.RemovePolicy(...)

    // Save the policy back to DB.
    e.SavePolicy()
}
```

## Custom Table Name

`Beego ORM` using `TableNameI` interface to customize the table name

reference: [custom-table-name](https://github.com/beego/beedoc/blob/master/en-US/mvc/model/models.md#custom-table-name)

You can customize the name of the table by `NewAdapterWithTableName`, and query the custom table name through `GetFullTableName`

```golang
func testCustomTableName(t *testing.T, a *Adapter, expectTableName string) {
    actualTableName := a.GetFullTableName()
    if actualTableName != expectTableName {
        t.Error("actual table name: ", a.GetFullTableName(), ", supposed to be ", expectTableName)
    }
}

func TestAdapterCustomTableName(t *testing.T) {
    a, err := NewAdapterWithTableName("customtablename", "mysql", "root:@tcp(127.0.0.1:3306)/casbin_test", "my_casbin", "prefix_")
    if err != nil {
        t.Fatal(err)
    }
    testCustomTableName(t, a, "prefix_my_casbin")
    testSaveLoad(t, a)
    testAutoSave(t, a)
}
```

**Note:** Currently, only one custom table name is supported, 
and no other table name should be used after the first use of `NewAdapterWithTableName`



## Getting Help

- [Casbin](https://github.com/casbin/casbin)

## License

This project is under Apache 2.0 License. See the [LICENSE](LICENSE) file for the full license text.
