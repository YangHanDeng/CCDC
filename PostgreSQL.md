1.  Log into psql

	if you're in user that has role in psql
	>   **psql -h \[localhost\] -U \[username\] -p \[port\] \[database name\]**
	>
	>   1.  -h is the host to connect to
	>   2.  -U is the user to connect as
    
	if not
	>   **sudo -u postgres -i**
	>
	>   **psql -h \[localhost\] -U \[username\] -p \[port\] \[database name\]**
    >
    >  **note: postgres is default psql's superuser
	>
	or in 1 line:
	>   **sudo -u postgres psql -h \[localhost\] -U \[username\] -p \[port\] \[database name\]**
    note:
	1. There is no default password - login uses either ident auth (see 3) or peer auth
	2. Default port is 5432
    
2.  Some useful commands
    1.  \\password \[Username\] change the user’s password. default: current user
    2.  \\? List all available commands
    3.  \\d List all tables in a database
        1.  Includes schema, name, type and owner
        2.  \\d+ lists more info
    4.  \\d \[tableName\] Describes a table.
        1.  Does not print the actual table
    5.  \\l (lowercase L) List all databases
    6.  \\dn List all schemas
    7.  \\df display all functions of a database
    8.  \\du List all users
    9.  \\c \[databaseName\] move to a new database
    10. \\q quit psql
    11. \\e text editor!
3.  Ident
    1.  Psql uses an ident server to get the client’s os username and uses that to determine what databases it can access.
    	Namely, it maps os user to postgres' user.
    2.  Uses TCP/IP
4.  Backup
	1. backup database
    
        0. login to psql's superuser
        > **sudo -u postgres -i**
        1. backup database
        > **pg_dump dbname > dumpfile**
        2. restore database
        >  1.  create database
        > 
        > 		**psql**
        > 
        >		**create database dbname**
        > 
        >  2.  dump file to db you just created
        >
        > 		**\q**
        > 
        > 		**psql dbname < dumpfile**
	2. backup entire PostgreSQL cluster
    	1. backup
        > pg_dumpall > dumpfile
        2. restored
        > psql -f dumpfile postgres
	3. backup large databases
    	1. backup with compression
        > pg_dump dbname | gzip > filename.gz
        2. restore
        > gunzip -c filename.gz | psql dbname
5. Role (User) Priviledges
    * role is equivalent to user
	1. list all roles(users) including their attributes(privilege)
    > **\du**
    >
    > or
    >
    > **SELECT rolname FROM pg_roles;**
    2. create/ drop roles
    > **CREATE ROLE name;**
    >
    > **DROP ROLE name;**
	3. alter privilege
    > **ALTER ROLE \<name\> \[ WITH \] OPTION \[ ... \]**
    >
    > * for options:    
    > [<u>https://www.postgresql.org/docs/current/sql-alterrole.html</u>](https://www.postgresql.org/docs/current/sql-alterrole.html)
