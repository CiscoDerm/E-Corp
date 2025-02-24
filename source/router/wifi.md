# 🖧 Documentation de l'Infrastructure Réseau - Configuration du WIFI avec Radius

---

## 📖 Sommaire
1. 🎯 [Introduction](#1-introduction)
2. 📌 [Installation des paquets](#2-installation-des-paquets)
3. ⚙️ [Configuration du wifi](#3-configuration-du-wifi)
4. ✅ [Tests et Validation](#4-tests-et-validation)
5. 🎯 [Conclusion](#5-conclusion)

---

## 🎯 1. Introduction
L'objectif de cette documentation est de détailler la **configuration du Wifi** sur un **routeur Linksys sous OpenWRT**. L'authentification se fera avec **RADIUS** pour le **wifi employé**, et par **wpa-psk** pour le **wifi-guest**

Le wifi guest ne doit avoir accès uniquement à Internet

Le wifi employé peut avoir accès à internet + gitlab. Les utilisateur wifi ne peuvent pas administrer.


---

## 📌 2. Installation des paquets

**WPAD** sert à faire **access point**, et est compatible avec l'authentification **RADIUS**.

- Accédez à `system > Software` 
- Tapez **wpad-openssl** dans la barre de recherche
- Cliquez sur update lists
- cliquez sur **download** en face de **wpad-openssl**

---

## ⚙️ 3. Configuration du wifi

1. Connectez vous en ssh : `ssh root@192.168.2.1`
2. Modifiez le fichier de conf wifi : `vim /etc/config/wireless`
3. faite `:%d` , entrée ,pour tout supprimer
3. Mettez cette conf :
```sh
config wifi-device 'radio0'
	option type 'mac80211'
	option path 'soc/soc:pcie/pci0000:00/0000:00:01.0/0000:01:00.0'
	option channel '36'
	option band '5g'
	option country 'FR'
	option cell_density '0'
	option disabled '0'

config wifi-iface 'default_radio0'
	option device 'radio0'
	option mode 'ap'
	option ssid 'E-corp'
	option encryption 'wpa2'
	option macaddr '24:f5:a2:c0:e6:8a'
	option network 'vlan_wifi_emp'
	option auth_server '192.168.4.3'
	option auth_port '1812'
	option auth_secret '123'
	option acct_server '192.168.4.3'
	option acct_port '1813'
	option acct_secret '123'
	option ieee8021x '1'
	option nasid 'radiusopenwrt'
    option eap_type 'peap'
    option auth 'EAP-MSCHAPV2'

config wifi-device 'radio1'
	option type 'mac80211'
	option path 'soc/soc:pcie/pci0000:00/0000:00:02.0/0000:02:00.0'
	option channel '1'
	option band '2g'
	option htmode 'HT20'
	option disabled '1'
	option country 'FR'

config wifi-iface 'default_radio1'
	option device 'radio1'
	option network 'lan'
	option mode 'ap'
	option ssid 'OpenWrt'
	option encryption 'none'
	option macaddr '24:f5:a2:c0:e6:89'

config wifi-device 'radio2'
	option type 'mac80211'
	option path 'platform/soc/soc:internal-regs/f10d8000.sdhci/mmc_host/mmc0/mmc0:0001/mmc0:0001:1'
	option channel '34'
	option band '5g'
	option htmode 'VHT80'
	option disabled '1'

config wifi-iface 'default_radio2'
	option device 'radio2'
	option network 'lan'
	option mode 'ap'
	option ssid 'OpenWrt'
	option encryption 'none'

config wifi-iface 'wifinet3'
	option device 'radio0'
	option mode 'ap'
	option ssid 'E-corp_Guest'
	option encryption 'psk2'
	option key 'Ecorp123'
	option network 'vlan_wifi_guest'
```

---

## ✅ 4. Tests et Validation

1. 🔎 **Vérification des dépendance** : Pour fonctionner, il faut avoir configurer le serveur **NPS**
2. 🔄 **Connexion à wifi guest** : Vérifier que l'authentification basique marche, et que l'IP est bien en 192.168.6.x
3. 🚨 **Connexion à wifi Employé** : Vérifier que l'authentification avec les identifiants LDAP marche, et que l'IP' est bien en 192.168.5.x
4. 📊 **Vérification des droits** : Wifi Guest ne doit avoir accès que à internet, et wifi emp à internet + gitlab uniquement.

---

## 🎯 5. Conclusion
Cette documentation fournit une **vue d'ensemble complète** de la configuration du WIFI sur **OpenWRT (routeur Linksys)**. Assurez-vous de **tester et sécuriser** votre infrastructure pour garantir un réseau performant et protégé. 🚀
