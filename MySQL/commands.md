# Create User
[Create User With Permissions](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql)

```sql
    CREATE USER 'username'@'host' IDENTIFIED WITH authentication_plugin BY 'password';
```

### Default Authentication
This authenticates with the reccomended caching_sha2_password plugin.

```sql
    CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```

### Change Authentication
```sql
    ALTER USER 'sammy'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

# Drop User
```sql
    DROP USER 'username'@'localhost';
```

## Grant Privileges
```sql
    GRANT PRIVILEGE ON database.table TO 'username'@'host';
```

```sql
    GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
```

### Grant All Privileges
```sql
    GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
```

### Privileges
|Privilege|Description|
|---------|-----------|
|CREATE   |   |
|ALTER    |   |
|DROP     |   |
|INSERT   |   |
|UPDATE   |   |
|DELETE   |   |
|SELECT   |   |
|REFERENCES|Create foreign keys|
|FLUSH    |   |
|RELOAD   |   |
|WITH GRANT OPTION|Allow user to grant privileges|

## Revoke Privileges
```sql
    REVOKE type_of_permission ON database_name.table_name FROM 'username'@'host';
```

## Show Privileges
```sql
    SHOW GRANTS FOR 'username'@'host';
```
