# Tutorial: Creating a REST api with Kotlin + SpringBoot + JPA + Flyway

>원본 : https://github.com/Grekz/example-kotlin-crud

## Setup

You need to have a mysql database running with a database named: 'kotlin_crud_db'
Let's run this command and you will have it:
```shell script
$ docker run --rm --name test-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=kotlin_crud_db -d mysql:latest
```

## Running the app

```shell script
$ gradle bootRun 
```

## Manual testing

```shell script
## Get all tasks
$ curl --location --request GET 'localhost:8080/v1/api/tasks'

## Get task by id
$ curl --location --request GET 'localhost:8080/v1/api/tasks/1'

## Post task
$ curl --location --request POST 'localhost:8080/v1/api/tasks/' \
--header 'Content-Type: application/json' \
--data-raw '{
	"title": "new 2nd task", 
	"status": 1, 
	"priority": 1,
	"description":"2244 blah blah task"
}'

## Update task
$ curl --location --request PUT 'localhost:8080/v1/api/tasks/2' \
--header 'Content-Type: application/json' \
--data-raw '{
	"title": "blah blah new 2nd task", 
	"status": 1, 
	"priority": 1,
	"description":"2244 blah blah task"
}'

## Delete task
$ curl --location --request DELETE 'localhost:8080/v1/api/tasks/2'
```

## PostgreSQL


## Spring DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring:
  datasource:
    url: &dbURL jdbc:postgresql://localhost:5432/kotlin_crud_db
    username: &dbUser kotlin_crud_db
    password: &dbPassword kotlin_crud_db
  flyway:
    enabled: true
    baselineOnMigrate: false # flyway_schema_history 테이블이 존재하는 경우 false, 반대면 true
    locations: classpath:db/migration
  #  flyway:
#    ignore-ignored-migrations=true:
  jpa:
    database: POSTGRESQL
    database-platform: org.hibernate.dialect.PostgreSQL95Dialect
    show-sql: false
    generate-ddl: true
    hibernate:
      ddl-auto: none
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
    properties:
      hibernate:
        jdbc:
          lob:
            non_contextual: true
#flyway:
#  url: *dbURL
#  user: *dbUser
#  password: *dbPassword


//	implementation("org.flywaydb:flyway-mysql")
//	runtimeOnly("mysql:mysql-connector-java")
	runtimeOnly("org.postgresql:postgresql")

V1

DROP TABLE IF EXISTS public.tasks;
CREATE TABLE IF NOT EXISTS public.tasks(
    id INT8 PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    start_date TIMESTAMP,
    due_date TIMESTAMP,
    status INT8 NOT NULL,
    priority INT8 NOT NULL,
    description TEXT,
    create_at TIMESTAMP NOT NULL DEFAULT now(),
    updated_at TIMESTAMP
);

DROP SEQUENCE IF EXISTS public.seq_tasks;
CREATE SEQUENCE IF NOT EXISTS public.seq_tasks
    INCREMENT BY 1
    MINVALUE 1
    MAXVALUE 9223372036854775807
    START 1
	CACHE 1
	CYCLE;
	
V2

insert into public.tasks(id, title, status, priority, description) values (NEXTVAL('seq_tasks'), 'First Task', 1, 1, 'My first task description');

V3

insert into public.tasks(id, title, status, priority, description) values (NEXTVAL('seq_tasks'), 'Another Task', 2, 2, 'lol lol this desc description');

V4

insert into public.tasks(id, title, status, priority, description) values (NEXTVAL('seq_tasks'), 'Again Another Task', 2, 2, 'Again... lol lol this desc description');
