# 🔐 Création et Application des GPO dans Active Directory (Windows Server 2019) - Projet **ECORP** 🚀

## ✅ 1. Prérequis
- 🖥️ Un serveur **Windows Server 2019** configuré en tant que **Contrôleur de Domaine**.
- ⚙️ L'outil **Group Policy Management Console (GPMC)** installé.
- 🔑 Un compte **administrateur AD** pour la gestion des GPO.

---

## 📜 2. Création des GPO
### 🎯 Accéder à la Gestion des Stratégies de Groupe
1. Ouvrir **"Group Policy Management"** (`gpmc.msc`).
2. Naviguer jusqu'à l'OU (**Organizational Unit**) cible.
3. 🖱️ **Clic droit** sur l'OU > **Create a GPO in this domain, and link it here**.
4. Renseigner le **nom de la GPO**.
5. 🛠️ **Modifier la GPO** via **Group Policy Management Editor**.

### 📂 GPO à Créer et Appliquer
| 🏷️ GPO | 📌 Groupes Ciblés |
|-------------|----------------|
| 🔑 **Politique de mot de passe** | Tous les groupes |
| 🔐 **Verrouillage du compte** (tentatives multiples) | Tous les groupes |
| 🚫 **Restriction installation logiciels** | Tous sauf **SSI** |
| 🔄 **Windows Update obligatoire** | Tous les groupes |
| 📦 **Déploiement de logiciel** | **SSI** (déploiement vers tous) |
| 📁 **Mappage d'un lecteur réseau** (partage fichiers) | Tous les groupes |
| 🚫 **Blocage accès registre** | Tous sauf **SSI** |
| ⚙️ **Interdiction accès Panneau de configuration** | Tous sauf **SSI** |
| 🔌 **Restriction stockage amovible** | Tous sauf **SSI** |
| 💤 **Mise en veille après 3 min d'inactivité** | Tous les groupes |
| 🎨 **Fond d'écran différent par groupe** | Appliqué à chaque groupe |
| 🔒 **Interdiction Invite de commandes et PowerShell** | Tous sauf **SSI** |

---

## ⚙️ 3. Configuration des GPO Principales au sein de ECORP

### 🔑 **Politique de mot de passe**
📍 **Chemin GPO** : `Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy`
- **Longueur minimale** : 12 caractères
- **Complexité requise** : ✅ Oui
- **Expiration** : 90 jours

### 🔐 **Verrouillage du compte**
📍 **Chemin GPO** : `Computer Configuration > Policies > Windows Settings > Security Settings > Account Lockout Policy`
- **Tentatives échouées avant verrouillage** : 5
- **Durée du verrouillage** : 30 minutes

### 🚫 **Restriction installation logiciels**
📍 **Chemin GPO** : `Computer Configuration > Policies > Administrative Templates > Windows Components > Windows Installer`
- **Désactiver Windows Installer** : ✅ Activé (tous sauf **SSI**)

### 🔄 **Windows Update obligatoire**
📍 **Chemin GPO** : `Computer Configuration > Policies > Administrative Templates > Windows Components > Windows Update`
- **Configuration automatique des mises à jour** : ✅ Activé

### 📦 **Déploiement de logiciel**
📍 **Chemin GPO** : `Computer Configuration > Policies > Software Settings > Software Installation`
- **Ajout de packages MSI** : ✅ Appliqué uniquement au groupe **SSI**

### 📁 **Mappage d'un lecteur réseau**
📍 **Chemin GPO** : `User Configuration > Preferences > Windows Settings > Drive Maps`
- **Ajout d'un lecteur réseau partagé** : ✅ Tous les groupes

### 🚫 **Blocage accès au registre**
📍 **Chemin GPO** : `User Configuration > Policies > Administrative Templates > System`
- **Empêcher accès Regedit** : ✅ Tous sauf **SSI**

### ⚙️ **Interdiction accès Panneau de configuration**
📍 **Chemin GPO** : `User Configuration > Policies > Administrative Templates > Control Panel`
- **Désactiver l'accès au Panneau de configuration** : ✅ Tous sauf **SSI**

### 🔌 **Restriction stockage amovible**
📍 **Chemin GPO** : `Computer Configuration > Policies > Administrative Templates > System > Removable Storage Access`
- **Désactiver accès périphériques USB** : ✅ Tous sauf **SSI**

### 💤 **Mise en veille après 3 minutes d'inactivité**
📍 **Chemin GPO** : `Computer Configuration > Policies > Administrative Templates > System > Power Management`
- **Mise en veille après inactivité** : ✅ 3 minutes

### 🎨 **Fond d'écran par groupe**
📍 **Chemin GPO** : `User Configuration > Policies > Administrative Templates > Desktop > Desktop Wallpaper`
- **Définir un fond d'écran spécifique par groupe** : ✅ Appliqué individuellement

### 🔒 **Interdiction Invite de commandes et PowerShell**
📍 **Chemin GPO** : `User Configuration > Policies > Administrative Templates > System`
- **Désactiver l'accès à cmd.exe et PowerShell** : ✅ Tous sauf **SSI**

---

## 🔍 5. Vérification des GPO Appliquées
- 📌 Pour voir les GPO appliquées à un utilisateur :

```powershell
gpresult /r
```

- 📌 Pour lister toutes les GPO appliquées dans le domaine :

```powershell
Get-GPO -All
```

---

## 🎯 6. Conclusion
Cette documentation détaille la **création et l'application des GPO** pour **ECORP** 🏢. 
