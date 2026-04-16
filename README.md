# Learn Programming in Go (golang): Webserver with PostgreSQL

## Setup

### Introduction

[Repository](https://github.com/jagottsicher/myGoWebserver)

### Postgres

#### Setup a PostgreSQL Database server in a container

```sh
sudo docker run --name postgres-server -p 6543:5432 -e POSTGRES_PASSWORD=supersecretpassword -d postgres
Port 6543 => 5432
psql -h localhost -U postgres -d postgres -p 6543
```

#### Setup for a table to store some data

```sql
CREATE SEQUENCE Posts_id_seq;

CREATE TABLE public.posts
(
	id integer NOT NULL DEFAULT nextval('Posts_id_seq'::regclass),
	title text COLLATE pg_catalog."default",
	body text COLLATE pg_catalog."default",
	CONSTRAINT "Posts_pkey" PRIMARY KEY (id)
)
WITH
(
	OIDS = FALSE
)

TABLESPACE pg_default;

ALTER TABLE public.posts OWNER to postgres;

INSERT INTO posts(title, body)
VALUES('the first title only','and this is the body of the first post.');

INSERT INTO posts(title, body)
VALUES('yet another title','and some more copy text for another body.');
```

#### Uselful curl commands to check out the API

```sh
# Create ()
curl -X POST 127.0.0.1:8000/posts -H "Content-Type: application/json" -d '{"title": "A new post headline for a new post", "body": "but also a new body text here will appear"}'

# Update ()
curl -X PUT 127.0.0.1:8000/posts/3 -H "Content-Type: application/json" -d '{"title": "and this is the new title for post number 1","body": "and the brand new edited body text of post number 1"}'

# Delete
curl -X DELETE 127.0.0.1:8000/posts/4 -H "Accept: application/json"
```

## Webserver

### Go setup

[pgxpool](https://github.com/jackc/pgx)

```sh
go mod init github.com/mariolazzari/go-postgres
go install github.com/air-verse/air@latest
go get github.com/jackc/pgx/v5
go mod tidy
```
