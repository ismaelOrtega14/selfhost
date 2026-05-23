# selfhost

Servicios self-hosted unificados en un solo `docker-compose.yml`.

## Orden de arranque

| Nivel | Rama    | Servicios                          |
|-------|---------|------------------------------------|
| 1     | network | dns-server                         |
| 2     | security| pangolin, gerbil, traefik, authentik |
| 3     | notflix | transmission, sonarr, radarr, jackett, bazarr, flaresolverr, seerr |
| 4     | tracker | yamtrack, redis-yamtrack           |
| 5     | resto   | linkwarden, meilisearch, actual-budget, immich, shepherd, mealie, n8n, paperless |

## Deploy

```bash
docker compose up -d
```

Las variables de entorno se gestionan desde Portainer (stack.env).
