# 🖥️ Contrôle des Ventilateurs via IPMI sur Dell R720

---

## 📖 Sommaire
1. ✅ [Prérequis](#1-prerequis)
2. 🔗 [Test de Connexion IPMI](#2-test-de-connexion-ipmi)
3. ⚙️ [Contrôle Manuel des Ventilateurs](#3-controle-manuel-des-ventilateurs)
4. 🚨 [Bonnes Pratiques et Surveillance](#4-bonnes-pratiques-et-surveillance)
5. 🛠️ [Dépannage](#5-depannage)

---

## ✅ 1. Prérequis

### 📦 1.1 Installation d'IPMI Tool
```bash
# Sur Debian/Ubuntu
apt-get install ipmitool

# Sur CentOS/RHEL
yum install ipmitool
```

### 🔑 1.2 Informations Nécessaires
- 🌐 **Adresse IP de l'iDRAC** : `192.168.4.22`
- 👤 **Nom d'utilisateur iDRAC**
- 🔒 **Mot de passe iDRAC**

---

## 🔗 2. Test de Connexion IPMI

Avant toute modification, vérifiez que la connexion IPMI fonctionne :
```bash
ipmitool -I lanplus -H 192.168.4.22 -U [USERNAME] -P [PASSWORD] chassis status
```

Si la connexion échoue, vérifiez :
- La connectivité réseau vers l'iDRAC
- Les identifiants iDRAC
- Les paramètres IPMI activés dans le BIOS

---

## ⚙️ 3. Contrôle Manuel des Ventilateurs

### 🔄 3.1 Activation/Désactivation du Mode Manuel

✅ **Activer le mode manuel** :
```bash
ipmitool -I lanplus -H 192.168.4.22 -U [USERNAME] -P [PASSWORD] raw 0x30 0x30 0x01 0x00
```

🚀 **Revenir en mode automatique** (recommandé après modification) :
```bash
ipmitool -I lanplus -H 192.168.4.22 -U [USERNAME] -P [PASSWORD] raw 0x30 0x30 0x01 0x01
```

### 🌬️ 3.2 Réglage de la Vitesse des Ventilateurs

| 🔧 **Vitesse (%)** | 🖥️ **Commande** |
|-----------------|--------------------------------------------------|
| 0% ❌ (Dangereux) | `raw 0x30 0x30 0x02 0xff 0x00` |
| 5% | `raw 0x30 0x30 0x02 0xff 0x05` |
| 10% | `raw 0x30 0x30 0x02 0xff 0x0A` |
| 20% | `raw 0x30 0x30 0x02 0xff 0x14` |
| 30% | `raw 0x30 0x30 0x02 0xff 0x1e` |
| 50% | `raw 0x30 0x30 0x02 0xff 0x32` |
| 70% | `raw 0x30 0x30 0x02 0xff 0x46` |
| 80% | `raw 0x30 0x30 0x02 0xff 0x50` |

---

## 🚨 4. Bonnes Pratiques et Surveillance

### ⚠️ 4.1 Précautions d'Usage
- ❌ **Ne jamais laisser les ventilateurs à 0%** ⚠️
- 🔍 **Surveiller attentivement les températures après modification**
- 📝 **Garder un script pour revenir en mode automatique en cas de problème**
- 📌 **Documenter les changements de configuration**

### 🌡️ 4.2 Surveillance des Températures
Pour vérifier la température du serveur :
```bash
ipmitool -I lanplus -H 192.168.4.22 -U [USERNAME] -P [PASSWORD] sdr type temperature
```

---

## 🛠️ 5. Dépannage

| 🔴 **Problème** | 🛠️ **Solution** |
|-----------------|----------------------------------------------|
| ❌ Erreur de connexion | Vérifiez les identifiants et l'adresse IP de l'iDRAC |
| 🔒 Commande refusée | Vérifiez les privilèges du compte IPMI |
| 🌐 Pas de réponse | Vérifiez la connectivité réseau et l'état de l'iDRAC |

### 📞 Contact Support
- 📧 **Support Système** : [à compléter]
- 📖 **Documentation IPMI Officielle** : [à compléter]

---

## 🎯 Conclusion
Cette documentation détaille le **contrôle manuel des ventilateurs** via **IPMI sur Dell R720** 🏢. Toujours s'assurer de **surveiller la température** après modification pour éviter toute surchauffe. 🚀

