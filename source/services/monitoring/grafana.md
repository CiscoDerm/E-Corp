#Config grafana

```yml
version: '3.9'
services:
  grafana:
    image: grafana/grafana
    container_name: grafana
    volumes:
      - grafana_data:/var/lib/grafana
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.grafana.rule=Host(`grafana.ecorp.ad`)"
      - "traefik.http.services.grafana.loadbalancer.server.port=3000"
      - "traefik.http.routers.grafana.entrypoints=https"
      - "traefik.http.routers.grafana.tls=true"
      #admins
      - "traefik.http.routers.grafana.middlewares=admin-ip-whitelist"
      - "traefik.http.middlewares.admin-ip-whitelist.ipWhiteList.sourceRange=192.168.2.0/29"
    networks:
      - traefik_proxy
      - monitoring
    restart: always
    mem_limit: 10g
    cpus: 2
      #environment:
      #- GF_ALERTING_INCLUDE_LINK=false
volumes:
  grafana_data:

networks:
  traefik_proxy:
    external: true
  monitoring:
    external: true
```