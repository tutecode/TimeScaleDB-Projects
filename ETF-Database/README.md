# Build an ETF Database with PostgreSQL and Docker

We get up and running with TimescaleDB using Docker. We discuss the first simple project we will build - an ETF database that tracks the ARK holdings over time.

ARK Invest (https://ark-invest.com/)

Project from Part Time Larry: https://youtu.be/4dwCjaX4QUE

Prerequisites:

- Docker, Curso Práctico para principiantes (desde Linux): https://youtu.be/NVvZNmfqg6M
- Full Stack Trading App Tutorial – Part 02 – Intro to SQL: https://hackingthemarkets.com/full-stack-trading-app-tutorial-part-02-intro-to-sql/t

## Install Docker (See Docker-for-Linux)

## Install TimescaleDB for Docker

Link: https://bit.ly/3zynqCh

```bash
docker run -d --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=password timescale/timescaledb:latest-pg12
```

- See Docker containers

```bash
docker images
```

- Run

```bash
docker exec -it timescaledb bash
```

- Go to database

```bash
psql -U postgres
```

- the \d commands display all objects that are visible in the current schema search path

```bash
\d
```

- List the names, owners, character set encodings, and access privileges of all the databases in the server. If + is appended to the command name, database sizes, default tablespaces, and descriptions are also displayed. (Size information is only available for databases that the current user can connect to.)

```bash
\l
```

- Exit database

```bash
exit
```

## Install TablePlus

Link: https://tableplus.com/blog/2019/10/tableplus-linux-installation.html

- Run TablePlus from Desktop

### Create database on Postgres

- First run

```bash
psql -U postgres
```

- Create database (db)

```bash
create database etfdb;
```

```bash
CREATE DATABASE
postgres=#
```

- Connect to database (db)

```bash
\c etfdb
```

```bash
You are now connected to database "etfdb" as user "postgres".
```
