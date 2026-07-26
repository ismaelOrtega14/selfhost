# selfhost

Servicios self-hosted organizados en stacks independientes para GitOps con Dockhand.

## Stacks

| Stack | Servicios | Puertos | Variables requeridas |
|---|---|---|---|
| network | dns-server, pangolin, gerbil, traefik | 53, 80, 443, 5380, 51820, 21820 | `PANGOLIN_SERVER_SECRET`, `TRAEFIK_DESEC_TOKEN` |
| uptime | uptime-kuma | 3001 | — |
| security | authentik (redis, server, worker) | 9433, 9900 | `AUTHENTIK_POSTGRESQL__PASSWORD`, `AUTHENTIK_SECRET_KEY` |
| media | transmission, flaresolverr, jackett, sonarr, radarr, bazarr, seerr | 9091, 8191, 9117, 8989, 7878, 6767, 5055 | `TRANSMISSION_PASSWORD` |
| tracker | yamtrack + redis | 8010 | `YAMTRACK_SECRET`, `YAMTRACK_DB_PASSWORD`, `YAMTRACK_HARDCOVER_KEY` |
| bookmarks | meilisearch, linkwarden | 7700, 3000 | `MEILI_MASTER_KEY`, `LINKWARDEN_NEXTAUTH_SECRET`, `LINKWARDEN_DATABASE_URL`, `LINKWARDEN_CLIENT_ID`, `LINKWARDEN_CLIENT_SECRET` |
| immich | valkey, postgres, server, ml | 2283 | `IMMICH_DB_PASSWORD`, `IMMICH_DB_USERNAME`, `IMMICH_DB_DATABASE_NAME` |
| mealie | mealie | 9925 | `MEALIE_POSTGRES_PASSWORD`, `OIDC_CLIENT_ID`, `OIDC_CLIENT_SECRET` |
| paperless | broker, webserver | 8998 | `PAPERLESS_DBPASS`, `PAPERLESS_SOCIALACCOUNT_PROVIDERS` |
| budget | actual-budget | 5006 | — |
| homepage | homepage | 3002 | — |
| obsidian | couchdb | 5984 | `COUCHDB_USER`, `COUCHDB_PASSWORD` |

## Deploy

```bash
# Stack individual
docker compose -f stacks/network/docker-compose.yml up -d

# Todos
for s in stacks/*/; do docker compose -f "$s/docker-compose.yml" up -d; done
```
