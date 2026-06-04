# AGENTS.md — selfhost

Repositorio unificado de servicios self-hosted gestionado con Docker Compose v2 (no Swarm).

## Estructura

- **Un solo stack** en `main` con todos los servicios en `docker-compose.yml`
- Sin `.env` en el repo — secretos como `${VAR}` sustituidos por Portainer en el deploy
- Valores no sensibles como **literales** en `environment:` (no `env_file`)

## Servicios (29)

| Nivel | Servicios |
|-------|-----------|
| 1 | `dns-server`, `uptime-kuma` |
| 2 | `authentik-redis`, `authentik-server`, `authentik-worker` |
| 3 | `transmission`, `flaresolverr`, `jackett`, `sonarr`, `radarr`, `bazarr`, `seerr` |
| 4 | `redis-yamtrack`, `yamtrack` |
| 5 | `meilisearch`, `linkwarden`, `actual-budget`, `immich-*`, `mealie`, `paperless-*`, `homepage` |
| 6 | `pangolin`, `gerbil`, `traefik` |

## Orden de arranque

Definido mediante `depends_on` con healthchecks. Cada nivel depende de `dns-server` + servicio(s) del nivel anterior. Nivel 6 (reverse proxy) arranca el último para que todos los servicios estén ya operativos.

## Convenciones

- No usar `version:` en compose.yml (obsoleto en Compose v2)
- Puerto en formato corto: `"host:container/protocol"` (ej: `"3002:3000/tcp"`)
- `restart: unless-stopped` en todos los servicios
- Healthchecks según herramientas disponibles en cada imagen: `wget`, `curl`, `node`, `python3`, `kill -0`, comando nativo
- Servicios con monturas NFS (`uptime-kuma`, `homepage`): `entrypoint: []` para evitar `chown` fallido
- Acceso a socket Docker: `group_add: ["992"]` montando `/var/run/docker.sock`

## Healthchecks

| Servicio | Healthcheck |
|----------|-------------|
| dns-server | `curl -sf http://127.0.0.1:5380` |
| uptime-kuma | `node` con `http.get` |
| authentik-server | `python3` contra `/-/health/ready/` |
| authentik-worker | `python3` contra authentik-server |
| transmission | `curl -s` (acepta 401 como servidor vivo) |
| flaresolverr | `curl -sf /health` |
| jackett, sonarr, radarr, bazarr | `curl -s` |
| seerr | `node` con `http.get` |
| yamtrack | `curl -sf /health` |
| linkwarden | `node` con `http.get` |
| actual-budget | `curl -s` |
| immich-server | `node` con timeout 15s |
| immich-machine-learning | built-in (`python3 healthcheck.py`) |
| mealie | `timeout 10s bash -c ':> /dev/tcp/127.0.0.1/9000'` |
| paperless-broker | `redis-cli ping | grep PONG` |
| paperless-webserver | `curl -s` |
| homepage | `wget -qO- http://127.0.0.1:3000/api/healthcheck` |
| pangolin | `kill -0 1` |
| gerbil | `wget -qO- http://127.0.0.1:3003/api/v1/config` |
| traefik | `traefik version` |

## Comandos

```bash
docker compose up -d                        # Deploy completo
docker compose up -d <service>              # Servicio específico
docker compose logs -f <service>            # Logs
docker compose ps                           # Estado
docker compose pull <service>               # Actualizar imagen
```

## Puertos destacados

| Puerto | Servicio |
|--------|----------|
| 5380 | Technitium DNS |
| 3001 | Uptime Kuma |
| 80/443 | Traefik |
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
| 3002 | Homepage |
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
