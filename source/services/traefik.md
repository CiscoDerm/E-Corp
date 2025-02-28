# 📄 Documentation - Déploiement de traefik avec Docker

---

## 📚 Sommaire  
1. 🎯 [Introduction](#1-introduction)  
2. 📌 [Installation et Configuration](#2-installation-et-configuration)  
3. ✅ [Vérification et Validation](#3-verification-et-validation)  
4. 🎯 [Conclusion](#4-conclusion)  

---

## 🎯 1. Introduction  
L'objectif de cette documentation est de déployer et configurer traefik sur le serveur **Ubuntu 22.04**. Chaque service sera installé via **docker-compose** et géré via **Docker**.

---

## 📌 2. Installation et Configuration  

### 🔧 Étape 1 : Connexion au serveur  
```sh
ssh ecorp@192.168.4.4
```

### 🔧 Étape 2 : Création du dossier du service  
```sh
cd ~/ecorp
mkdir traefik
cd traefik
```

### 🔧 Étape 3 : Création du fichier `docker-compose.yml`  
```sh
vim docker-compose.yml
```
Ajoutez la configuration suivante :  
```yml
version: '3.9'
services:
  traefik:
    image: traefik:latest
    container_name: traefik
    ports:
      - "80:80"     # HTTP
      - "443:443"   # HTTPS
      - "8082:8082"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik_data:/etc/traefik
    command:
      - "--api.insecure=true" # Pour activer le dashboard (ne pas utiliser en production sans sécurisation)
      - "--providers.docker=true"
      - "--log.level=DEBUG"
      - "--entrypoints.http.address=:80"      # HTTP
      - "--entrypoints.https.address=:443"   # HTTPS
      - "--entryPoints.metrics.address=:8082"
      - "--metrics.prometheus=true"
      - "--metrics.prometheus.entryPoint=metrics"
      - "--metrics.prometheus.buckets=0.1,0.3,1.2,5.0"
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.dashboard.rule=Host(`traefik.ecorp.ad`)"
      - "traefik.http.routers.dashboard.service=api@internal"
      - "traefik.http.routers.dashboard.entrypoints=https"
      - "traefik.http.routers.traefik.rule=Host(`traefik.local`)"
      - "traefik.http.routers.dashboard.tls=true"
      # Autoriser les admins ( marche pas encore jcp pk)
      #- "traefik.http.routers.traefik.middlewares=admin-ip-whitelist"
      #- "traefik.http.middlewares.admin-ip-whitelist.ipWhiteList.sourceRange=192.168.2.0/29"
    restart: always
    networks:
      - traefik_proxy
      - gitlab_network
      - monitoring
volumes:
  traefik_data:

networks:
  traefik_proxy:
    external: true
  gitlab_network:
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
   docker ps | grep traefik
   ```
   ✅ Doit afficher une ligne avec **traefik** en état **Up**.  

2. 🌍 **Accès à l'interface Web**  
   - Ouvrir un navigateur et aller sur `https://traefik.ecorp.ad`  

---

## 🎯 4. Conclusion  
Le service **Traefik** est maintenant installé et fonctionnel. Il permettra de faire le routage pour les services ! 🚀  

---
