# PostgreSQL

### Online free PostgreSQL&#x20;

* &#x20;[ElephantSQL](https://www.elephantsql.com)

### Installing PSQL in Ubuntu &#x20;

```
# this only install the psql client and not the database

sudo apt-get update
sudo apt-get install postgresql-client


```

### Connecting to Remote PostgreSQL instance in psql client

```
psql -h <IP_Address> -p <port_no> -d <database_name> -U <DB_username> -W

-W option will prompt for password

psql -h 192.168.1.50 -p 5432 -d testdb -U testuser -W


```

### Connecting via Docker

* [How To Install and Run PostgreSQL using Docker ?](https://dev.to/shree\_j/how-to-install-and-run-psql-using-docker-41j2)

### Install and Configure PSQL using Docker:

Run the below command in linux or windows or mac machine from the terminal or command-prompt to pull PSQL from docker-hub.\
\
`docker run --name postgresql-container -p 5432:5432 -e POSTGRES_PASSWORD=somePassword -d postgres`\
\
In the above command replace :\


* Optional - _postgresql-container_ with a preferable container name if necessary.
* _somePassword_ with a password to authenticate and connect to the postgres (in application with connection string as well as the PG-admin viewer).

### Install PG-admin using Docker:

Download the pgAdmin-4 browser version from docker-hub using the following command.\
\
&#x20;`sudo docker run --rm -p 5050:5050 thajeztah/pgadmin4`\


Now manage your postgres from the browser by launching [http://localhost:5050](http://localhost:5050) .

Verify a new container created and running at 0.0.0.0:5432 with the below command.\
`docker ps -a`

```
$ sudo docker pull postgres
$ sudo docker pull postgres:13.2-alpine


Running the Docker image for first time
$ sudo docker run --name postgresql-container -p 5432:5432 -e POSTGRES_PASSWORD=somePassword -d postgres:13.2-alpine


- Where some-postgresh is the name of the container
- Now if one has psql installed locally and want to
connect to postgresql




- Alternatively,
Connect to the docker container first  
$ sudo docker exec -it 35a62e54e14b  bash


- Connecting a "postgre" username
$ psql -h localhost -p 5432 -U postgres -W
ENTER the password asked during the setup at line6
```

### Clean PostgreSQL terminal

```
Keyborad shortcut  : cntrl +L

Exit  : \q
```

### Login a Postgresql database&#x20;

```
psql -U <username>

<prompt for password>
```

### List all the PostgreSQL user

* [Reference](https://www.postgresqltutorial.com/postgresql-list-users/)

```
postgres=# \du

-Relation \ display tables

# \dt

- Display more data for table
# \d+

- Display schema of table 
# \d+ <tablename>
```

### List of All Database &#x20;

```
$ \l
$ \list
```

### Creating a user and Database

```
# create user harshit WITH PASSWORD 'secret';
# create Database people with owner 'harshit';

Now entering datatabase with as the user you created
from the shell

$ psql people harshit
$ <enter ther password>
```

### Creating a Table&#x20;

```
CREATE TABLE pg4e_debug (
  id SERIAL,
  query VARCHAR(4096),
  result VARCHAR(4096),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY(id)
);

SELECT query, result, created_at FROM pg4e_debug;

CREATE TABLE pg4e_result (
  id SERIAL,
  link_id INTEGER UNIQUE,
  score FLOAT,
  title VARCHAR(4096),
  note VARCHAR(4096),
  debug_log VARCHAR(8192),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP
);

CREATE TABLE ages ( 
  name VARCHAR(128), 
  age INTEGER
);
```

### Inserting Data into the table

```
DELETE FROM ages;
INSERT INTO ages (name, age) VALUES ('Corinn', 22);
INSERT INTO ages (name, age) VALUES ('Cully', 22);
INSERT INTO ages (name, age) VALUES ('Ghyll', 14);
INSERT INTO ages (name, age) VALUES ('Karyss', 25);
INSERT INTO ages (name, age) VALUES ('Tracey', 15);
```

### Read the data from Table &#x20;

```
SELECT make.name, model.name
    FROM model
    JOIN make ON model.make_id = make.id
    ORDER BY make.name LIMIT 5;
```

### Update the value from Table&#x20;

```
```

### Deleting the data from Table

```
DELETE FROM users where email='abc@gmailcom'


WARNING

DELETE FROM users // will delete all rown  
```

### Ordering in Table &#x20;



### LIMIT / OFFSET Clause

```
```

### Datatypes

* &#x20;Char(n) - allocate the entire space (faster for small string where the lenght is known) (fixing size helps a lot) (fixe to ASCII char set)
* Varchar(n) - allocate variables amount of space depending on the data length(less space)
* TEXT - pararaph or HTML pages ,generally not used with indexing or sorting and only then limited to prefix
* BYTEA(n) up to 255 bytes
* SMALLINT (-32768,+32768)
* INTEGER (2 Billion)
* BIGINT (10\*\*18 ish)
* REAL (32 bit) 10\*\*38 with 7 digit of approximation
* DOUBLE PRECISION (64 bit) 10\*\*308 with 14 digits of accuracy
* NUMERIC (accuracy , decimal)- specified digits if accuracy and digits after decimal point
* TIMESTAMP -  ' YYYY-MM-DD HH:MM:SS"
* DATE -" YYYY- MM- DD "
* TIME -" HH: MM :SS "

Built in PostgreSQL functions **NOW()**

### Database keys and Indexes in PostgreSQL

```
id SERIAL

UNIQUE (logical key)

```

Auto increment  key for as data comes in &#x20;

Database decides itself what to use for indexing&#x20;

* Trees (B tree)
* Hashes  (Modulus % like)  are used for Indexes&#x20;

**Analysing Query performance** &#x20;

```
explain analyze select id from employee where id=2000;
```



### Loading Data from SQL&#x20;

```
wget https://www.pg4e.com/tools/sql/library.csv
curl -O https://www.pg4e.com/tools/sql/library.csv


CREATE TABLE track_raw
 (title TEXT, artist TEXT, album TEXT,
  count INTEGER, rating INTEGER, len INTEGER);
  
 
  
 \copy track_raw(title,artist,album,count,rating,len) FROM 'library.csv' WITH DELIMITER ',' CSV;
```

### Relational Database

#### Keys

* Primary Key - integer auto increment field&#x20;
* Logical key - What outside world use for lookup&#x20;
* Foreign key - value point to another table

#### Best Practise&#x20;

* &#x20;Never use logical key as primary key
* logical keys can do change , albeit slowly
* Relationships that are based on matching string fields are less efficient then integers

### Database Normalization &#x20;

#### 3 NF&#x20;

* Do not replicate Data , instead reference data  , point at data &#x20;
* use integer for keys and for references
* Add a special "key" column to each table  , which you will make references to .&#x20;

### Joins in SQL

* [https://www.postgresqltutorial.com/postgresql-joins/](https://www.postgresqltutorial.com/postgresql-joins/)

#### One to Many Relations  :

```
CREATE TABLE make (
    id SERIAL,
    name VARCHAR(128) UNIQUE,
    PRIMARY KEY(id)
);

CREATE TABLE model (
  id SERIAL,
  name VARCHAR(128),
  make_id INTEGER REFERENCES make(id) ON DELETE CASCADE,
  PRIMARY KEY(id)
);


make	model
Chevrolet	Cruze Limited
Chevrolet	Cruze Limited Eco
Chevrolet	Cruze Premier
Volvo	V70 FWD
Volvo	V70 R AWD


SELECT make.name, model.name
    FROM model
    JOIN make ON model.make_id = make.id
    ORDER BY make.name LIMIT 5;
```

### Many to Many relationship

![](<.gitbook/assets/image (49).png>)

![](<.gitbook/assets/image (50).png>)

```
CREATE TABLE student (
    id SERIAL,
    name VARCHAR(128) UNIQUE,
    PRIMARY KEY(id)
);

DROP TABLE course CASCADE;
CREATE TABLE course (
    id SERIAL,
    title VARCHAR(128) UNIQUE,
    PRIMARY KEY(id)
);

DROP TABLE roster CASCADE;
CREATE TABLE roster (
    id SERIAL,
    student_id INTEGER REFERENCES student(id) ON DELETE CASCADE,
    course_id INTEGER REFERENCES course(id) ON DELETE CASCADE,
    role INTEGER,
    UNIQUE(student_id, course_id),
    PRIMARY KEY (id)
);


Rhylee, si106, Instructor
Grant, si106, Learner
Kassie, si106, Learner
Rahma, si106, Learner
Raymond, si106, Learner
Xiong, si110, Instructor
Fatiha, si110, Learner
Neeve, si110, Learner
Shelley, si110, Learner
Yaris, si110, Learner
Tristain, si206, Instructor
Lukas, si206, Learner
Reeva, si206, Learner
Shreeram, si206, Learner
Toby, si206, Learner


SELECT student.name, course.title, roster.role
    FROM student 
    JOIN roster ON student.id = roster.student_id
    JOIN course ON roster.course_id = course.id
    ORDER BY course.title, roster.role DESC, student.name;
```

###

### Reference  : &#x20;

* [Zerodha Engineering Blog on optimising PostgreSQL](https://zerodha.tech/blog/working-with-postgresql/)
* [HOW TO INTERPRET POSTGRESQL EXPLAIN ANALYZE OUTPUT](https://www.cybertec-postgresql.com/en/how-to-interpret-postgresql-explain-analyze-output/)
* [Cut Out the Middle Tier: Generating JSON Directly from Postgres](https://blog.crunchydata.com/blog/generating-json-directly-from-postgres)
* [Working Around a Case Where the Postgres Planner Is "Not Very Smart"](https://heap.io/blog/when-the-postgres-planner-is-not-very-smart)



**Video Reference :**

* [Database Indexing Explained (with PostgreSQL)](https://www.youtube.com/watch?v=-qNSXK7s7\_w)
* [Indexing in PostgreSQL vs MySQL](https://www.youtube.com/watch?v=T9n\_-\_oLrbM)

