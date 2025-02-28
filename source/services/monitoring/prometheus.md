# 📄 Documentation - Déploiement de prometheus avec Docker

---

## 📚 Sommaire  
1. 🎯 [Introduction](#1-introduction)  
2. 📌 [Installation et Configuration](#2-installation-et-configuration)  
3. ✅ [Vérification et Validation](#3-verification-et-validation)  
4. 🎯 [Conclusion](#4-conclusion)  

---

## 🎯 1. Introduction  
L'objectif de cette documentation est de déployer et configurer node exporter sur le serveur **Ubuntu 22.04**. Chaque service sera installé via **docker-compose** et géré via **Docker**.

---

## 📌 2. Installation et Configuration  

### 🔧 Étape 1 : Connexion au serveur  
```sh
ssh ecorp@192.168.4.4
```

### 🔧 Étape 2 : Création du dossier du service  
```sh
cd ~/ecorp
mkdir -p monitoring/prometheus
cd monitoring/prometheus
```

### 🔧 Étape 3 : Création du fichier `docker-compose.yml`  
```sh
vim docker-compose.yml
```
Ajoutez la configuration suivante :  
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

Créer prometheus.yml
```yml
global:
  scrape_interval: 30s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node_exporter:9100']

  - job_name: 'docker'
    static_configs:
      - targets: ['docker_exporter:9323']

  - job_name: 'portainer'
    static_configs:
      - targets: ['portainer:9000']

  - job_name: 'traefik'
    metrics_path: '/metrics'
    static_configs:
      - targets: ['192.168.64.5:8082']
```

### 🔧 Étape 4 : Lancement du service  
```sh
docker-compose up -d
```

---

## ✅ 3. Vérification et Validation  

1. 🚀 **Vérification du conteneur**  
   ```sh
   docker ps | grep prometheus
   ```
   ✅ Doit afficher une ligne avec **prometheus** en état **Up**.  

2. **Vérification avec grafana**
  FAUT CONFIG LA DATASOURCE VIA GRAFANA AUSSI
---

## 🎯 4. Conclusion  
Le service **Prometheus** est maintenant installé et fonctionnel. Il permettra de monitorer notre infra ! 🚀  

---