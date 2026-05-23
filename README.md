# selfhost

Servicios self-hosted unificados en un solo `docker-compose.yml`.

## Orden de arranque

| Nivel | Servicios |
|-------|-----------|
| 1 | dns-server |
| 2 | pangolin, gerbil, traefik, authentik-redis, authentik-server, authentik-worker |
| 3 | transmission, flaresolverr, jackett, sonarr, radarr, bazarr, seerr |
| 4 | redis-yamtrack, yamtrack |
| 5 | meilisearch, linkwarden, actual-budget, immich (valkey+database+server+ml), mealie, paperless (broker+webserver) |

## Env vars

### Secretos (${VAR} en compose — rellenar en Portainer)

| Variable | Servicio |
|----------|----------|
| `PANGOLIN_SERVER_SECRET` | pangolin |
| `TRAEFIK_DESEC_TOKEN` | traefik |
| `AUTHENTIK_POSTGRESQL__PASSWORD` | authentik |
| `AUTHENTIK_SECRET_KEY` | authentik |
| `TRANSMISSION_PASSWORD` | transmission |
| `YAMTRACK_SECRET` | yamtrack |
| `YAMTRACK_DB_PASSWORD` | yamtrack |
| `IMMICH_DB_PASSWORD` | immich |
| `IMMICH_DB_USERNAME` | immich |
| `IMMICH_DB_DATABASE_NAME` | immich |
| `IMMICH_VERSION` | immich (default: release) |
| `MEALIE_POSTGRES_PASSWORD` | mealie |
| `OIDC_CLIENT_ID` | mealie |
| `OIDC_CLIENT_SECRET` | mealie |
| `PAPERLESS_DBPASS` | paperless |
| `PAPERLESS_SOCIALACCOUNT_PROVIDERS` | paperless |

### No sensibles (stack.env con valores por defecto)

| Variable | Servicio |
|----------|----------|
| `TZ`, `PUID`, `PGID` | general |
| `DNS_SERVER_DOMAIN`, `DNS_SERVER_FORWARDERS` | dns-server |
| `TRAEFIK_LOG_LEVEL`, `LEGO_*` | traefik |
| `AUTHENTIK_POSTGRESQL__HOST/PORT/NAME/USER`, `AUTHENTIK_REDIS__HOST` | authentik |
| `USER` | transmission |
| `AUTO_UPDATE` | jackett |
| `REDIS_URL`, `DB_HOST/NAME/USER/PORT` | yamtrack |
| `NEXTAUTH_URL` | linkwarden |
| `IMMICH_MACHINE_LEARNING_MEMORY_LIMIT` | immich |
| `ALLOW_SIGNUP`, `BASE_URL`, `DB_ENGINE`, `POSTGRES_USER/SERVER/PORT/DB`, `OIDC_*` | mealie |
| `PAPERLESS_REDIS`, `PAPERLESS_DBHOST/PORT`, `PAPERLESS_TIME_ZONE`, `PAPERLESS_OCR_*`, `PAPERLESS_URL`, `PAPERLESS_AUTO_*`, `PAPERLESS_ENABLE_*`, `PAPERLESS_PRE_CONSUME_SCRIPT` | paperless |

## Deploy

```bash
docker compose up -d
```
