# Traefik Example

Usage in an example app:

```yml
version: '3'
services:
  example_app:
    
    ...

    networks:
      - internal
      - web
    labels:
      - traefik.enable=true
      - traefik.backend=example_app
      - traefik.http.services.example_app.loadbalancer.server.port=3000
      - traefik.http.routers.example_app.rule=Host(`subdomain.example.com`)
      # SSL configuration
      - traefik.http.routers.example_app.entrypoints=https
      - traefik.http.routers.example_app.tls=true
      - traefik.http.routers.example_app.tls.certresolver=http
      - traefik.http.routers.example_app.middlewares=secureHeaders@file

networks:
  internal:   
  web:
    external: true
```

Portainer example:

```yml
version: "3"

services:
  portainer:
    container_name: portainer
    image: portainer/portainer:latest
    restart: unless-stopped
    volumes:
      - ./data:/data
      - /var/run/docker.sock:/var/run/docker.sock
    expose:
      - "9000"
    networks:
      - web
    labels:
      - traefik.enable=true
      - traefik.backend=portainer
      - traefik.http.services.portainer.loadbalancer.server.port=9000
      - traefik.http.routers.portainer.rule=Host(`portainer.example.com`)
      # SSL configuration
      - traefik.http.routers.portainer.entrypoints=https
      - traefik.http.routers.portainer.tls=true
      - traefik.http.routers.portainer.tls.certresolver=http

networks:
  web:
    external: true

```