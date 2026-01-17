# Container Management

## Setting Up

If you don't already have one create a `services`, create one in the home folder:

```bash
mkdir ~/services
```

For each service that you run, it is recommended to create a new folder with the name of that service, and create a compose file inside that folder. This will make it easy to deploy, share and update these services over time.

To manage your services, we recommend to use `podman` and `oxker`, as both are fully managed in the terminal (making management through SSH possible) and updates of podman are simpler. You can install both tools as follows:

=== "macOS"

    ``` bash
    brew install podman oxker
    ```


## N8N

### Compose File

Start by creating a folder called `n8n` and paste the following compose file below:

``` bash
mkdir ~/services/n8n
```

``` yaml title="compose.yaml"
services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    environment:
      - GENERIC_TIMEZONE=Europe/Zurich
      - NODE_ENV=production
      - N8N_SECURE_COOKIE=false
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    restart: unless-stopped

volumes:
  n8n_data:
    name: n8n_data
```

Then you can run the command:

=== "Podman"

    ``` bash
    cd ~/services/n8n
    docker-compose up -d
    ```

=== "Docker Desktop"

    ``` bash
    cd ~/services/n8n
    docker compose up -d
    ```


### Updating

Run the following commands to update the n8n container:

=== "Podman"

    ``` bash
    docker-compose pull
    docker-compose down
    docker-compose up -d
    ```

=== "Docker Desktop"

    ``` bash
    docker compose pull
    docker compose down
    docker compose up -d
    ```