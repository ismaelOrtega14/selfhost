# AGENTS.md — selfhost

Repositorio unificado de servicios self-hosted gestionado con Docker Compose v2 (no Swarm).

## Estructura

- **Un solo stack** en `main` con todos los servicios en `docker-compose.yml`
- Las ramas por servicio (`network`, `security`, `notflix`, `tracker`, etc.) se mantienen como histórico pero no se usan para deploy
- Las variables de entorno se gestionan desde Portainer con `stack.env` (no hay `.env` en el repo)
- Sin secrets de Swarm — `env_file: stack.env` + secretos como `${VAR}` sustituidos por Portainer

## Orden de arranque

Definido mediante `depends_on` con healthchecks, en 5 niveles:

| Nivel | Servicios clave                                           |
|-------|-----------------------------------------------------------|
| 1     | `dns-server` (Technitium DNS)                            |
| 2     | `pangolin`, `gerbil`, `traefik`, `authentik`             |
| 3     | `transmission`, `flaresolverr`, `jackett`, `sonarr`, `radarr`, `bazarr`, `seerr` |
| 4     | `yamtrack`, `redis-yamtrack`                             |
| 5     | `linkwarden`, `meilisearch`, `actual-budget`, `immich`, `mealie`, `paperless` |

## Comandos

```bash
docker compose up -d                        # Deploy completo
docker compose up -d <service>              # Servicio específico
docker compose logs -f <service>            # Logs
docker compose ps                           # Estado
```

## Convenciones

- No usar `version:` en compose.yml (obsoleto en Compose v2)
- Puerto en formato corto: `"host:container/protocol"` (ej: `"3000:3000/tcp"`)
- `restart:` en vez de `deploy.restart_policy`
- No usar `deploy.replicas`, `mode: host`, ni `external: true` en secrets
- Overlay networks convertidas a bridge

## Stack.env (Portainer)

- Cada servicio carga `env_file: stack.env`
- Las rutas `/run/secrets/...` del Swarm original se convierten a variables directas
- Secretos (passwords, tokens) en `environment:` con `${VAR}` — Portainer los sustituye en el deploy
