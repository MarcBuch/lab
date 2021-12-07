# Traefik

Traefik is a reverse proxy and load balancer. It was built with cluster technologies like Docker and Kubernetes in mind. There is no need to maintain a configuration file, since Traefik updates the routes by integrating into the cluster software.

## Getting Started

1. `sudo mkdir /etc/traefik`
2. `sudo cp traefik-config.yml /etc/traefik/config.yml`
3. `docker-compose up -d`

## Caveats for production environments

Modifications to the Traefik config are required to use this setup in production.

- Do not simply expose the dashboard. It has no authentication mechanism.
- SSL certificates are required in todays world.

## Exposing a service with Traefik

First, you need an entrypoint defined in the Traefik config. This is used to expose the service.

```YAML
# traefik.yml
...
entryPoints:
    web:
        address: 80
...
```

Then you can add labels to your docker container. **Make sure that your service is on the same docker network as Traefik.**

```YAML
# docker-compose.yml
...
nginx:
    image: nginx:latest
    name: nginx
    labels:
        - "traefik.enable=true"
        - "traefik.http.routers.nginx.entrypoints=web"
        - "traefik.http.routers.nginx.rule=Host(`nginx.local.buchardtprivat.de`)"
...
```

## References

- [Traefik Docs](https://doc.traefik.io/traefik/)
