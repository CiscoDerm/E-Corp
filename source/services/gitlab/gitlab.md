# Configuration de GitLab

---

## 📖 Sommaire
1. 🎯 [Création du répertoire gitlab](#1-creation-du-repertoire-gitlab)
2. 📌 [Création du fichier docker-compose](#2-creation-du-fichier-docker-compose)
3. 🛠 [Remplacement du mot de passe](#3-remplacement-du-mot-de-passe)
4. 🔗 [Démarrage du conteneur gitlab](#4-demarrage-du-conteneur-gitlab)
5. ✅ [Vérification](#5-verification)
6. 🎯 [Conclusion](#6-conclusion)

---
## 1. Création du répertoire GitLab

```sh
mkdir gitlab
cd gitlab
```

## 2. Création du fichier docker-compose

Créez et ouvrez le fichier `docker-compose.yml` avec votre éditeur de texte préféré :

```sh
vim docker-compose.yml
```

Copiez-collez la configuration suivante :

```yml
version: '3.9'
services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    container_name: gitlab
    dns:
      - 192.168.4.3
    volumes:
      - gitlab_config:/etc/gitlab
      - gitlab_logs:/var/log/gitlab
      - gitlab_data:/var/opt/gitlab
    environment:
      GITLAB_OMNIBUS_CONFIG:  |
        external_url 'http://gitlab.ecorp.ad'
        gitlab_rails['ldap_enabled'] = true
        gitlab_rails['ldap_servers'] = YAML.load <<-EOS
        main:
          label: 'ECorp AD'
          host: 'ecorpad.ecorp.ad'
          port: 389
          uid: 'sAMAccountName'
          bind_dn: 'CN=ecorp,CN=Users,DC=ecorp,DC=ad'
          password: 'USER_PASSWORD'
          encryption: 'plain'
          active_directory: true
          lowercase_usernames: false
          allow_username_or_email_login: false
          base: 'CN=Users,DC=ecorp,DC=ad'
          user_filter: ''
          group_base: 'CN=Groups,DC=ecorp,DC=ad'
          admin_group: 'GitLab Admins'
          sync_ssh_keys: false
        EOS
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.gitlab.rule=Host(`gitlab.ecorp.ad`)
      - "traefik.http.services.gitlab.loadbalancer.server.port=80"
      - "traefik.http.routers.gitlab.entrypoints=http"
    networks:
      - traefik_proxy
      - gitlab_network
    restart: always

volumes:
  gitlab_config:
  gitlab_logs:
  gitlab_data:

networks:
  traefik_proxy:
    external: true
  gitlab_network:
    external: true
```

## 3. Remplacement du mot de passe

N'oubliez pas de remplacer `USER_PASSWORD` par le mot de passe de l'utilisateur LDAP.

## 4. Démarrage du conteneur GitLab

Lancez GitLab avec la commande suivante :

```sh
docker-compose up -d
```

## 5. Vérification

```sh
docker ps
https://gitlab.ecorp.ad
```

## 6. Conclusion

Notre **GitLab** est maintenant fonctionnel 🚀. Nous pouvons nous connecter grace aux identifiant LDAP !
