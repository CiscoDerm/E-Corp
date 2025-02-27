#Config prometheus

```yml
version: '3.9'
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
      #ports:
      #- "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.prometheus.rule=Host(`prometheus.ecorp.ad`)"
      - "traefik.http.services.prometheus.loadbalancer.server.port=9090"
      - "traefik.http.routers.prometheus.entrypoints=https"
      - "traefik.http.routers.prometheus.tls=true"
      #admins
      - "traefik.http.routers.prometheus.middlewares=admin-ip-whitelist"
      - "traefik.http.middlewares.admin-ip-whitelist.ipWhiteList.sourceRange=192.168.2.0/29"

    networks:
      - traefik_proxy
      - monitoring
    restart: always

volumes:
  prometheus_data:

networks:
  traefik_proxy:
    external: true
  monitoring:
    external: true
```