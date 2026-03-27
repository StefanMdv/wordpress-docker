# WordPress Docker Setup

## Table of Contents

* [Description](#description)
* [Quickstart](#quickstart)
* [Usage](#usage)
* [Configuration](#configuration)
* [Notes](#notes)

---

## Description

This repository contains a minimal Docker-based setup to run a WordPress instance together with a MySQL database.

### Purpose

The purpose of this project is to:

* Provide a simple and reproducible WordPress setup
* Run WordPress and MySQL in isolated containers
* Ensure data persistence using Docker volumes
* Allow configuration via environment variables

### Repository Contents

* `docker-compose.yaml` – Defines the WordPress and MySQL services
* `.gitignore` – Excludes unnecessary and sensitive files from version control
* `README.md` – Project documentation

---

## Quickstart

### Prerequisites

Make sure the following tools are installed:

* Docker
* Docker Compose

### Setup

1. Clone the repository:

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_NAME>
```

2. Create a `.env` file in the project root:

```env
DB_NAME=<db_name>
DB_USER=<db_username>
DB_PASSWORD=<db_password>
DB_ROOT_PASSWORD=<db_root_password>
```

3. Start the containers:

```bash
docker-compose up -d
```

4. Open WordPress in your browser:

```
http://<YOUR_SERVER_IP>:8080
```

---

## Usage

### Starting the services

```bash
docker-compose up -d
```

### Stopping the services

```bash
docker-compose down
```

### Restarting the services

```bash
docker-compose restart
```

### Viewing logs

```bash
docker-compose logs -f
```

---

## Configuration

### Environment Variables

Sensitive configuration values are managed via a `.env` file and **must not be committed to the repository**.

| Variable         | Description            | Default    |
| ---------------- | ---------------------- | ---------- |
| DB_NAME          | Name of the database   | wordpress  |
| DB_USER          | Database user          | wordpress  |
| DB_PASSWORD      | Database user password | (required) |
| DB_ROOT_PASSWORD | MySQL root password    | (required) |

---

### Docker Compose Configuration

The setup consists of two services:

#### 1. Database Service (`db`)

* Uses MySQL 8.0
* Stores data in a persistent Docker volume
* Configured via environment variables

#### 2. WordPress Service (`wordpress`)

* Uses the official WordPress image
* Connects to the database service
* Exposes port `8080`

---

### Port Configuration

Default:

```yaml
ports:
  - "8080:80"
```

To change the port (e.g., to 8081):

```yaml
ports:
  - "8081:80"
```

---

### Data Persistence

Database data is stored using a Docker volume:

```
db_data:/var/lib/mysql
```

This ensures that:

* Data is not lost after container restarts
* WordPress content remains intact

---

### Networking

Both services run in the same Docker network:

```
wordpress_network
```

This allows communication using service names (e.g., `db`).

---

### Customization

You can modify the setup by:

* Changing environment variables in `.env`
* Adjusting service configuration in `docker-compose.yaml`
* Updating port mappings
* Switching Docker images (e.g., different WordPress or MySQL versions)

---

## Notes

* Do not store passwords or secrets in the repository
* Always use environment variables for sensitive data
* The containers are configured with `restart: always`
* WordPress will be available on port `8080`
* Data persists across restarts due to Docker volumes

---

## Testing

After starting the setup:

1. Open the application in your browser:

   ```
   http://<YOUR_SERVER_IP>:8080
   ```

2. Complete the WordPress installation

3. Verify:

   * Login works with your credentials
   * Content can be created and saved

4. Restart containers:

```bash
docker-compose restart
```

5. Ensure that all data is still available after restart
