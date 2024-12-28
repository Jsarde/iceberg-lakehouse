# Apache Iceberg Lakehouse with MinIO, Trino and Nessie

This project sets up a local environment for building an **Apache Iceberg Lakehouse** using **MinIO**, **Trino** and **Nessie**. It is designed for **on-premise, open-source and development purposes**.

Enjoy building your Apache Iceberg Lakehouse! 🚀

---

## Table of Contents

1. [Tech Stack](#tech-stack)
2. [Prerequisites](#prerequisites)
3. [Setup and Installation](#setup-and-installation)
4. [Configuration](#configuration)
5. [Usage](#usage)
6. [Acknowledgments](#acknowledgments)

---

## Tech Stack

- **Apache Iceberg**: Open table format for large datasets.
- **MinIO**: S3-compatible object storage.
- **Trino**: Distributed SQL query engine.
- **Nessie**: Git-like versioning for data lakes.
- **Docker**: Containerization for easy setup and deployment.
- **Docker Compose**: Orchestration of multi-container Docker applications.

---

## Prerequisites

Before you begin, ensure you have [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on your system.

---

## Setup and Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/Jsarde/iceberg-lakehouse.git
   cd iceberg-lakehouse
   ```

2. **Start the Services**:
   ```bash
   docker-compose up -d
   ```

3. **Verify the Services**:
   
   Ensure all services are running correctly:
   
   - [MinIO Console](http://localhost:9000)
     - Username: `minioadmin`
     - Password: `minioadmin`
   
   - [Trino UI](http://localhost:8080)
   
   - [Nessie API](http://localhost:19120)

---

## Configuration

#### Apache Iceberg
- Table format: `parquet`
- Catalog type: `nessie`

#### MinIO
- Access key: `minioadmin`
- Secret key: `minioadmin`
- Endpoint: `http://localhost:9000`
- Bucket: `warehouse` (used as the default warehouse directory for Iceberg)

#### Trino
- Catalog file: `catalog/iceberg.properties`
- Nessie catalog URI: `http://nessie:19120/api/v1`
- Default warehouse directory: `s3a://warehouse/`

#### Nessie
- API endpoint: `http://localhost:19120/api/v1`
- Default branch: `main`

### Environment Variables

The following environment variables are used in the `docker-compose.yml` file:

| Variable                     | Description                    | Default Value                |
|------------------------------|--------------------------------|------------------------------|
| `MINIO_ROOT_USER`            | MinIO root username            | `minioadmin`                 |
| `MINIO_ROOT_PASSWORD`        | MinIO root password            | `minioadmin`                 |
| `NESSIE_URI`                 | Nessie catalog URI             | `http://nessie:19120/api/v1` |
| `S3_ENDPOINT`                | MinIO S3 endpoint              | `http://minio:9000`          |
| `S3_ACCESS_KEY`              | MinIO access key               | `minioadmin`                 |
| `S3_SECRET_KEY`              | MinIO secret key               | `minioadmin`                 |

---

## Usage

Connect to the Trino container

```bash
docker exec -it <trino-container-id> trino
```

Show catalogs

```sql
SHOW CATALOGS;
```

Create a schema

```sql
CREATE SCHEMA iceberg.my_schema;
```

Show all the schemas in the catalog

```sql
SHOW SCHEMAS FROM iceberg;
```

Create a table

```sql
CREATE TABLE iceberg.my_schema.films (
    id bigint,
    name varchar,
    year integer,
    director varchar,
    rating double
) WITH (
    format = 'PARQUET'
);
```

Insert data into the table

```sql
INSERT INTO iceberg.tests.films (id, name, year, director, rating) VALUES
(1, 'Schindler''s List', 1993, 'Steven Spielberg', 8.9),
(2, 'The Lord of the Rings: The Return of the King', 2003, 'Peter Jackson', 8.9),
(3, 'Pulp Fiction', 1994, 'Quentin Tarantino', 8.9),
(4, 'Fight Club', 1999, 'David Fincher', 8.8),
(5, 'Forrest Gump', 1994, 'Robert Zemeckis', 8.8),
(6, 'Inception', 2010, 'Christopher Nolan', 8.7),
(7, 'The Matrix', 1999, 'Lana Wachowski, Lilly Wachowski', 8.7),
(8, 'Star Wars: Episode V - The Empire Strikes Back', 1980, 'Irvin Kershner', 8.7),
(9, 'Interstellar', 2014, 'Christopher Nolan', 8.6);
```

---

## Acknowledgments

A big shoutout to [DeepSeek](https://chat.deepseek.com/) for its awesome support and help in making this project happen!

---

