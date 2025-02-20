# 🖧 Documentation de l'Infrastructure Réseau - Configuration du Switch Netgear

---

## 📖 Sommaire
1. 🎯 [Introduction](#1-introduction)
2. 📌 [Table des VLANs](#2-table-des-vlans)
3. ⚙️ [Configuration du Switch Netgear](#3-configuration-du-switch-netgear)
4. 🔒 [Sécurisation des VLANs](#4-securisation-des-vlans)
5. ✅ [Tests et Validation](#5-tests-et-validation)

---

## 🎯 1. Introduction
L'objectif de cette documentation est de détailler la **configuration des VLANs** sur un **switch Netgear manageable 24 ports** `(IP : 192.168.0.115)`. Les **VLANs** permettent de segmenter le réseau en différentes zones sécurisées et d'optimiser la gestion du trafic. 🚀

---

## 📌 2. Table des VLANs

| VLAN | 📛 **Nom** | 🌍 **Adresse Réseau** | 📜 **Description** |
|------|-------------|----------------------|-----------------------------|
| 10   | **Admin**       | 192.168.2.x/24       | VLAN réservé aux administrateurs réseau |
| 20   | **Utilisateurs**| 192.168.3.x/24       | VLAN destiné aux utilisateurs standards |
| 30   | **Services**    | 192.168.4.x/24       | VLAN pour tous les services internes |
| 40   | **WiFi Users**  | À définir            | VLAN pour les utilisateurs WiFi |
| 50   | **WiFi Admin**  | À définir            | VLAN pour l'administration WiFi |

---

## ⚙️ 3. Configuration du Switch Netgear

### 📌 3.1 Attribution Physique des Ports
- **VLAN ADMIN** : Ports en bas à gauche.
- **VLAN USERS** : Ports en bas à droite.
- **VLAN SERVICES** : Ports en haut à droite.

### 🛠️ 3.2 Étapes de Configuration via l'Interface Web Netgear
1. 🔗 **Se connecter** à l'interface web du switch : `http://192.168.0.115`
2. 🔄 **Accéder à** `Switching > VLAN > VLAN Configuration`
3. ➕ **Créer les VLANs** `10, 20, 30, 40, 50`
4. 🖥️ **Attribuer les ports** selon le tableau suivant :

| VLAN | 🔌 **Ports Assignés** |
|------|----------------------|
| 10 (Admin) | Bas gauche |
| 20 (Users) | Bas droite |
| 30 (Services) | Haut droite |
| 40 (WiFi Users) | À définir |
| 50 (WiFi Admin) | À définir |

5. 🔄 **Configurer les ports** en mode `Access` ou `Trunk` selon les besoins.
6. 💾 **Appliquer la configuration** et effectuer des tests.

---

## 🔒 4. Sécurisation des VLANs

- 🔐 **Restreindre l'accès au VLAN Admin (10)** en appliquant des règles ACL.
- 🛑 **Isoler les VLANs utilisateurs et services** pour éviter le trafic inter-VLAN non autorisé.
- 🔑 **Activer 802.1X** pour sécuriser les accès aux VLANs sensibles.
- 🔥 **Configurer un pare-feu** pour filtrer les accès entre VLANs.

📜 **Exemple de règle ACL** pour bloquer l'accès au VLAN Admin depuis le VLAN Utilisateurs :
```shell
access-list 100 deny ip 192.168.3.0 0.0.0.255 192.168.2.0 0.0.0.255
access-list 100 permit ip any any
```

---

## ✅ 5. Tests et Validation

1. 🔎 **Vérifier l'accès réseau** : Chaque VLAN doit pouvoir accéder uniquement à ses ressources.
2. 🔄 **Tester l'assignation des ports** : Vérifier que les VLANs sont bien isolés.
3. 🚨 **Tester les ACLs** : Vérifier que les restrictions fonctionnent.
4. 📊 **Monitorer le trafic** : Utiliser `tcpdump` ou `Wireshark` pour analyser le trafic.

---

## 🎯 Conclusion
Cette documentation fournit une **vue d'ensemble complète** de la configuration des VLANs sur le **switch Netgear** `(192.168.0.115)`. Assurez-vous de **tester et sécuriser** votre infrastructure pour garantir un réseau performant et protégé. 🚀

