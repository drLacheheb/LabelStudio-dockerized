# LabelStudio Dockerized

A comprehensive Docker-based setup for deploying **Label Studio** with **PostgreSQL**, **Nginx Proxy Manager**, and an automated backup.

## Features

- **Label Studio**: The open source data labeling tool.
- **PostgreSQL**: Robust database backend for Label Studio.
- **Nginx Proxy Manager**: Easy-to-use reverse proxy with free SSL management.
- **Automated Backups**: Scheduled volume backups using `offen/docker-volume-backup` with local and remote support.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd LabelStudio-dockerized
    ```

2.  **Configure Environment Variables:**
    Copy the example environment file:
    ```bash
    cp .env.example .env
    ```
    Then edit `.env` with your secure passwords and preferences.

3.  **Start the Services:**
    ```bash
    docker-compose up -d
    ```

## Usage

| Service | URL | Description |
| :--- | :--- | :--- |
| **Label Studio** | `http://localhost:8080` | Main application interface (login with `LS_USERNAME`/`LS_PASSWORD`). |
| **Nginx Proxy Manager** | `http://localhost:81` | Admin interface (Default: `admin@example.com` / `changeme`). |
| **PostgreSQL** | `localhost:5432` | Database access. |

### Nginx Proxy Manager Setup
Access the admin panel at `http://localhost:81`. Use it to configure domains (Proxy Hosts) pointing to:
- **Hostname:** `labelstudio`
- **Port:** `8080`

## Backups

The `backup` service uses [offen/docker-volume-backup](https://github.com/offen/docker-volume-backup) to create compressed tarballs of the data volumes.

- **Storage**: Backups are saved to `./backups` on the host AND uploaded to **WebDAV** (configurable).
- **Schedule**: Defined by `CRON_SCHEDULE` in `.env` (Default: Daily at 2am).
- **Retention**: Keeps backups for 14 days (configurable via `BACKUP_RETENTION_DAYS`).
- **Consistency**: To ensure data consistency, the `labelstudio` and `db` containers are **briefly stopped** during the backup process.

### Configuration
You can configure the backup behavior using environment variables in `.env` or by modifying `docker-compose.yml`.
See [offen/docker-volume-backup documentation](https://offen.github.io/docker-volume-backup/) for advanced features like S3/WebDAV upload and notifications.

## Directory Structure

```
LabelStudio-dockerized/
├── data/               # Persistent data (DB, Label Studio data)
├── nginx/              # Nginx Proxy Manager data and certificates
├── .env.example        # Example environment variables
├── docker-compose.yml  # Service definition
└── README.md           # Project documentation
```