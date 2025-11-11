# Troubleshooting Guide

If you run into errors after the initial setup, it's almost always due to a password mismatch or an old, broken configuration saved in a Docker volume.

### Error: `password authentication failed for user "teslamate"` (In TeslaMate logs)

This error means the `teslamate` container's password doesn't match the `database` container's password.

1.  Stop the stack: `docker compose down`
2.  Edit your `docker-compose.yml` and ensure the `DATABASE_PASS` (under `teslamate`) and `POSTGRES_PASSWORD` (under `database`) are **identical**.
3.  Remove the old, bad database volume:
    ```bash
    docker volume rm teslamate_teslamate-db
    ```
4.  Restart the stack: `docker compose up -d`
    *(Note: This will delete any data logged so far.)*

### Error: `db query error: pq: password authentication failed` (In Grafana Dashboards)

This error means the `grafana` container has a saved, broken configuration and isn't using the new, correct password from the `docker-compose.yml` file.

1.  Stop the stack: `docker compose down`
2.  Edit your `docker-compose.yml` and ensure the `DATABASE_PASS` (under `grafana`) **matches** the `POSTGRES_PASSWORD` (under `database`).
3.  Remove the old Grafana configuration volume:
    ```bash
    docker volume rm teslamate_teslamate-grafana-data
    ```
4.  Restart the stack: `docker compose up -d`
    *(Note: This will reset your Grafana login. Log back in with `admin` / `admin`.)*
