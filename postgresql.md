Source: https://youtu.be/qw--VYLpxG4
timestamp: 1h43m57s
Documentation: https://www.postgresql.org/docs/current/tutorial.html
Data types: https://www.postgresql.org/docs/current/datatype.html

In this course, we learn how to use an interactive shell (psql) because the tutor believes that by using an UI (User Interface) – ie clicking, adding and draging – students don't really learn the logic of databases. Also, SSH-ing into a remote server means you will need to use shell.

- Create databases
- Create tables
- Insert, delete, update/mutate records
- Join tables
- Foreign keys
- Relationships
- Sequences
- Export to csv
- Grouping
- Aggreggation
- Database constraints (assure there is no garbage data)
- Primary keys
________________________________________________________________________________
> SQL Shell (psql)
\! command		Invoke command from current console (cmd.exe)
\?			Help
\l			List all database we have
\c database_name	Connect to database
\d			describes all tables
\d table_name		describes table
\dt			only shows tables
\x 			expanded display
\i FILE			executes commands from file

> LOGICAL OPERATORS:
 AND, OR, >, >=, <, <=, =, <>
 
> KEY WORDS:
 CREATE DATABASE name;		Creates a database
 DROP DATABASE/TABLE name;	Deletes a database/table

 SELECT				grabs column
 SELECT DISTINCT		grabs unique
 FROM
 WHERE condition
 ORDER BY
 OFFSET int			ignores the first int
 LIMIT int			does not print more than int
   	Example
	 SELECT * FROM table WHERE gender = 'Male' AND (country = 'Poland' or country = 'China'); 
	 SELECT * FROM person WHERE gender in ('Bigender', 'Male') ORDER BY gender ASC/DESC OFFSET 5 LIMIT 3;
	 LIMIT 3 = FETCH FIRST 3 ROW ONLY (Standard)
	 
> CONDITIONS
 column IN (a, b, c)			checks whether column in array a, b, c for the register
 column BETWEEN DATE date1 AND date2 	checks whether value in range
 email LIKE '____@google.%'; 		checks whether domain is "google." and e-mail has 4 caracters before @. 
              _ is a place holder, % is a wildcard
 ILIKE 					LIKE but not-case sensititive
> STATISTICS
 GROUP BY 			SELECT country_of_brith, COUNT(*) FROM person GROUP BY country_of_birth
 HAVING 			after GROUP BZ to constraint the output "... HAVING COUNT(*) > 40 ..."
	HAVING takes SQL Aggregating Functions
> AGGREGGATION FUNCTIONS
 MIN	SELECT MIN(price) FROM car;
 MAX	SELECT MAX(price) FROM car; 
 AVG	SELECT ROUND(AVG(price)) FROM car;
 SUM	SELECT make, SUM(price) FROM car GROUP BY make;
> ARITHMETIC OPERATORS
 +, -, *, /, !, %
> 
 COALESCE	substitute if not found
               SELECT COALESCE(email, 'not provided') FROM person;
> TIME DATE
 SELECT NOW()				Date and time
 SELECT NOW()::Date			Casts answer to date
 SELECT NOW() + INTERVAL '1 YEAR' 	Adds 1 year to date
 SELECT EXTRACT(MONTH FROM NOW())	Extract only the month from date
 AGE(NOW(), date_of_birth)		calculates the age between date1 date2
 
> MODIFYING TABLE
 ALTER TABLE person ...
   DROP CONSTRAINT person_pkey;					removes the primary key constraint
   RENAME people;						renames table
   ADD PRIMARY KEY (id);					adds primary key
   ADD CONSTRAINT ...
     constraint_name UNIQUE (email);				makes email unique
     constraint_name CHECK (gender IN ('Male', 'Female'));	creates a check constraint
 DELETE FROM table;						deletes all in the table
 DELETE FROM table WHERE condition;				deletes all where condition evaluates true
 
> UPDATE COMMAND
 UPDATE table SET email = 'omar@gmail.com' WHERE condition;	updates where condition evaluates true
> ON CONFLICT - useful for insert a new register, or update if exists
 statement...
    ON CONFLICT (id) DO NOTHING;				if hindered by a constraing involving id, do nothing
    ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email;	user tries to register again with other e-mail. conflict happens. email updated
> JOINS
 INNER JOIN: matching fields are found in both tables
 LEFT JOIN:  matching fields plus those of left table without matches
> EXPORT A QUERY TO CSV
\copy (SELECT * FROM person LEFT JOIN car ON car.id = person.car_id) TO 'path' DELIMITER ',' CSV HEADER;
________________________________________________________________________________
FEW CONCEPTS:
Database = store, manipulate, retrieve (typically stored inside a computer server) 
Postgres = database engine with high conformance with the latest SQL-standard
SQL 	 = Structured Query Language for databases (started in 1974) Object-relational database management system
SSH	 = Secure Shell (a cryptographed network protocol for the secure operation of network services over unsecured networks)
________________________________________________________________________________
1. DOWNLOAD PostgreSQL
The most advanced open source relational database, in development for over 30 years. Many new startups use PostgreSQL instead of Oracle because there is no license.
https://www.postgresql.org/
Download > Windows > Download the installer
Download the latest version

- PostgreSQL server
- pgAdmin (a graphical tool for managing and developing your databases)
- StackBuilder (a package manager that can be used to download and install additional PostgreSQL tools and drivers. Stackbuilder includes management, integration, migration, replication, geospatial, connectors and other tools.)

Data directory [default] C:\Program Files\PostgreSQL\XX\data

Password: admin; Port: 5432

> Wanna work with geospatial data .gpkg, .shp?
  Add PostGIS Bundle  3.2.1 for PostgreSQL x64 14 via StackBuilder (postgis_32_sample <- database name
  a series of Environmental Variables will be set
  PROJ
  GDAL_DATA (env variable)
________________________________________________________________________________
2. CONNECT TO THE DATABASE (two ways)
> 1ST OPTION [via "SQL Shell (psql)"]-------------------------------------------
It prompts to enter to a server. SQL Databases use the Service-Framework to allow
several users accessing the Database. A file can only have one user writing it at a time. Others may solely access read-only

If connecting to a remote server, type the "Server url"
Else,
Server: localhost
Database: postgres
Port: 5432
Password for user postgres: admin

Encoding error: <https://stackoverflow.com/questions/20794035/postgresql-warning-console-code-page-437-differs-from-windows-code-page-125>

> 2ND OPTION [psql in the Env Variables] ---------------------------------------
One can also connect to a database calling "psql" in the Command Prompt (psql -h -p -U -w -W)

Example:
psql -h localhost -p 5432 -U postgres database_name
________________________________________________________________________________
> CREATE A TABLE
CREATE TABLE table_name (
    Column name + data type + constraints if any
);

Data types -> postgresql.org/docs/14/datatype.html
	      BIGSERIAL is zB a signed int that auto-increment

Example:
CREATE TABLE person (
    id int NOT NULL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    gender VARCHAR(6) NOT NULL,
    data_of_birth DATE NOT NULL,
    email VARCHAR(150) <- "no NOT NULL constraint because some might not have an email"
);
--------------------------------------------------------------------------------
> INSERT RECORDS INTO TABLE
INSERT INTO table_name (
    first_name,
    last_name,
    gender,
    date_of_birth)
VALUES ('Anne', 'Smith', 'FEMALE', DATE '1988-01-31');
--------------------------------------------------------------------------------
> Running several commands from a file "\i path"
On Windows, the path must have "/" instead of "\"

\i 'C:/juliano/sapotec/Memoria/2022 08 01/SQL (Thomas)/Dados_brutos/SQL_commands.sql'

> DATA TYPE CONVERSION IN STATEMENT
    cast('5' as numeric) > 4;
or, alternatively
    '5'::numeric > 4

[ERRORS] -----------------------------------------------------------------------------
> pgadmin4 : postgresql application server could not be contacted. [pgAdmin4 err]
delete content inside
C:\Users\%USERNAME%\AppData\Roaming\pgAdmin\sessions

> postgresql permission denied while importing from file:
Windows does not accept backslash '\'; change em to '/'
________________________________________________________________________________
SQL: <https://youtu.be/zsjvFFKOm3c>
> Relational database organizes data in tables. Columns contain attributes or types of data.
Each row is a record with its unique id, the Primary Key. Relations are stablished associating keys, 
storing the Foreign Key which are the actual Primary Key in another table. 

> Data is structured in its smallest normal form (normalized) to eliminate duplication and redundancy

> The syntax is comprised of several key parts. A statement reads or writes/mutates the database
SELECT <- grab columns		table and column names are known as identifiers
FROM   <- table name
WHERE  <- filters, only return records where the Predicate evaluates to True
INNER/LEFT/RIGHT JOIN <- connect data between tables by matching Primary and Foreign keys

> Example: are left table fields not found in right table, returns 'N.A.'
--------------------------------------------------------------------------------
SELECT [1 Poços e sondagens].ID_poco, 
             IIF(ISNULL([Dados amostragem IC].[Nível d’água (m)]),
                 'N.A.',
                 [Dados amostragem IC].[Nível d’água (m)])
                 AS [NA IC],
             IIF(ISNULL([Dados amostragem ID_AR].[Nível d’água (m)]),
                 'N.A.',
                 [Dados amostragem ID_AR].[Nível d’água (m)])
                 AS [NA ID AR],
             IIF(ISNULL([Dados amostragem AC_2].[Nível d’água (m)]),
                 'N.A.',
                 [Dados amostragem AC_2].[Nível d’água (m)])
                 AS [NA AC]

FROM (([1 Poços e sondagens] LEFT JOIN [Dados amostragem IC] ON [1 Poços e sondagens].ID_poco = [Dados amostragem IC].Poço) LEFT JOIN [Dados amostragem ID_AR] ON [1 Poços e sondagens].ID_poco = [Dados amostragem ID_AR].Poço) LEFT JOIN [Dados amostragem AC_2] ON [1 Poços e sondagens].ID_poco = [Dados amostragem AC_2].Poço

WHERE ((([1 Poços e sondagens].ID_poco) Is Not Null And ([1 Poços e sondagens].ID_poco) Not Like 'Po*'));
--------------------------------------------------------------------------------
________________________________________________________________________________
Access:
type mismatch in expression <- quer dizer que os campos com os quais está tentando dar JOIN são de diferentes tipos. 
Para arrumar, abra a tabela, clique sobre a coluna desses campos e na aba "Campo" altere o tipo para ser o mesmo nas duas tabelas
________________________________________________________________________________
Further videos:
https://youtu.be/17AZQ2-5Rrk
https://youtu.be/eddcoyLtqqs
https://youtu.be/gC0z4miZmtQ
https://youtu.be/7OUxHAhqMv8
https://youtu.be/m6jLnEOoZvw
https://youtu.be/0yMXbew4tsA
https://youtu.be/QpUZd2TQ0H0
https://youtu.be/gUKrlBNYxlg
https://youtu.be/t8-BQjWJFKw
https://youtu.be/F3AkNXiSv50
https://youtu.be/Vfq0Mje1z-E
https://youtu.be/vlqLJtZW3AA
https://youtu.be/nqRjg5SQJiw
https://youtu.be/P-iHxxj7heE

Still cant see tables in QGIS:
https://gis.stackexchange.com/questions/285543/tables-do-not-show-in-qgis-postgis
