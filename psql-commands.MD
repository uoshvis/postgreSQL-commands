# PostgreSQL configuration and commands


### Main configuration

`/etc/postgresql/11/main/postgresql.conf`

`sudo service postgresql status`

`sudo service postgresql restart`


#### Hostname and port

Default port: `5432`

Default hostname: `localhost:5432`

psql:

```
SELECT *
FROM pg_settings
WHERE name = 'port';
```

`\conninfo`

Linux:

`sudo netstat -plunt |grep postgres
`

### Working with Python
To get Python working with Postgres, you will need to install the “psycopg2” module. However, you must first have pg_config installed on your OS

`which pg_config`

### Help

`psql --help`

### Create user, database, adding access

Default user: **postgres** has superadmin access to PostgreSQL

#### Admin mode

`sudo -u postgres psql`

Getting help:

`sudo -u postgres psql --help`

####Create password for admin 

`sudo passwd postgres`

#### Login as admin

`su posgres`

#### Create Database

`sudo -u postgres createdb <dbname>`

#### Giving the user a password

```
$ sudo -u postgres psql
psql=# ALTER USER <username> WITH ENCRYPTED PASSWORD '<password>';
```

#### Granting privileges on database

`psql=# GRANT ALL PRIVILEGES ON DATABASE <dbname> TO <username> ;
`


**Via psql:**

```
CREATE DATABASE yourdbname;
CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
GRANT ALL PRIVILEGES ON DATABASE yourdbname TO youruser;

```

### Postgres login

`psql -d mydb -U myuser`

Log into a Postgres database on a server named `myhost`:

`psql -h myhost -d mydb -U myuser`

Prompt for a password:

```
psql -U myuser -W
psql -d mydb -U myuser -W
psql -h myhost -d mydb -U myuser -W
```


## psql list commands


|             Listing                       |  Command  | Argument |
|:-----------------------------------------:|:---------:|:--------:|
| Table, index, view, or sequence           | \d        | name     |
| All user accounts (or roles)              | \du       | name     |
| All user accounts (or roles) + description| \du+       | name     |
| Tables or table with size desc.           | \dt or \dt+| name     |
| Indexes                                   | \di       | name     |
| Sequences                                 | \ds       | name     |
| Views                                     | \dv       | name     |
| Permissions                               | \dp or \z | name     |
| System tables                             | \dS       | name     |
| Large objects                             | \dl       | name     |
| Types                                     | \dT       | name     |
| Functions                                 | \df       | name     |
| Operators                                 | \do       | name     |
| Aggregates                                | \da       | name     |
| Comments                                  | \dd       | name     |
| Databases                                 | \l        | name     |

#### Listing users using SQL statement

```
SELECT usename AS role_name,
  CASE 
     WHEN usesuper AND usecreatedb THEN 
	   CAST('superuser, create database' AS pg_catalog.text)
     WHEN usesuper THEN 
	    CAST('superuser' AS pg_catalog.text)
     WHEN usecreatedb THEN 
	    CAST('create database' AS pg_catalog.text)
     ELSE 
	    CAST('' AS pg_catalog.text)
  END role_attributes
FROM pg_catalog.pg_user
ORDER BY role_name desc;

```

### Change password

`sudo -u user_name psql db_name`

`ALTER USER user_name WITH PASSWORD 'new_password';
`
### Connect to db

`sudo -u postgres psql -h localhost -p 5432 -U <user> <dbname>`

`\c <databasename>`


#### Connect with role

`\c <dbname> <rolename>`

### disconnect from db

`\q`


### Security

[postgresql security](https://www.enterprisedb.com/blog/how-to-secure-postgresql-security-hardening-best-practices-checklist-tips-encryption-authentication-vulnerabilities)


### GUI

pgAdmin

### Quit

`\q`

### psql Help

`\?`
