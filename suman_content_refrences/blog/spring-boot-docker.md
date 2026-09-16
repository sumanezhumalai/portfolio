---
title: "Containerising a Spring Boot App with Docker — A Practical Guide"
description: "A step-by-step walkthrough of packaging a Spring Boot application into Docker using a production-ready multi-stage Dockerfile and Docker Compose with PostgreSQL."
pubDate: 2026-08-20
tags:
  - java
  - spring-boot
  - docker
draft: false
---

Running a Spring Boot application locally with `./mvnw spring-boot:run` is straightforward. Getting it to run reliably anywhere else — a colleague's machine, a staging server, a production VPS — is where Docker earns its place. This guide walks through the entire process from a working Spring Boot app to a production-ready container.

## What we're building

A multi-stage Docker build that:

- Compiles and packages the application using Maven in a builder image.
- Copies only the fat JAR into a minimal JRE runtime image.
- Runs the container as a non-root user.
- Exposes the app via Docker Compose alongside a PostgreSQL service.

The result is a single `docker compose up` command that boots the entire stack from scratch.

## The multi-stage Dockerfile

A naive Dockerfile copies source code and installs the full JDK in the image, resulting in a 600MB+ image. A multi-stage build separates the build environment from the runtime environment:

```dockerfile
# ── Stage 1: Build ──────────────────────────────
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app

COPY .mvn/ .mvn/
COPY mvnw pom.xml ./
RUN ./mvnw dependency:go-offline -q

COPY src/ src/
RUN ./mvnw package -DskipTests -q

# ── Stage 2: Runtime ─────────────────────────────
FROM eclipse-temurin:21-jre-alpine AS runtime
WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/target/*.jar app.jar

USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

The builder stage uses a full JDK image to compile. The runtime stage uses a lean JRE-only Alpine image — typically under 200MB for the final image. The `adduser` step runs the process as a non-root user, which is a basic but important security practice.

## Caching Maven dependencies

The order of `COPY` instructions in the builder stage matters. Copying `pom.xml` and running `dependency:go-offline` before copying source code means Docker caches the dependency layer separately. If only source files change, the dependency download is skipped on the next build.

## Docker Compose for the full stack

Most Spring Boot applications need a database. Docker Compose wires them together:

```yaml
services:
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB:       myapp
      POSTGRES_USER:     myuser
      POSTGRES_PASSWORD: secret
    volumes:
      - pg_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U myuser -d myapp"]
      interval: 10s
      retries: 5

  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL:      jdbc:postgresql://db:5432/myapp
      SPRING_DATASOURCE_USERNAME: myuser
      SPRING_DATASOURCE_PASSWORD: secret
      SPRING_JPA_HIBERNATE_DDL_AUTO: update
    depends_on:
      db:
        condition: service_healthy

volumes:
  pg_data:
```

The `healthcheck` on the database service ensures the application container only starts after PostgreSQL is actually accepting connections — not just after the container has started.

## Building and running

```bash
# Build and start
docker compose up --build

# Detached
docker compose up --build -d

# Stream app logs
docker compose logs -f app

# Tear down (keeps volumes)
docker compose down

# Tear down and remove volumes
docker compose down -v
```

## What this unlocks

Once the application is containerised, the path to production is straightforward. Push the image to a registry (GHCR), pull it on a VPS, and run it behind Traefik. GitHub Actions can automate the entire pipeline — build, push, and deploy on every push to `main`. That's the stack I'm using for this portfolio's future deployment, and the same pattern I applied when building Q-Mail.
