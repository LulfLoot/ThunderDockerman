# Automation Plan: Containerized Server Manager

1. Create the Manager Service
   We will create a new directory `server-manager` in the project root containing:
   * `server_manager.py`: The Python script using Flask and the Docker SDK. It will handle the webhook logic (start timer on empty, stop server) and the start command.
   * `requirements.txt`: Lists dependencies (flask, docker).
   * `Dockerfile`: A lightweight Python image setup.

2. Update ThunderModMan Configuration
   We will modify `docker-compose.yml` to:
   * Add the `server-manager` service:
       * Builds from the `./server-manager` directory.
       * Mounts `/var/run/docker.sock` so it can control the `valheim-server` container.
       * Exposes port 5000.
   * Configure Valheim:
       * Set `WEBHOOK_STATUS_URL=http://server-manager:5000/webhook`.
       * This allows the Valheim server to talk directly to the manager over the internal Docker network.

3. Verification
   * We will verify the new compose config.
   * You will be able to start the server via `http://localhost:5000/start`.
