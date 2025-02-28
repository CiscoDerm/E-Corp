# 📄 Documentation - Déploiement de portainer avec Docker

---

## 📚 Sommaire  
1. 🎯 [Introduction](#1-introduction)  
2. 📌 [Installation et Configuration](#2-installation-et-configuration)  
3. ✅ [Vérification et Validation](#3-verification-et-validation)  
4. 🎯 [Conclusion](#4-conclusion)  

---

## 🎯 1. Introduction  
L'objectif de cette documentation est de déployer et configurer les services Docker sur le serveur **Ubuntu 22.04**. Chaque service sera installé via **docker-compose** et géré via **Docker**.

---

## 📌 2. Installation et Configuration  

### 🔧 Étape 1 : Connexion au serveur  
```sh
ssh ecorp@192.168.4.4
```

### 🔧 Étape 2 : Création du dossier du service  
```sh
cd ~/ecorp
mkdir portainer
cd portainer
```

### 🔧 Étape 3 : Création du fichier `docker-compose.yml`  
```sh
vim docker-compose.yml
```
Ajoutez la configuration suivante :  
```yaml

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

### 🔧 Étape 4 : Lancement du service  
```sh
docker-compose up -d
```

---

## ✅ 3. Vérification et Validation  

1. 🚀 **Vérification du conteneur**  
   ```sh
   docker ps | grep portainer
   ```
   ✅ Doit afficher une ligne avec **portainer** en état **Up**.  

2. 🌍 **Accès à l'interface Web**  
   - Ouvrir un navigateur et aller sur `https://portainer.ecorp.ad`  
   - Suivre l’assistant de configuration (création du premier compte).  

---

## 🎯 4. Conclusion  
Le service **Portainer** est maintenant installé et fonctionnel. Il permettra d’administrer et superviser les conteneurs Docker via une interface web. 🚀  

---