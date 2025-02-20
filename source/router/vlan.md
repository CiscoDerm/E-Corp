# 🖧 Documentation de l'Infrastructure Réseau - VLANs

---

## 📖 Sommaire
1. 🎯 [Introduction](#-1-introduction)
2. 📌 [Table des VLANs](#-2-table-des-vlans)
3. ⚙️ [Configuration des VLANs](#-3-configuration-des-vlans)
   - 🖥️ [Via OpenWRT (Routeur Linksys)](#-31-configuration-des-vlans-via-openwrt-routeur-linksys)
   - 🏢 [Via le Switch Netgear](#-32-configuration-du-switch-netgear)
4. 🔒 [Sécurisation des VLANs](#-4-sécurisation-des-vlans)
5. ✅ [Tests et Validation](#-5-tests-et-validation)
6. 🎯 [Conclusion](#-6-conclusion)

---

## 🎯 1. Introduction
L'objectif de cette documentation est de détailler la **configuration des VLANs** sur notre infrastructure réseau basée sur un **routeur Linksys sous OpenWRT** et un **switch Netgear manageable 24 ports** `(IP : 192.168.0.115)`. Les **VLANs** permettent de segmenter le réseau en différentes zones sécurisées et d'optimiser la gestion du trafic. 🚀

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

## ⚙️ 3. Configuration des VLANs

### 🖥️ 3.1 Configuration des VLANs via OpenWRT (Routeur Linksys)
1. 🔗 **Se connecter** à l'interface web d'**OpenWRT**.
2. 🔄 **Accéder à** `Network > Switch`.
3. ➕ **Ajouter les VLANs** en respectant les informations ci-dessus.
4. 🖥️ **Affecter les ports** selon la configuration souhaitée.
5. 💾 **Sauvegarder et appliquer** les modifications.

### 🏢 3.2 Configuration du Switch Netgear (IP : 192.168.0.115)

#### 📌 Attribution Physique des Ports
- **VLAN ADMIN** : Ports en bas à gauche.
- **VLAN USERS** : Ports en bas à droite.
- **VLAN SERVICES** : Ports en haut à droite.

#### 🛠️ Étapes de Configuration via l'Interface Web Netgear
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

### 🏷️ 3.3 Attribution des Adresses IP et DHCP
- ⚙️ **Activer un serveur DHCP** sur chaque VLAN si nécessaire.
- 🔄 **Configurer les plages d'adresses** pour éviter les conflits IP.
- ✅ **Vérifier l'attribution automatique** des adresses IP selon les VLANs.

📜 **Exemple de configuration DHCP sur OpenWRT** :
```shell
config dhcp 'vlan10'
    option interface 'vlan10'
    option start '100'
    option limit '50'
    option leasetime '12h'
```

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
2. 🔄 **Tester le DHCP** : Vérifier que chaque VLAN reçoit bien son adresse IP.
3. 🚨 **Tester les ACLs** : Vérifier que les restrictions fonctionnent.
4. 📊 **Monitorer le trafic** : Utiliser `tcpdump` ou `Wireshark` pour analyser le trafic.

---

## 🎯 6. Conclusion
Cette documentation fournit une **vue d'ensemble complète** de la configuration des VLANs sur **OpenWRT (routeur Linksys)** et le **switch Netgear** `(192.168.0.115)`. Assurez-vous de **tester et sécuriser** votre infrastructure pour garantir un réseau performant et protégé. 🚀
