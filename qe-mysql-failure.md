---
layout: doc
sublayout: issue
title: Quay Enterprise fails to install or update on MySQL 5.7
reviewed: 2017-04-26T17:30:00-04:00
---
Attempting to install or update the Quay Enterprise on MySQL version 5.7 fails with the following
error:

```sh
sqlalchemy.exc.OperationalError: (pymysql.err.OperationalError) (1142, u"SELECT command denied to user 'coreosuser'@'172.17.42.1' for table 'session_variables'") [SQL: "SHOW VARIABLES LIKE 'sql_mode'"]
*** /etc/my_init.d/runmigration.sh failed with status 1
```

### Cause

This error is due to a recent (undocumented) change by Oracle to MySQL to make access to the `session_variables` table restricted
to certain users only.

### Workarounds

Currently, the possible workarounds are:

- Use a root user when connecting from QE to MySQL
- Set `show_compatibility_56 = on` in `/etc/my.cnf`
- If in MySQL 5.7.8: `GRANT` access to that table to your QE database user:

```
GRANT SELECT ON performance_schema.global_variables TO 'coreosuser'
```

- Downgrade to MySQL 5.6