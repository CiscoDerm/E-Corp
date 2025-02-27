# config exporter

```yml
version: '3.9'
services:
  node_exporter:
    image: prom/node-exporter:latest
    container_name: node_exporter
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.node_exporter.rule=Host(`node-exporter.ecorp.ad`)"
      - "traefik.http.services.node_exporter.loadbalancer.server.port=9100"
      - "traefik.http.routers.node_exporter.entrypoints=http"
    networks:
      - traefik_proxy
      - monitoring
    restart: always

networks:
  traefik_proxy:
    external: true
  monitoring:
    external: true
```