# ⚙️ Configuration du GitLab Runner

---

## 📖 Sommaire
1. 🎯 [Introduction](#1-introduction)
2. 📌 [Création du Runner](#2-creation-du-runner)
3. 🛠 [Déploiement du Runner](#3-deploiement-du-runner)
4. 🔗 [Connexion du Runner à GitLab](#4-connexion-du-runner-a-gitlab)
5. ✅ [Vérification](#5-verification)
6. 🎯 [Conclusion](#6-conclusion)

---

## 🎯 1. Introduction
Le but de cette documentation est de configurer un **GitLab Runner** auto-hébergé qui s'intégrera avec **GitLab** pour exécuter des pipelines CI/CD.

Le GitLab Runner sera déployé dans un **conteneur Docker** et se connectera au serveur GitLab.

---

## 📌 2. Création du Runner

1. Accédez à l'interface d'administration de GitLab :
   - **URL** : [http://gitlab.ecorp.ad/admin/runners](http://gitlab.ecorp.ad/admin/runners)
2. Cliquez sur **Créer un Runner**
3. Définissez :
   - **Un tag** pour identifier le runner
   - **Une description**
4. Copiez le **token d'enregistrement** généré (il sera utilisé plus tard)

---

## 🛠 3. Déploiement du Runner

1. **Se connecter en SSH au serveur :**
   ```sh
   ssh ecorp@192.168.4.4
   ```
2. **Créer le dossier du service :**
   ```sh
   mkdir -p ~/ecorp/gitlab_runner
   cd ~/ecorp/gitlab_runner
   ```
3. **Créer et éditer le fichier `docker-compose.yml` :**
   ```sh
   vim docker-compose.yml
   ```
4. **Ajouter la configuration suivante :**
   ```yaml
   version: '3.9'
   services:
     gitlab-runner:
       image: gitlab/gitlab-runner:latest
       container_name: gitlab-runner
       volumes:
         - /var/run/docker.sock:/var/run/docker.sock
         - gitlab_runner_config:/etc/gitlab-runner
       restart: always
       networks:
         - gitlab_network
   volumes:
     gitlab_runner_config:
   networks:
     gitlab_network:
       external: true
   ```
5. **Démarrer le conteneur :**
   ```sh
   docker-compose up -d
   ```

---

## 🔗 4. Connexion du Runner à GitLab

1. Exécutez la commande suivante pour enregistrer le Runner :
   ```sh
   gitlab-runner register --url http://gitlab.ecorp.ad --token VOTRE_TOKEN
   ```
2. Un dialogue interactif s'ouvre :
   - Appuyez sur **Entrer**
   - Donner un nom : (ex runnerX) 
   - **Confirmez les autres options** (par défaut, les paramètres fonctionnent bien)
3. Vérifiez que le Runner est bien **"Registered"** dans l'interface GitLab.

---

## ✅ 5. Vérification

1. **Vérifier que le conteneur tourne correctement :**
   ```sh
   docker ps | grep gitlab-runner
   ```
2. **Tester le runner en lançant une pipeline GitLab.**

---

## 🎯 6. Conclusion

Notre **GitLab Runner** est maintenant fonctionnel et prêt à exécuter des pipelines CI/CD 🚀. Assurez-vous de **gérer correctement les permissions** et **de surveiller son fonctionnement** via l'interface GitLab.

