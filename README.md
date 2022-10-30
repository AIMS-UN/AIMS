# AIMS (Academic Information Management System)

This is a project for the Software Architecture subject at the
National University of Colombia.

It uses a layered architecture with multiple microservices, and different
technologies.

This project was created with the intention of being a platform for
academic information management, where students and teachers can
interact with the system easily offering a diverse set of services.

## Members

| Name                          | Email                      | Github                                                      |
| ----------------------------- | -------------------------- | ----------------------------------------------------------- |
| David Esteban Hernandez Gomez | davhernandezgo@unal.edu.co | [DavidHernandez2001](https://github.com/DavidHernandez2001) |
| Josué David Briceño Urquijo   | jbriceno@unal.edu.co       | [jdbu2002](https://github.com/jdbu2002)                     |
| Juan Diego Ramírez Lemos      | jramirezle@unal.edu.co     | [Judirale13](https://github.com/jdbu2002)                   |
| Santiago Rodríguez Vallejo    | sarodriguezva@unal.edu.co  | [sarodriguezva](https://github.com/sarodriguezva)           |
| Santiago Sánchez Mora         | sansanchezmo@unal.edu.co   | [sansanchezmo](https://github.com/sansanchezmo)             |
| Sebastian Garnica Quiroz      | sgarnicaq@unal.edu.co      | [SGman98](https://github.com/SGman98)                       |

## How to run

### Prerequisites

- [Git](https://git-scm.com/downloads) 🐱‍💻
- [Docker](https://docs.docker.com/install/) 🐳
- [Docker Compose](https://docs.docker.com/compose/install/) 🐙

### Run

Clone the repository and run the following command to get the submodules:

```bash
git submodule update --init --recursive
```

Then run the following command to start the project:

```bash
docker-compose up -d --build
```

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

This layer may be deployed using the docker-compose file in the data folder.
The deployment details file can be found [here](https://github.com/SGman98/AIMS/blob/main/data/docker-compose.yml).

### Logic Layer

It contains all the microservices.

| Microservice  |                   Language                   |                        Framework                         |                           Repository                           |
| :------------ | :------------------------------------------: | :------------------------------------------------------: | :------------------------------------------------------------: |
| account-ms    | [JavaScript](https://www.javascript.com/) 📜 |           [Express](https://expressjs.com/) 🚀           |      [Link](https://github.com/jdbu2002/aims_account_ms)       |
| college-ms    |       [Java](https://www.java.com/) ☕       | [Spring Boot](https://spring.io/projects/spring-boot) 🍃 |     [Link](https://github.com/Judirale13/aims_college_ms)      |
| enrollment-ms |       [Java](https://www.java.com/) ☕       | [Spring Boot](https://spring.io/projects/spring-boot) 🍃 |  [Link](https://github.com/sarodriguezva/aims_enrollment_ms)   |
| grading-ms    |    [Rust](https://www.rust-lang.org/) 🦀     |             [Rocket](https://rocket.rs/) 🚀              |       [Link](https://github.com/SGman98/aims_grading_ms)       |
| profile-ms    |     [Python](https://www.python.org/) 🐍     | [Flask](https://flask.palletsprojects.com/en/1.1.x/) 🌶️  |    [Link](https://github.com/sansanchezmo/aims_profile_ms)     |
| schedule-ms   |         [Go](https://golang.org/) 🐹         |             [Gin](https://gin-gonic.com) 🍸              | [Link](https://github.com/DavidHernandez2001/aims_schedule_ms) |
| subject-ms    |       [Java](https://www.java.com/) ☕       | [Spring Boot](https://spring.io/projects/spring-boot) 🍃 |     [Link](https://github.com/Judirale13/aims_subject_ms)      |

Each microservice exposes a REST API.

### Integration Layer

It contains the API Gateway.

| Microservice |                     Language                     |              Framework               |                     Repository                      |
| :----------- | :----------------------------------------------: | :----------------------------------: | :-------------------------------------------------: |
| ag           | [TypeScript](https://www.typescriptlang.org/) 📜 | [Express](https://expressjs.com/) 🚀 | [Link](https://github.com/SGman98/aims_api_gateway) |

API Gateway exposes a GraphQL API using [Apollo Server](https://www.apollographql.com/docs/apollo-server/).

### Interface (Presentation) Layer

In this layer, we have the web application and the mobile application.

| Application |                     Language                     |             Framework              |                     Repository                     |
| :---------- | :----------------------------------------------: | :--------------------------------: | :------------------------------------------------: |
| web         | [TypeScript](https://www.typescriptlang.org/) 📜 | [Angular](https://angular.io/) 🍃  |  [Link](https://github.com/sarodriguezva/aims_wa)  |
| mobile      |           [Dart](https://dart.dev/) 🎯           | [Flutter](https://flutter.dev/) 🎯 | [Link](https://github.com/SGman98/aims_mobile_app) |

To run the web application there is a docker-compose file in the interfaces/web folder.

To run the mobile application, you need to install [Flutter](https://flutter.dev/docs/get-started/install) and run the following command in the interfaces/mobile folder:

```sh
flutter run
```
