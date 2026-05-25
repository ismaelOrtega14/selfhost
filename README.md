# selfhost

Servicios self-hosted unificados en un solo `docker-compose.yml`.

## Orden de arranque

| Nivel | Servicios |
|-------|-----------|
| 1 | dns-server, uptime-kuma |
| 2 | authentik-redis, authentik-server, authentik-worker |
| 3 | transmission, flaresolverr, jackett, sonarr, radarr, bazarr, seerr |
| 4 | redis-yamtrack, yamtrack |
| 5 | meilisearch, linkwarden, actual-budget, immich-*, mealie, paperless-*, homepage |
| 6 | pangolin, gerbil, traefik |

## Puertos destacados

| Puerto | Servicio |
|--------|----------|
| 5380 | Technitium DNS |
| 3001 | Uptime Kuma |
| 3002 | Homepage |
| 9091 | Transmission |
| 8191 | Flaresolverr |
| 9117 | Jackett |
| 8989 | Sonarr |
| 7878 | Radarr |
| 6767 | Bazarr |
| 5055 | Seerr |
| 8010 | Yamtrack |
| 3000 | Linkwarden |
| 5006 | Actual Budget |
| 2283 | Immich |
| 9925 | Mealie |
| 8998 | Paperless |
| 51820/udp | Gerbil (WireGuard) |

## Secretos (${VAR} en compose — rellenar en Portainer)

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
| `LINKWARDEN_NEXTAUTH_SECRET` | linkwarden |
| `LINKWARDEN_DATABASE_URL` | linkwarden |
| `LINKWARDEN_CLIENT_ID` | linkwarden |
| `LINKWARDEN_CLIENT_SECRET` | linkwarden |

## Deploy

```bash
docker compose up -d
```
