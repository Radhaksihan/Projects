# Docker Compose: Nginx + MySQL + Persistent Volume

## Services

- **nginx**
  - Image: `nginx:alpine`
  - Port: `8080:80`
  - Serves static content from `./nginx/html`

- **mysql**
  - Image: `mysql:8.4`
  - Port: `3306:3306`
  - Environment variables configure root password, database, and app user
  - Data is persisted using a named volume

## Persistent Volume

- Named volume: `mysql_data`
- Mounted to MySQL data directory: `/var/lib/mysql`
- This keeps database data across container restarts/removals.

## Run

From the `docker/` directory:

```bash
docker compose up -d
```

## Verify

```bash
docker compose ps
curl http://localhost:8080
```

## Stop

```bash
docker compose down
```

## Stop and remove containers + network + volumes

```bash
docker compose down -v
```

> ⚠️ The credentials in `docker-compose.yml` are for local development only.
