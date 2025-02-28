# 📄 Documentation - Déploiement de Grafana avec Docker

---

## 📚 Sommaire  
1. 🎯 [Introduction](#1-introduction)  
2. 📌 [Installation et Configuration](#2-installation-et-configuration)  
3. ✅ [Vérification et Validation](#3-verification-et-validation)  
4. 🎯 [Conclusion](#4-conclusion)  

---

## 🎯 1. Introduction  
L'objectif de cette documentation est de déployer et configurer GRAFANA sur le serveur **Ubuntu 22.04**. Chaque service sera installé via **docker-compose** et géré via **Docker**.

---

## 📌 2. Installation et Configuration  

### 🔧 Étape 1 : Connexion au serveur  
```sh
ssh ecorp@192.168.4.4
```

### 🔧 Étape 2 : Création du dossier du service  
```sh
cd ~/ecorp
mkdir -p monitoring/grafana
cd monitoring/grafana
```

### 🔧 Étape 3 : Création du fichier `docker-compose.yml`  
```sh
vim docker-compose.yml
```
Ajoutez la configuration suivante :  
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

### 🔧 Étape 4 : Lancement du service  
```sh
docker-compose up -d
```

---

## ✅ 3. Vérification et Validation  

1. 🚀 **Vérification du conteneur**  
   ```sh
   docker ps | grep grafana
   ```
   ✅ Doit afficher une ligne avec **grafana** en état **Up**.  

2. 🌍 **Accès à l'interface Web**  
   - Ouvrir un navigateur et aller sur `https://grafana.ecorp.ad`  

---

## 🎯 4. Conclusion  
Le service **Grafana** est maintenant installé et fonctionnel. Il permettra de monitorernotre infra ! 🚀  

---