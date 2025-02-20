# 📘 Création des Utilisateurs et Groupes dans Active Directory (Windows Server 2019) - Projet **ECORP** 🚀

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

uwu

## ✅ 1. Prérequis
- 🖥️ Un serveur **Windows Server 2019** configuré en tant que **Contrôleur de Domaine**.
- 🛠️ L'outil **Active Directory Users and Computers** (**ADUC**) installé.
- 🔑 Un compte **administrateur AD** pour effectuer les modifications.

---

## 🏗️ 2. Création des Groupes
### 🎯 Accéder à Active Directory Users and Computers
1. Ouvrir **"Active Directory Users and Computers"** (`dsa.msc`).
2. Naviguer jusqu'à l'OU (**Organizational Unit**) où vous souhaitez créer les groupes.
3. 🖱️ **Clic droit** sur l'OU > **New** > **Group**.
4. Renseigner les informations :
   - **📌 Nom du groupe** : Exemple `RH`, `Dev`, `SSI`
   - **📌 Scope du groupe** : Global
   - **📌 Type du groupe** : Security
5. ✅ Cliquer sur **OK**.

### 📂 Groupes à Créer
- 👥 **RH** (Ressources Humaines)
- 💻 **Dev** (Développement)
- 🔒 **SSI** (Sécurité des Systèmes d'Information)