# Advanced Databases project environment

This is the starting point for CA1, the Database Design Project. The brief is in [BRIEF.md](BRIEF.md).

## Start the database

Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and start it. Then run:

```
docker compose up -d
```

Open phpMyAdmin at <http://localhost:8084>.

Stop the database with `docker compose down`. Your data is kept. Don't add `-v`, because that deletes your databases.

This uses different ports from your lab environment (MySQL on `3307` and phpMyAdmin on `8084`, instead of `3306` and `8080`), so you can run both at the same time.

## Import the starting database

Follow the instructions in your app's README:

- [Goodreads](apps/goodreads/README.md)
- [Steam](apps/steam/README.md)

## Your work

Write your work in these three files, and commit often:

- `schema.sql`: your create table, create index, create role and create user commands
- `queries.sql`: your queries, stored procedures, triggers and views
- `data.sql`: queries to add sample data to your tables

## Connect with DataGrip

If using DataGrip or another MySQL client, connect to the MySQL server with the following:

| Setting  | Value       |
| -------- | ----------- |
| Host     | `127.0.0.1` |
| Port     | `3307`      |
| User     | `root`      |
| Password | `root`      |
