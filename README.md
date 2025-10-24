# KudosPoints: A Distributed Loyalty Rewards Platform

KudosPoints is a complete, event-driven loyalty rewards system built using a modern microservices architecture. It features a reliable `points-service` to manage member ledgers and a decoupled `rewards-service` to handle complex, rule-based points calculations.

The entire system is containerized with Docker and orchestrated with Docker Compose, demonstrating a scalable and resilient design.

## System Architecture

The system consists of four primary components running in their own Docker containers:

1.  **Points Service:** A Java/Spring Boot service that acts as the "bank." It provides a REST API for managing member data and their immutable points ledger.
2.  **Rewards Service:** A stateless Java/Spring Boot service that acts as the "rules engine." It listens to events from a message queue, calculates points based on business logic, and issues commands to the Points Service.
3.  **PostgreSQL:** The primary database, storing member data and the `points_ledger` audit trail.
4.  **RabbitMQ:** The message broker that decouples the services and enables asynchronous, event-driven communication.

---

## Technical Stack

| Component     | Technology                            | Purpose                                                |
| :------------ | :------------------------------------ | :----------------------------------------------------- |
| **Backend**   | Java 21, Spring Boot, Spring Data JPA | Core application logic and REST APIs                   |
| **Database**  | PostgreSQL                            | Persistent data storage (members, ledger)              |
| **Messaging** | RabbitMQ (Spring AMQP)                | Asynchronous, event-driven communication               |
| **DevOps**    | Docker, Docker Compose, Flyway        | Full containerization, orchestration, and DB migration |
| **Tools**     | Maven, Git                            | Build management and version control                   |

---

## Component Repositories

This "Hub" repository contains the master orchestration file (`docker-compose.yml`) and this README. The code for the individual microservices lives in their own dedicated repositories:

- **`points-service` Repository:** [**https://github.com/bhavesh-saluru/points-service**](https://github.com/bhavesh-saluru/points-service)
- **`rewards-service` Repository:** [**https://github.com/bhavesh-saluru/rewards-service**](https://github.com/bhavesh-saluru/rewards-service)

---

## How to Run the Entire System

This project is designed to be run as a complete, multi-container application.

### Prerequisites

- Java 21
- Maven 3.x
- Docker Desktop (running)

### Running Locally

1.  Clone all three repositories into the same parent directory:

    ```bash
    git clone https://github.com/bhavesh-saluru/kudospoints.git
    git clone https://github.com/bhavesh-saluru/points-service.git
    git clone https://github.com/bhavesh-saluru/rewards-service.git
    ```

    Your local directory should look like this:

    ```
    Spring Boot/
    ├── kudospoints/
    ├── points-service/
    └── rewards-service/
    ```

2.  Navigate to the `kudospoints` directory:

    ```bash
    cd kudospoints
    ```

3.  Build and run the entire application stack:
    ```bash
    docker-compose up --build
    ```

### Endpoints

Once running, the following services are available on your host machine:

- **Points Service API:** `http://localhost:8080`
- **Rewards Service (Health Check):** `http://localhost:8081/actuator/health`
- **RabbitMQ Management UI:** `http://localhost:15672` (User: `guest`, Pass: `guest`)
- **PostgreSQL Database:** `localhost:5432` (Connect with DBeaver or any SQL client)
