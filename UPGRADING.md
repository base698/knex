## Upgrading to new knex.js versions

### Upgrading to version 0.16.0+

* MSSQL: DB versions older than 2008 are no longer supported, make sure to update your DB;
* PostgreSQL: second argument for `table.datetime(name, [useTz], [precision])` and `table.timestamp(name, [useTz], [precision])` now correctly expects `TRUE` value for `timestamptz` and `FALSE` for `timestamp` type. In previous versions this behaviour was reversed. 

### Upgrading to version 0.15.0+

* Node.js older than 6 is no longer supported, make sure to update your environment;

* MSSQL: Creating a unique index on the table targeted by stored procedures that were created with QUOTED_IDENTIFIER = OFF fails.

You can use this query to identify all affected stored procedures:

```
SELECT name = OBJECT_NAME([object_id]), uses_quoted_identifier
FROM sys.sql_modules
WHERE uses_quoted_identifier = 0;
```

The only known solution is to recreate all stored procedures with QUOTED_IDENTIFIER = OFF

* MariaDB: `mariadb` dialect is no longer supported;

Instead, use "mysql" or "mysql2" dialects.

### Upgrading to version 0.14.4+

* Including schema in tableName parameter in migrations no longer works, so this is invalid:

```
await knex.migrate.latest({
    directory: 'src/services/orders/database/migrations',
    tableName: 'orders.orders_migrations'
})
```

Instead, starting from 0.14.5 you should use new parameter schemaName:

```
await knex.migrate.latest({
    directory: 'src/services/orders/database/migrations',
    tableName: 'orders_migrations',
    schemaName: 'orders'
})
```

