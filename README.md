# AIMS (Academic Information Management System)

This is a project for the Software Architecture subject at the
National University of Colombia.

It uses a layered architecture with multiple microservices, and different
technologies.

This project was created with the intention of being a platform for
the management of academic information, where students and teachers can
interact with the system easily offering a diverse set of services.

## Members

| Name                          | Email                      | Github                                                      |
| ----------------------------- | -------------------------- | ----------------------------------------------------------- |
| David Esteban Hernandez Gomez | davhernandezgo@unal.edu.co | [DavidHernandez2001](https://github.com/DavidHernandez2001) |
| Josué David Briceño Urquijo   | jbriceno@unal.edu.co       | [jdbu2002](https://github.com/jdbu2002)                     |
| Juan Diego Ramírez Lemos      | jramirezle@unal.edu.co     | [Judirale13](https://github.com/Judirale13)                 |
| Santiago Rodríguez Vallejo    | sarodriguezva@unal.edu.co  | [sarodriguezva](https://github.com/sarodriguezva)           |
| Santiago Sánchez Mora         | sansanchezmo@unal.edu.co   | [sansanchezmo](https://github.com/sansanchezmo)             |
| Sebastian Garnica Quiroz      | sgarnicaq@unal.edu.co      | [SGman98](https://github.com/SGman98)                       |

## How to run

### Prerequisites

- [Git](https://git-scm.com/downloads) 🐱‍💻
- [Docker](https://docs.docker.com/install/) 🐳
- [Docker Compose](https://docs.docker.com/compose/install/) 🐙
- [OpenSSL](https://www.openssl.org/) 🛡️

### Run

Clone the repository and run the following command to get the submodules:

```sh
git submodule update --init --recursive
```

Create the ssl certificates running the following command in the security folder:

```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout aims.key -out aims.crt -config aims.conf
```

Then run the following command to start the project:

```sh
docker-compose up -d --build
```

> This will run:\
> 6 containers for databases\
> 7 containers for microservices\
> 1 container for the api-gateway\
> 1 container for the message broker\
> 1 container for the web application\
> 2 containers for the reverse proxies (one for the web application and one for the api-gateway)\
> 1 container for the ldap and 1 for phpldapadmin\
> 1 container for the interface\
> In total 21 containers

> Each part of the project have a different git repository with its own
> documentation and deployment instructions and files.

## Layers

### Data Layer

It contains all the databases needed for the project.

| Database      | Type                                         |
| ------------- | -------------------------------------------- |
| account-db    | [MySQL](https://www.mysql.com/) 🐬           |
| college-db    | [MySQL](https://www.mysql.com/) 🐬           |
| enrollment-db | [PostgreSQL](https://www.postgresql.org/) 🐘 |
| grading-db    | [MongoDB](https://www.mongodb.com/) 🍃       |
| profile-db    | [MongoDB](https://www.mongodb.com/) 🍃       |
| subject-db    | [MySQL](https://www.mysql.com/) 🐬           |

> There is an initial script with some data in the corresponding logic folder, the file is called `init.sql`.

### Logic Layer

It contains all the microservices, each one uses the corresponding database, and
exposes a REST API to interact with the data.

| Microservice  |                   Language                   |                        Framework                         |                    Repository                    |
| :------------ | :------------------------------------------: | :------------------------------------------------------: | :----------------------------------------------: |
| account-ms    | [JavaScript](https://www.javascript.com/) 📜 |           [Express](https://expressjs.com/) 🚀           |  [Link](https://github.com/AIMS-UN/account_ms)   |
| college-ms    |       [Java](https://www.java.com/) ☕       | [Spring Boot](https://spring.io/projects/spring-boot) 🍃 |  [Link](https://github.com/AIMS-UN/college_ms)   |
| enrollment-ms |       [Java](https://www.java.com/) ☕       | [Spring Boot](https://spring.io/projects/spring-boot) 🍃 | [Link](https://github.com/AIMS-UN/enrollment_ms) |
| grading-ms    |    [Rust](https://www.rust-lang.org/) 🦀     |             [Rocket](https://rocket.rs/) 🚀              |  [Link](https://github.com/AIMS-UN/grading_ms)   |
| profile-ms    |     [Python](https://www.python.org/) 🐍     | [Flask](https://flask.palletsprojects.com/en/1.1.x/) 🌶️  |  [Link](https://github.com/AIMS-UN/profile_ms)   |
| schedule-ms   |         [Go](https://golang.org/) 🐹         |             [Gin](https://gin-gonic.com) 🍸              |  [Link](https://github.com/AIMS-UN/schedule_ms)  |
| subject-ms    |       [Java](https://www.java.com/) ☕       | [Spring Boot](https://spring.io/projects/spring-boot) 🍃 |  [Link](https://github.com/AIMS-UN/subject_ms)   |

> The schedule microservices uses the same database as the enrollment microservice.
> enrollment-ms is for mutations and schedule-ms is for queries.

### Integration Layer

It contains the API Gateway, this gateway connects all the microservices and
exposes a single API using the [GraphQL](https://graphql.org/) query language,
and the [Apollo Server](https://www.apollographql.com/docs/apollo-server/).

| Microservice |                     Language                     |              Framework               |                   Repository                   |
| :----------- | :----------------------------------------------: | :----------------------------------: | :--------------------------------------------: |
| ag           | [TypeScript](https://www.typescriptlang.org/) 📜 | [Express](https://expressjs.com/) 🚀 | [Link](https://github.com/AIMS-UN/api_gateway) |

> The connection with the grading microservice uses a queue message broker, in this case [RabbitMQ](https://www.rabbitmq.com/).

### Presentation (App) Layer

In this layer, we have the web application and the mobile application.

| Application |                     Language                     |             Framework              |                  Repository                   |
| :---------- | :----------------------------------------------: | :--------------------------------: | :-------------------------------------------: |
| web         | [TypeScript](https://www.typescriptlang.org/) 📜 | [Angular](https://angular.io/) 🍃  |  [Link](https://github.com/AIMS-UN/web_app)   |
| mobile      |           [Dart](https://dart.dev/) 🎯           | [Flutter](https://flutter.dev/) 🎯 | [Link](https://github.com/AIMS-UN/mobile_app) |

To run the mobile application, you need to install [Flutter](https://flutter.dev/docs/get-started/install) and run the following command in the apps/mobile folder:

```sh
flutter run
```

### Security Layer

In this layer, we have 2 proxies, one for the web application and one for the
API gateway.

| ..                |                    Technology                    |
| :---------------- | :----------------------------------------------: |
| web-app-proxy     |        [Nginx](https://www.nginx.com/) 🐳        |
| api-gateway-proxy |        [Nginx](https://www.nginx.com/) 🐳        |
| account-ldap      |     [OpenLDAP](https://www.openldap.org/) 🐳     |
| phpldapadmin      | [phpLDAPadmin](https://www.phpLDAPadmin.org/) 🐳 |

### Interoperability Layer

In this layer, we have the interface it exposes a SOAP API with the
getSubjects functionality.

| ..        |                                                      Technology                                                      |
| :-------- | :------------------------------------------------------------------------------------------------------------------: |
| interface | [Javascript](https://www.javascript.com/) 📜 [Node.js](https://nodejs.org/en/) 🐳 [SOAP](https://www.soapui.org/) 🐳 |
