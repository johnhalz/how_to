# Container Management

## Setting Up

## N8N

### Compose File

``` yaml title="n8n.yaml"
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

### Updating

Run the following commands to update the n8n container:

``` bash
docker compose -f n8n.yaml pull
docker compose -f n8n.yaml down
docker compose -f n8n.yaml up -d
```