# Mise en place d'un serveur NPS (Network Policy Server) sur un Active Directory - Projet **ECORP** 🚀

## 📖 Sommaire

1. ✅ [Prérequis](#1-prerequis)
2. 📜 [Installation et Configuration du Serveur de Certificat](#2-installation-et-configuration-du-serveur-de-certificat)
3. ⚙️ [Installation du Serveur NPS](#3-installation-du-serveur-nps)
4. 🛠️ [Configuration du Serveur NPS](#4-configuration-du-serveur-nps)
5. 🔑 [Création du Groupe RADIUS et Ajout des Utilisateurs](#5-creation-du-groupe-radius-et-ajout-des-utilisateurs)
6. 🖧 [Configuration des Stratégies réseaux](#6-configuration-des-strategies-reseaux)
7. 🔍 [Vérification de la Configuration NPS](#7-verification-de-la-configuration-nps)
8. 🎯 [Conclusion](#8-conclusion)

---

## ✅ 1. Prérequis

- 🖥️ Un serveur **Windows Server 2019** avec le rôle **Active Directory Domain Services** installé et configuré.
- ⚙️ **Group Policy Management Console (GPMC)** pour la gestion des stratégies de groupe.
- 🔑 Un compte **administrateur AD** avec les droits nécessaires.
- 🖧 Un **client OpenWRT** (adresse IP 192.168.4.1) configuré pour l'authentification RADIUS.

---

## 📜 2. Installation et Configuration du Serveur de Certificat

### 🎯 Installation de l'Autorité de Certification

1. Ouvrir **Server Manager** et cliquer sur **Manage > Add Roles and Features**.
2. Sélectionner le rôle **Active Directory Certificate Services** (ADCS).
3. Installer le rôle avec les options par défaut.
4. Une fois l'installation terminée, configurez l'**Autorité de Certification** et créez un **certificat racine** auto-signé.

### 📂 Configuration du Certificat

1. Dans **Certification Authority**, faites un clic droit sur **Certificates** et choisissez **All Tasks > Request New Certificate**.
2. Suivez l'assistant pour générer un certificat valide pour l'authentification PEAP et MSCHAPv2.
 
 > *note : pour l'instant, le certificat pour radius utilisé est le racine*

---

## ⚙️ 3. Installation du Serveur NPS

1. Ouvrir **Server Manager** > **Manage > Add Roles and Features**.
2. Sélectionner **Network Policy and Access Services**.
3. Cocher **Network Policy Server** et compléter l'assistant d'installation.

---

## 🛠️ 4. Configuration du Serveur NPS

### 🎯 Ajouter le Serveur NPS au Domaine

1. Ouvrir **Network Policy Server** (`nps.msc`).
2. Sélectionner **NPS (Local)** et cliquer sur **Register Server in Active Directory**.

### 📂 Configuration des Clients RADIUS

1. Dans **NPS**, aller à **RADIUS Clients and Servers > RADIUS Clients**.
2. Ajouter un nouveau **client RADIUS** en cliquant sur **New**.
3. Configurer :
   - **Activer ce client radius** [X]
   - **Nom** : radiusopenwrt
   - **Adresse IP** : `192.168.4.1`
   - **Secret partagé** : `123`.
   - **Nom du fournisseur** : `RADIUS standard`

---

## 🔑 5. Création du Groupe RADIUS et Ajout des Utilisateurs

1. Ouvrir **Active Directory Users and Computers** (`dsa.msc`).
2. Créer un nouveau **groupe** appelé **RADIUS**.
3. Ajouter les utilisateurs autorisés dans ce groupe.

---

## 🖧 6. Configuration des Stratégies réseaux

1. Ouvrir **Network Policy Server** (`nps.msc`).
2. Aller dans **Stratégies > Stratégies réseaux** et créer une règle nommé: `Allow OpenWRT Radius`
3. Dans cette règle, dans l'onglet `vue d'ensemble` :
    - **Stratégie activé** : [X]
    - **Accorder l'accès.** : [X]
    - **Type de serveur d'accès réseau** : Non spécifié
4. Dans `Conditions` :
    - **Ajouter**
    - **Groupe windows**
    - choisir le groupe **RADIUS**
5. Dans `Contrainte` :
    - Méthodes d'authentification :
        - Types de protocole EAP : `PEAP` + `EAP-MSCHAP version 2`
        - cochez `Authentification chiffrée Microsoft version 2 MSVCHAPV2`
    - Type de port NAS :
        - Types de tunnel pour connexions 802.1X standard : Sans fil - IEE 802.11
---

### 📂 Sécuriser les Communications

- **PEAP** pour chiffrer la communication.
- **MSCHAPv2** pour l'authentification des utilisateurs.

---

## 🔍 7. Vérification de la Configuration NPS

### 📂 Vérification des Logs

1. Dans **NPS**, ouvrir **Monitoring > Accounting** pour voir les connexions RADIUS.
2. Utiliser **Event Viewer** pour inspecter les logs :
   - **Applications and Services Logs > Microsoft > Windows > NPS**
3. Tester une connexion avec un utilisateur du groupe **RADIUS**.

### 📡 Test de Connexion

1. Depuis un appareil membre du groupe RADIUS, tenter de se connecter au Wi-Fi OpenWRT.
2. Si échec, vérifier les logs et les paramètres.

---

## 🎯 8. Conclusion

Cette documentation explique l'installation et la configuration d'un serveur **NPS** sur un **Active Directory**, l'ajout d'un **client RADIUS** (OpenWRT), la création d'un **groupe RADIUS** et l'implémentation des règles d'authentification avec **PEAP** et **MSCHAPv2**. Cette configuration assure une authentification sécurisée des utilisateurs et renforce la sécurité du réseau **ECORP**.

