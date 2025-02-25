# Création des Utilisateurs et Groupes dans Active Directory (Windows Server 2019) - Projet **ECORP** 🚀

## 📖 Sommaire
1. ✅ [Prérequis](#1-prerequis)
2. 🏗️ [Création des Groupes](#2-creation-des-groupes)
   - 🎯 [Accéder à Active Directory Users and Computers](#acceder-a-active-directory-users-and-computers)
   - 📂 [Groupes à Créer](#groupes-a-creer)
3. 🆕 [Création des Utilisateurs](#3-creation-des-utilisateurs)
   - 📜 [Utilisateurs à Créer](#utilisateurs-a-creer)
4. 🔗 [Ajout des Utilisateurs aux Groupes](#4-ajout-des-utilisateurs-aux-groupes)
5. 🔍 [Vérification des Utilisateurs et Groupes](#5-verification-des-utilisateurs-et-groupes)

---

## ✅ 1. Prérequis
- Un serveur **Windows Server 2019** configuré en tant que **Contrôleur de Domaine**.
- L'outil **Active Directory Users and Computers** (**ADUC**) installé.
- Un compte **administrateur AD** pour effectuer les modifications.

---

## 🏗️ 2. Création des Groupes
### 🎯 Accéder à Active Directory Users and Computers
1. Ouvrir **"Active Directory Users and Computers"** (`dsa.msc`).
2. Naviguer jusqu'à l'OU (**Organizational Unit**) où vous souhaitez créer les groupes.
3. **Clic droit** sur l'OU > **New** > **Group**.
4. Renseigner les informations :
   - **Nom du groupe** : Exemple `RH`, `Dev`, `SSI`
   - **Scope du groupe** : Global
   - **Type du groupe** : Security
5. Cliquer sur **OK**.

### 📂 Groupes à Créer
- 👥 **RH** (Ressources Humaines)
- 💻 **Dev** (Développement)
- 🔒 **SSI** (Sécurité des Systèmes d'Information)

---

## 🆕 3. Création des Utilisateurs
1. Dans **Active Directory Users and Computers**, naviguer vers l'OU cible.
2. **Clic droit** sur l'OU > **New** > **User**.
3. Renseigner les champs :
   - **Prénom et Nom**
   - **Nom d'utilisateur** (sAMAccountName)
4. Cliquer sur **Next**.
5. Renseigner le **mot de passe** et cocher :
   - **User cannot change password**
   - **Password never expires**
6. Cliquer sur **Finish**.

### 📜 Utilisateurs à Créer
| 👤 Nom Complet | 🆔 Nom d'utilisateur | 📌 Groupe |
|--------------|----------------|--------|
| Rose Rouge  | r.rouge        | RH (Responsable) |
| Rachel Rime | r.rime         | RH |
| Maxim Dufossey | m.dufosse   | RH |
| David Drean | d.drean        | Dev (Responsable) |
| Dwayne Dig  | d.dig          | Dev |
| Tristan Arnaud | t.arnaud    | Dev |
| Sacha Souf  | s.souf         | SSI (Responsable) |
| Sandrine Sodo | s.sodo       | SSI |
| Charles Senamaud | c.senamaud| SSI |
| Léo Hery | l.hery            | SSI |

---

## 🔗 4. Ajout des Utilisateurs aux Groupes
1. Dans **Active Directory Users and Computers**, ouvrir l'utilisateur.
2. Aller dans l'onglet **Member Of**.
3. **Cliquer sur Add** et sélectionner le groupe correspondant.
4. Valider avec **OK**.

---

## 🔍 5. Vérification des Utilisateurs et Groupes
- Utiliser la commande **PowerShell** suivante pour lister les utilisateurs d'un groupe :

```powershell
Get-ADGroupMember -Identity "RH"
Get-ADGroupMember -Identity "Dev"
Get-ADGroupMember -Identity "SSI"
```

- Pour vérifier qu'un utilisateur existe :

```powershell
Get-ADUser -Identity "r.rouge"
```


