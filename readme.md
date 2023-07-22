# Traefik Example

Usage in an example app:

```yml
version: '3'
services:
  example_app:
      
    networks:
      - internal
      - web
    labels:
      - traefik.enable=true
      - traefik.backend=switch
      - traefik.http.services.switch.loadbalancer.server.port=3000
      - traefik.http.routers.switch.rule=Host(`subdomain.example.com`)
      # SSL configuration
      - traefik.http.routers.switch.entrypoints=https
      - traefik.http.routers.switch.tls=true
      - traefik.http.routers.switch.tls.certresolver=http
      # authentication
      - traefik.http.routers.switch.middlewares=secureHeaders@file

networks:
  internal:   
  web:
    external: true
```