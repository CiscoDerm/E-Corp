# 📄 Documentation - Déploiement de node_exporter avec Docker

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
mkdir -p monitoring/node_exporter
cd monitoring/node_exporter
```

### 🔧 Étape 3 : Création du fichier `docker-compose.yml`  
```sh
vim docker-compose.yml
```
Ajoutez la configuration suivante :  
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

### 🔧 Étape 4 : Lancement du service  
```sh
docker-compose up -d
```

---

## ✅ 3. Vérification et Validation  

1. 🚀 **Vérification du conteneur**  
   ```sh
   docker ps | grep node_exporter
   ```
   ✅ Doit afficher une ligne avec **node_exporter** en état **Up**.  

2. **Vérification avec grafana**
  Vérifier que les metrics remontent
---

## 🎯 4. Conclusion  
Le service **node exporter** est maintenant installé et fonctionnel. Il permettra de monitorer notre infra ! 🚀  

---