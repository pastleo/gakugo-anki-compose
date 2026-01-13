# Gakugo Compose

## Setup

1. Copy `.env.prod` and `compose.prod.yml` and fill in the values

   ```bash
   cp compose.prod.yml compose.yml # find and set gakugo image tag
   cp .env.prod .env
   ```

2. Setup SSH key for Ollama proxy:

   ```bash
   mkdir -p ollama_proxy_data/ssh && ssh-keygen -t ed25519 -f ollama_proxy_data/ssh/id_rsa -N ""
   ```

   Then add `ollama_proxy_data/ssh/id_rsa.pub` to the remote server's `~/.ssh/authorized_keys`

3. Start the services:

   ```bash
   docker-compose up -d
   ```

4. Create database, run migrations and seed (first time only):

   ```bash
   docker compose exec -u root gakugo chown nobody /app/data
   docker compose exec gakugo bin/gakugo eval "Gakugo.Release.create"
   docker compose exec gakugo bin/gakugo eval "Gakugo.Release.migrate"
   docker compose exec gakugo bin/gakugo eval "Gakugo.Release.seed"
   ```
