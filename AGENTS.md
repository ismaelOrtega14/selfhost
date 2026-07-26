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
- Secretos como `${VAR}` inline en `environment:` (Dockhand los sustituye)
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

## Variables por stack (configurar en Dockhand Secrets)

| Stack | Variables |
|---|---|
| network | `PANGOLIN_SERVER_SECRET`, `TRAEFIK_DESEC_TOKEN` |
| security | `AUTHENTIK_POSTGRESQL__PASSWORD`, `AUTHENTIK_SECRET_KEY` |
| media | `TRANSMISSION_PASSWORD` |
| tracker | `YAMTRACK_SECRET`, `YAMTRACK_DB_PASSWORD`, `YAMTRACK_HARDCOVER_KEY` |
| bookmarks | `MEILI_MASTER_KEY`, `LINKWARDEN_NEXTAUTH_SECRET`, `LINKWARDEN_DATABASE_URL`, `LINKWARDEN_CLIENT_ID`, `LINKWARDEN_CLIENT_SECRET` |
| immich | `IMMICH_DB_PASSWORD`, `IMMICH_DB_USERNAME`, `IMMICH_DB_DATABASE_NAME` |
| mealie | `MEALIE_POSTGRES_PASSWORD`, `OIDC_CLIENT_ID`, `OIDC_CLIENT_SECRET` |
| paperless | `PAPERLESS_DBPASS`, `PAPERLESS_SOCIALACCOUNT_PROVIDERS` |
| obsidian | `COUCHDB_USER`, `COUCHDB_PASSWORD` |
