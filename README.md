# E-Commerce Order & Payment Platform

A microservices backend built with Java 21 and Spring Boot. Users register, browse products, place orders and pay online. Each part of the business is its own service, and services communicate through Kafka events.

## Architecture (planned)

| Service | Responsibility | Status |
|---|---|---|
| api-gateway | Single entry point, JWT validation, routing | planned |
| user-service | Registration, login, roles, JWT | in progress |
| product-service | Catalog, stock, Redis cache | planned |
| order-service | Orders and status, publishes events | planned |
| payment-service | Razorpay (test mode), idempotent payments | planned |
| notification-service | Email on order success/failure | planned |

**Order flow (saga):** `OrderCreated` -> reserve stock -> payment -> `PAID` (email) or failure -> release stock -> `CANCELLED`.

## Tech stack

Java 21, Spring Boot 3, Spring Data JPA, MySQL, Spring Security + JWT, Spring Cloud Gateway, Kafka, Redis, Resilience4j, Docker Compose, Testcontainers, GitHub Actions, OpenAPI/Swagger.

## Getting started

Requirements: JDK 21, Maven, Docker Desktop.

```bash
# 1. Start MySQL, Redis and Kafka
docker compose up -d

# 2. Check that all three are running
docker compose ps

# 3. Run the user service
cd user-service
mvn spring-boot:run
```

Check it: http://localhost:8081/actuator/health should return `{"status":"UP"}`.

## Project structure

```
ecommerce-platform/
├── docker/mysql/init.sql   # creates one database per service
├── docker-compose.yml         # MySQL, Redis, Kafka
├── user-service/
└── (more services added as we go)
```

## Roadmap

1. user-service: register, login, JWT
2. product-service: catalog, stock, Redis caching
3. order-service: create order, status
4. api-gateway: routing and JWT check
5. Kafka events and saga
6. payment-service: Razorpay test mode, Resilience4j
7. notification-service
8. Tests (Testcontainers), CI (GitHub Actions), architecture diagram
