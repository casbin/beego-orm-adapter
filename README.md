Beego ORM Adapter [![Build Status](https://travis-ci.com/casbin/beego-orm-adapter.svg?branch=master)](https://travis-ci.com/casbin/beego-orm-adapter) [![Coverage Status](https://coveralls.io/repos/github/casbin/beego-orm-adapter/badge.svg?branch=master)](https://coveralls.io/github/casbin/beego-orm-adapter?branch=master) [![Godoc](https://godoc.org/github.com/casbin/beego-orm-adapter?status.svg)](https://godoc.org/github.com/casbin/beego-orm-adapter)
====

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
    a, err := beegoormadapter.NewAdapter("default", "mysql", "mysql_username:mysql_password@tcp(127.0.0.1:3306)/dbname") // Your driver and data source. 
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

## Getting Help

- [Casbin](https://github.com/casbin/casbin)

## License

This project is under Apache 2.0 License. See the [LICENSE](LICENSE) file for the full license text.
