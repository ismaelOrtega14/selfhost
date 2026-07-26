# selfhost

Servicios self-hosted organizados en stacks independientes para GitOps con Dockhand.

## Stacks

| Stack | Servicios | Puertos |
|-------|-----------|---------|
| network | dns-server, pangolin, gerbil, traefik | 53, 80, 443, 5380, 51820, 21820 |
| uptime | uptime-kuma | 3001 |
| security | authentik (redis, server, worker) | 9433, 9900 |
| media | transmission, flaresolverr, jackett, sonarr, radarr, bazarr, seerr | 9091, 8191, 9117, 8989, 7878, 6767, 5055 |
| tracker | yamtrack + redis | 8010 |
| bookmarks | meilisearch, linkwarden | 7700, 3000 |
| immich | valkey, postgres, server, ml | 2283 |
| mealie | mealie | 9925 |
| paperless | broker, webserver | 8998 |
| budget | actual-budget | 5006 |
| homepage | homepage | 3002 |
| obsidian | couchdb | 5984 |

## Secretos

Los secretos van en `stacks/<stack>/stack.env` (no commit).
Usar `stack.env.example` como plantilla:

```bash
cp stacks/network/stack.env.example stacks/network/stack.env
```

## Deploy

```bash
# Stack individual
docker compose -f stacks/network/docker-compose.yml up -d

# Todos
for s in stacks/*/; do docker compose -f "$s/docker-compose.yml" up -d; done
```
