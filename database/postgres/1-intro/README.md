# Getting started with Postgres (again!)

So this is the first part of Postgres learning. Since I have been using Postgres for quite a while, there might be a few things which I assume the readers (future me) are aware of!

## Let's get started

Following is the simple command to spinup a postgres docker container

```
docker run --name postgres-intro -e POSTGRES_PASSWORD=password -d postgres
```

Now that the postgres docker instance is running we can start executing some commands on it. Following command will let you log into docker instance.

```
docker exec -it postgres-intro /bin/sh
```

Once you are in shell of postgres docker instance, log into postgres using following command and create a simple table!

```
psql -U postgres
```

Creating a simple table -

```
# create table test (test_id bigint primary key, test_name varchar(20));

# \dt
        List of relations
 Schema | Name | Type  |  Owner
--------+------+-------+----------
 public | test | table | postgres
(1 row)
```

Note that we didn't specify the data volume and hence once we delete the container, all the postgres data will also be gone.

```
docker rm -f postgres-intro
```

## Persisting data

This time we will specify a volume so that we can have the data even if the container is deleted/gone.

```
docker run --name postgres-intro -e POSTGRES_PASSWORD=password -v ${PWD}/pgdata:/var/lib/postgresql/data -d postgres
```

Now, if we follow the same steps to create a `test` table and deleete the docker container and re-create it using above command, you will see that the `test` table exists in the re-created docker instance.

## Allowing connections from outside

So far the postgres docker instances we created did not allow us to connect to database from external programs. Since the postgres is running in the docker, we have to expose the port so that the external programs can connect to it.
This can be done by using `-p` argument with `external_port:internal_port` mapping.

```
docker run --name postgres-intro -e POSTGRES_PASSWORD=password -v ${PWD}/pgdata:/var/lib/postgresql/data -p 8765:5432 -d postgres
```

And that's it for this session!
