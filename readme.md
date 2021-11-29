# Homelab

## Installation
First you need to install **docker** in your server.
Then create a `proxy` network if you want use *traefik* reverse proxy

```bash
docker network create proxy
```

Then you can deploy single docker-compose project to enable features in your homelab server