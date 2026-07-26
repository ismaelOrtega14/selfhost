# AGENTS.md — selfhost

Repositorio de servicios self-hosted organizados en stacks independientes.
Gestionado con Dockhand (GitOps) via GitHub.

## Estructura

```
stacks/
├── network/         dns-server, pangolin, gerbil, traefik
├── uptime/          uptime-kuma
├── security/        authentik (redis, server, worker)
├── media/           transmission, flaresolverr, jackett, sonarr, radarr, bazarr, seerr
├── tracker/         yamtrack + redis
├── bookmarks/       meilisearch, linkwarden
├── immich/          valkey, postgres, server, ml
├── mealie/          mealie
├── paperless/       broker, webserver
├── budget/          actual-budget
├── homepage/        homepage
└── obsidian/        couchdb
```

## Reglas

- Cero dependencias cruzadas entre stacks
- `depends_on` solo dentro del mismo stack
- Secretos en `stack.env` (no commit), referenciado via `env_file: stack.env`
- Valores no sensibles como literales en `environment:`
- `restart: unless-stopped` en todos los servicios
- Healthchecks según herramientas disponibles en cada imagen
- Servicios con monturas NFS: `entrypoint: []`
- Acceso a socket Docker: `group_add: ["992"]`
- Puerto en formato corto: `"host:container/protocol"`

## Comandos

```bash
# Stack individual
docker compose -f stacks/network/docker-compose.yml up -d
docker compose -f stacks/media/docker-compose.yml up -d

# Logs de un servicio
docker compose -f stacks/media/docker-compose.yml logs -f sonarr

# Todos los stacks (orden recomendado)
for s in network uptime security media tracker bookmarks immich mealie paperless budget homepage obsidian; do
  docker compose -f stacks/$s/docker-compose.yml up -d
done
```

## Stack.env (no commit)

Cada stack tiene `stack.env.example` con las variables documentadas.
Copiar a `stack.env` y rellenar secretos antes de desplegar:

```bash
cp stacks/network/stack.env.example stacks/network/stack.env
# editar stack.env con valores reales
```
