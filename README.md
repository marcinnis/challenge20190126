# SYSADMIN ASSIGNMENT

## 0. Overview

Attached package consists of 3 parts:
- **docker-compose.yml** - definition of Docker compose
- **pythonapp** - definition for application container
- **mysql** - SQL import for database

## 1. TECHNOLOGY USED

To solve the challenge I decided to use Docker Compose because of its simplicity and portability.

Using one command it will build 2 containers:
- **mysql server** - importing attached SQL queries (Image: *mysql:5.7*)
- **application server** - Flask server (Image: minimalistic python:2.7-alpine)

Application - due to security reasons - is running as *app* user with limited privileges (created during deployment process).

Of course this is very simple deployment. In production it would be different - taking care of reliability, security and high availability.

## 2. EXECUTION

#### CONFIGURATION

There is **.env** file included storing initial configuration used for database setup and execution of Flask application.

There are following parameters:

Parameter | Description
----|----
MYSQL_COMPOSE_USER|non privileged user for database
MYSQL_COMPOSE_PASS|password for above user
MYSQL_COMPOSE_DB|database where initial SQL will be imported
MYSQL_COMPOSE_ROOT_PASS|initial root password
APP_SERVER_PORT|Port for Flask application

#### EXECUTION

```
docker-compose up
```

#### VERIFICATION

```
curl http://localhost:5000
```

#### EXPECTED RESULT

```
Hello world!
```

## 3. ADDITIONAL REMARKS

I have to admit that at the beginning I used usual link between containers using following configuration:

```
links:
  - db
```

but then I noticed that Flask application listens on localhost only inside container.
In normal circumstances I would recommend allowing application to work on 0.0.0.0 which would be enough in that case but one of remarks was "It should not be modified."

Because of that finally links definition was removed and in that place :
```
network_mode: "host"
```
was applied. I think for single instance, simple application that should be enough.

Alternatively we could use different solution - like *nginx* redirection to local port of container, but this would be definitely too complex for this task.
