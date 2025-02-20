# Documentation de l'Infrastructure Réseau - VLANs

## Introduction
L'objectif de cette documentation est de détailler la configuration des VLANs sur notre infrastructure réseau basée sur un routeur Linksys sous OpenWRT et un switch Netgear manageable de 24 ports (IP : 192.168.0.115). Les VLANs permettent de segmenter le réseau en différentes zones sécurisées et d'optimiser la gestion du trafic.

## Table des VLANs

| VLAN | Nom          | Adresse Réseau       | Description |
|------|-------------|----------------------|-------------|
| 10   | Admin       | 192.168.2.x/24       | VLAN réservé aux administrateurs réseau |
| 20   | Utilisateurs| 192.168.3.x/24       | VLAN destiné aux utilisateurs standards |
| 30   | Services    | 192.168.4.x/24       | VLAN pour tous les services internes |
| 40   | WiFi Users  | À définir            | VLAN pour les utilisateurs WiFi |
| 50   | WiFi Admin  | À définir            | VLAN pour l'administration WiFi |

## Configuration des VLANs
### 1. Configuration des VLANs via OpenWRT (Routeur Linksys)

1. Connectez-vous à l'interface web de **OpenWRT**.
2. Accédez à **Network > Switch**.
3. Ajoutez les VLANs en respectant les informations ci-dessus.
4. Affectez les ports selon la configuration souhaitée.
5. Sauvegardez et appliquez les modifications.

### 2. Configuration du switch Netgear (IP : 192.168.0.115)

#### Attribution physique des ports :
- **VLAN ADMIN** : Ports en bas à gauche.
- **VLAN USERS** : Ports en bas à droite.
- **VLAN SERVICES** : Ports en haut à droite.

#### Étapes de configuration via l'interface web Netgear :
1. Connectez-vous à l'interface web du switch à l'adresse **192.168.0.115**.
2. Allez dans **Switching > VLAN > VLAN Configuration**.
3. Ajoutez les VLANs 10, 20, 30, 40 et 50.
4. Assignez les ports selon le tableau suivant :

| VLAN | Ports assignés |
|------|---------------|
| 10 (Admin) | Bas gauche |
| 20 (Users) | Bas droite |
| 30 (Services) | Haut droite |
| 40 (WiFi Users) | À définir |
| 50 (WiFi Admin) | À définir |

5. Configurez les ports en mode **Access** ou **Trunk** selon les besoins.
6. Appliquez et testez la configuration.

### 3. Attribution des adresses IP et du DHCP
- Activez un serveur DHCP sur chaque VLAN si nécessaire.
- Configurez les plages d'adresses pour éviter les conflits IP.
- Assurez-vous que les adresses IP sont bien attribuées automatiquement selon les VLANs définis.

Exemple de configuration DHCP sur OpenWRT :
```shell
config dhcp 'vlan10'
    option interface 'vlan10'
    option start '100'
    option limit '50'
    option leasetime '12h'
```

### 4. Sécurisation des VLANs
- Restreindre l'accès au VLAN Admin (10) en appliquant des règles ACL.
- Isoler les VLANs utilisateurs et services pour éviter le trafic inter-VLAN non autorisé.
- Activer **802.1X** pour sécuriser les accès aux VLANs sensibles.
- Configurer un pare-feu pour filtrer les accès entre VLANs selon les besoins.

Exemple de règle ACL pour bloquer l'accès au VLAN Admin depuis le VLAN Utilisateurs :
```shell
access-list 100 deny ip 192.168.3.0 0.0.0.255 192.168.2.0 0.0.0.255
access-list 100 permit ip any any
```

## Tests et Validation
1. **Vérifier l'accès réseau** : Chaque VLAN doit pouvoir communiquer avec les ressources qui lui sont attribuées.
2. **Vérifier le DHCP** : Tester l'attribution correcte des IPs par VLAN.
3. **Tester les ACLs** : Vérifier que les restrictions d'accès sont bien appliquées.
4. **Monitorer le trafic** : Utiliser des outils comme `tcpdump` ou `Wireshark` pour analyser le trafic réseau.

## Conclusion
Cette configuration permet une segmentation efficace du réseau, améliore la sécurité et optimise l'utilisation des ressources. La mise en place des VLANs doit être suivie de tests réguliers pour garantir leur bon fonctionnement et éviter toute faille de sécurité.
