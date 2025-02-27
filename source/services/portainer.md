# Config portainer 

```yml
version: '3.9'
services:
  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    volumes:
      - portainer_data:/data
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - "PROMETHEUS=1"
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.portainer.rule=Host(`portainer.ecorp.ad`)"
      - "traefik.http.services.portainer.loadbalancer.server.port=9000"
      - "traefik.http.routers.portainer.entrypoints=https"
      - "prometheus.scrape=true"
      - "traefik.http.routers.portainer.tls=true"
        #admins
      - "traefik.http.routers.portainer.middlewares=admin-ip-whitelist"
      - "traefik.http.middlewares.admin-ip-whitelist.ipWhiteList.sourceRange=192.168.2.0/29"

    networks:
      - traefik_proxy
    restart: always

volumes:
  portainer_data:

networks:
  traefik_proxy:
    external: true
```