# 🖧 Documentation de l'Infrastructure Réseau - Configuration du routeur OpenWRT

---

## 📖 Sommaire
1. 🎯 [Introduction](#1-introduction)
2. 📌 [Table des VLANs](#2-table-des-reseaux-ip-des-vlans)
3. ⚙️ [Configuration des Interfaces](#3-configuration-des-interfaces)
   - 🖥️ [Création des devices)](#31-creation-des-devices)
   - 🖥️ [Création des interfaces)](#32-creation-des-interfaces)
   -  [Configuration du DHCP](#33-configuration-dhcp)
4. 🔒 [Sécurisation des VLANs](#4-securisation-des-vlans)
5. ✅ [Tests et Validation](#5-tests-et-validation)
6. 🎯 [Conclusion](#6-conclusion)

---

## 🎯 1. Introduction
L'objectif de cette documentation est de détailler la **configuration des Interfaces** associé aux **VLANS** sur notre infrastructure réseau basée sur un **routeur Linksys sous OpenWRT**. Les **VLANs** permettent de segmenter le réseau en différentes zones sécurisées et d'optimiser la gestion du trafic. 🚀

---

## 📌 2. Table des réseaux IP des VLANs

| VLAN | 📛 **Nom** | 🌍 **Adresse Réseau** | **Adresse Interface** |📜 **Description** |
|------|-------------|------------------|------------------------|-----------------------------|
| 2   | **Admin**       | 192.168.2.0/24     | 192.168.2.1     | VLAN réservé aux administrateurs réseau |
| 3   | **Utilisateurs**| 192.168.3.0/24     | 192.168.3.1     | VLAN destiné aux utilisateurs standards |
| 4   | **Services**    | 192.168.4.0/24     | 192.168.4.1     | VLAN pour tous les services internes |
| 5   | **WiFi Admin**  | 192.168.5.0/24     | 192.168.5.1     | VLAN pour l'administration WiFi |
| 6   | **WiFi Users**  | 192.168.6.0/24     | 192.168.6.1     | VLAN pour les utilisateurs WiFi |

---

## ⚙️ 3. Configuration des interfaces

### 🖥️ 3.1 Création des devices
1. 🔗 **Se connecter** à l'interface web d'**OpenWRT**.
2. 🔄 **Accéder à** `Network > Interfaces > Devices`.
3. ➕ **Ajouter les devices** Cliquez sur `Add device configuration` et pour chaques vlans, mettez ces informations :
>   
|📜 **Vlan_NAME** | **Vlan_ID** | 📛 **Device Type** | 🌍 **Base-device** |
|-------------|---------------|-------------------------|-----------------------------|
| vlan_admin       | 2  | Vlan 802.1q     | Br_lan      |
| vlan_user        | 3  | Vlan 802.1q     | Br_lan      |
| vlan_svc         | 4  | Vlan 802.1q     | Br_lan      |
| vlan_wifi_emp    | 5  | Vlan 802.1q     | Phy_ap0     |
| vlan_wifi_guest  | 6  | Vlan 802.1q     | Phy_ap0     |

4. 💾 **Sauvegarder et appliquer** les modifications.

### 🏷️ 3.2 Création des interfaces
1. 🔄 **Accéder à** `Network > Interfaces > Interfaces`.
2. ➕ **Ajouter les interfaces** Cliquez sur `Add new interface` et pour chaques interfaces, mettez ces informations :
>   
|📜 **Vlan_NAME** | **Protocole** | 📛 **Device** | 🌍 **IPv4 Address** | 🌍 **IPv4 Mask** | 🌍 **IPv4 Gateway** |
|-------------|---------------|------------------------|-----------------------------|-----|-----|
| vlan_admin       | Static  | vlan_admin     | 192.168.2.1     | 255.255.255.0 | 192.168.0.240 |
| vlan_user        | Static  | vlan_user     | 192.168.3.1     | 255.255.255.0 | 192.168.0.240 |
| vlan_svc         | Static  | vlan_svc     | 192.168.4.1     | 255.255.255.0 | 192.168.0.240 |
| vlan_wifi_emp    | Static  | vlan_wifi_emp     | 192.168.5.1     | 255.255.255.0 | 192.168.0.240 |
| vlan_wifi_guest  | Static  | vlan_wifi_guest     | 192.168.6.1     | 255.255.255.0 | 192.168.0.240 |

### 🏷️ 3.3 Configuration DHCP
1. 🔄 **Accéder à** `Network > Interfaces > Interfaces`.
2. ➕ **Ajouter le serveur DHCP** Cliquez sur `Edit interface` et pour chaques interfaces, aller dans l'onglet `DHCP`, activer le et mettez ces informations :
>
|📜 **Interface** | **Start** | 📛 **Limit** | 🌍 **Lease time** | 🌍 **DHCP option** |
|------------------|----|------|---------|---------------|
| vlan_admin       | 2  | 100  | 12h     | 6,192.168.4.3 |
| vlan_user        | 2  | 100  | 12h     | 6,192.168.4.3 |
| vlan_svc         | 2  | 100  | 12h     | 6,192.168.4.3 |
| vlan_wifi_emp    | 2  | 100  | 12h     | 6,192.168.4.3 |
| vlan_wifi_guest  | 2  | 100  | 12h     | 6,192.168.4.3 |

###### *note : start,limit et lease time à déterminer, pas définitif pr l'instant*
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
