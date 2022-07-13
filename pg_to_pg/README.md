CDC From Postgres to Postgres
=

# Topology

```
                   +--------------+
                   |              |
                   |  PostgreSQL  |
                   |              |
                   +------+-------+
                          |
                          |
                          |
          +---------------v------------------+
          |                                  |
          |           Kafka Connect          |
          |  (Debezium, JDBC connectors)     |
          |                                  |
          +---------------+------------------+
                          |
                          |
                          |
                          |
                  +-------v--------+
                  |                |
                  |   PostgreSQL   |
                  |                |
                  +----------------+

```


# Running it up

## Spin up Docker compose


```bash
docker-compose up -d
```

## Setup Database

```bash
psql -U mine -d sharingcdc -W
```

`-W` option will prompt us to input password to connect.

Once you are in the `psql` mode, create table:


```sql
CREATE TABLE public.todo (
	id uuid NOT NULL,
	created_date timestamp NULL DEFAULT now(),
	is_done bool NULL,
	modified_date timestamp NULL,
	title varchar(255) NULL,
	CONSTRAINT todo_pkey PRIMARY KEY (id)
);
```

To enable CDC with Postgres, we need to give replication permission to our database:

```sql
ALTER TABLE public.todo REPLICA IDENTITY FULL;
```

Let's insert some data in it:

```sql
INSERT INTO public.todo
(id, created_date, is_done, modified_date, title)
VALUES('79fe5ffa-94ed-4871-a5cd-300471586914'::uuid, '2022-05-31 14:47:12.198', false, '2022-06-20 22:52:10.648', 'do laundry');
INSERT INTO public.todo
(id, created_date, is_done, modified_date, title)
VALUES('129eef91-8f55-4edd-9c63-804c6f1a3f5b'::uuid, '2022-05-31 14:59:58.150', false, '2022-06-20 22:52:11.481', 'feed the dog');
```


## Creating connectors

### `source` connector


```bash
curl -i -X POST -H "Content-Type:application/json" -H "Accept:application/json" localhost:8083/connectors -d "@debezium-source-config.json"
```

#### Check connector status

```bash
curl localhost:8083/connectors/sharingcdc-connector/status
```

If nothing went wrong, it will give result:

```json
{
  "name":"sharingcdc-connector",
  "connector":{
    "state":"RUNNING",
    "worker_id":"172.21.0.6:8083"
  },
  "tasks":[
    {
      "id":0,
      "state":"RUNNING",
      "worker_id":"172.21.0.6:8083"
    }
  ],
  "type":"source"
}
```

### `sink` connector

After we have all the provider config set up, now it's time to consume the message and replicate all the data into another PostgresDB.

Send the payload through `curl`:

```bash
curl -i -X POST -H "Content-Type:application/json" -H "Accept:application/json" localhost:8083/connectors -d "@debezium-sink-config.json"
```

It will gives us this:

```json
HTTP/1.1 201 Created
Date: Tue, 12 Jul 2022 06:43:15 GMT
Location: http://localhost:8083/connectors/sharingcdc-sink-connector
Content-Type: application/json
Content-Length: 539
Server: Jetty(9.4.43.v20210629)

{
  "name":"sharingcdc-sink-connector",
  "config":{
    "auto.create":"true",
    "connection.url":"jdbc:postgresql://host.docker.internal:5434/sharingcdc-copy?user=mine&password=qwepoi123",
    "connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
    "insert.mode":"upsert",
    "pk.fields":"id",
    "pk.mode":"record_value",
    "tasks.max":"1",
    "topics":"postgres.public.todo",
    "transforms":"unwrap",
    "transforms.unwrap.type":"io.debezium.transforms.ExtractNewRecordState",
    "transforms.unwrap.drop.tombstones":"false",
    "name":"sharingcdc-sink-connector"
  },
  "tasks":[],
  "type":"sink"
}
```

## Checking your topic

To check your topic list, Kafka Connect also provide a CLI to check our topics using the `kafka-topics.sh` file located in the `/bin` directory.

Open terminal in our Kafka Connect container, and run:

```bash
./bin/kafka-topics.sh --bootstrap-server=kafka:9092 --list
```

> ℹ️  `--bootstrap-server` param can also be replaced with `--zookeeper` if you prefer.


Another tool we can use is `Kafkacat` which I also include in the `docker-compose.yml` file. To use it, we simply run:

```bash
docker exec kafkacat kafkacat -b kafka:9092 -L # List existing topic in kafka:9092
```

I noticed the weirdness of topic name creation by having this topic:

```bash
  topic "todo" with 1 partitions:
    partition 0, leader 1, replicas: 1, isrs: 1
```

### Monitor changes detection by tailing it

Now, we need to check whether the `source` connector is actually able to stream the data changes. There are several ways to do this:

#### Via Schema Registry

We can use `kafka-avro-console-consumer` (because we are using `Avro` converter), by running:

```bash
docker-compose exec schema-registry /usr/bin/kafka-avro-console-consumer --bootstrap-server kafka:9092 --from-beginning --property print.key=true --property schema-registry.url=http://schema-registry:8081 --topic postgres.public.todo
```

or if you prefer via `bash` :

- Open `schema-registry` bash:

```bash
docker exec -it <container-name> sh
```
- then run

```bash
./usr/bin/kafka-avro-console-consumer -bootstrap-server kafka:9092 --from-beginning --property print.key=true --property schema-registry.url=http://schema-registry:8081 --topic postgres.public.todo
```

#### Via Kafkacat

Or we can also use `kafkacat`:

```bash
docker exec kafkacat kafkacat -b kafka:9092 -t todo -C
```

You can change `todo` with the topic name you want to tail.

### Checking if the connector works

Now that we have tailed our connector, we can confirm by trying to modify data, insert new data or even delete data.

#### Try to insert new data

```sql
INSERT INTO public.todo
(id, created_date, is_done, modified_date, title)
VALUES('6fc341f0-04a8-4c91-92bb-3dbb1ef0b557'::uuid, '2022-06-20 22:11:09.613', false, '2022-06-20 22:52:26.648', 'test');
```

#### What we will get in the topic monitoring console

```json
{
  "id":"6fc341f0-04a8-4c91-92bb-3dbb1ef0b557"
}   
{
  "before":null,
  "after":{
    "postgres.public.todo.Value":{
      "id":"6fc341f0-04a8-4c91-92bb-3dbb1ef0b557",
      "created_date":{
        "long":1655763069613000
      },
      "is_done":{
        "boolean":false
      },
      "modified_date":{
        "long":1655765546648000
      },
      "title":{
        "string":"test"
      }
    }
  },
  "source":{
    "version":"1.4.2.Final",
    "connector":"postgresql",
    "name":"postgres",
    "ts_ms":1657617691850,
    "snapshot":{
      "string":"false"
    },
    "db":"sharingcdc",
    "schema":"public",
    "table":"todo",
    "txId":{
      "long":493
    },
    "lsn":{
      "long":23876504
    },
    "xmin":null
  },
  "op":"c",
  "ts_ms":{
    "long":1657617692088
  },
  "transaction":null
}
```

Try another operation if you want to explore more 😊

## Verify the data capturing

Now, everything should be running normally. If so, let's try to connect to the second PostgreDB and check if it works~

```bash
docker exec -it pg2 sh #run bash of pg2

psql -U mine -d sharingcdc-copy -W #login to sharingcdc-copy database
```

After submitting the password, you can run:

```sql
\d --check all relations
```

We should see that table `todo` automatically created for us, which we never created it before.

```bash
sharingcdc-copy=# \d
       List of relations
 Schema | Name | Type  | Owner
--------+------+-------+-------
 public | todo | table | mine
(1 row)
```

```bash
sharingcdc-copy=# select * from todo;
 is_done |                  id                  |   created_date   |  modified_date   |    title
---------+--------------------------------------+------------------+------------------+--------------
 f       | 129eef91-8f55-4edd-9c63-804c6f1a3f5b | 1654009198150000 | 1655765531481000 | feed the dog
 f       | 79fe5ffa-94ed-4871-a5cd-300471586914 | 1654008432198000 | 1655765530648000 | do laundry
 f       | 6fc341f0-04a8-4c91-92bb-3dbb1ef0b557 | 1655763069613000 | 1655765546648000 | test
(3 rows)
```

Let's try to insert new data into our first database:

```bash
sharingcdc=# INSERT INTO public.todo
(id, created_date, is_done, modified_date, title)
VALUES('850c5925-bb49-4795-9c19-4b7a6b4514b5'::uuid, '2022-06-20 22:12:02.335', false, NULL, 'Test12');
INSERT 0 1
```

Then confirm it in the second database:

```bash
sharingcdc-copy=# select * from todo;
 is_done |                  id                  |   created_date   |  modified_date   |    title
---------+--------------------------------------+------------------+------------------+--------------
 f       | 129eef91-8f55-4edd-9c63-804c6f1a3f5b | 1654009198150000 | 1655765531481000 | feed the dog
 f       | 79fe5ffa-94ed-4871-a5cd-300471586914 | 1654008432198000 | 1655765530648000 | do laundry
 f       | 6fc341f0-04a8-4c91-92bb-3dbb1ef0b557 | 1655763069613000 | 1655765546648000 | test
 f       | 850c5925-bb49-4795-9c19-4b7a6b4514b5 | 1655763122335000 |                  | Test12
(4 rows)
```

You could also see the activity logged in the schema registry:

```json
{
  "id":"850c5925-bb49-4795-9c19-4b7a6b4514b5"
}   
{
  "before":null,
  "after":{
    "postgres.public.todo.Value":{
      "id":"850c5925-bb49-4795-9c19-4b7a6b4514b5",
      "created_date":{
        "long":1655763122335000
      },
      "is_done":{
        "boolean":false
      },
      "modified_date":null,
      "title":{
        "string":"Test12"
      }
    }
  },
  ...
  <snip>
}
```