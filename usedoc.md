# 📘 Utiliser et Contribuer à la Documentation

---

## 📖 Sommaire
1. 🎯 [Introduction](#-1-introduction)
2. ✅ [Prérequis](#-2-prérequis)
   - 🛠️ [Installation de Git](#-21-installation-de-git)
   - 🐍 [Installation de Python et pip](#-22-installation-de-python-et-pip)
3. ⚙️ [Installation de MkDocs](#-3-installation-de-mkdocs)
4. 🔄 [Utilisation de Git](#-4-utilisation-de-git)
5. 🚀 [Test rapide de MkDocs](#-5-test-rapide-de-mkdocs)
6. 🎯 [Conclusion](#-6-conclusion)

---

## 🎯 1. Introduction
**MkDocs** est un générateur de documentation statique simple et puissant écrit en **Python**. Cette documentation vous guidera à travers l'installation de **MkDocs** sur **Linux** et **Windows**, ainsi que son utilisation et contribution via **Git**. 🚀

---

## ✅ 2. Prérequis
Avant d'installer **MkDocs**, assurez-vous d'avoir installé **Git**, **Python** et **pip**.

### 🛠️ 2.1 Installation de Git

#### 🔹 Sous Linux
```sh
sudo apt update && sudo apt upgrade -y
sudo apt install git -y
git --version
```

#### 🔹 Sous Windows
1. Téléchargez et installez [Git](https://git-scm.com/downloads)
2. Vérifiez l'installation :
   ```sh
   git --version
   ```

### 🔧 2.2 Configurer Git
```sh
git config --global user.email "email.used@for.github"
git config --global user.name "githubuser"
```

### 🐍 2.3 Vérification et Installation de Python et pip

#### 🔹 Vérification de Python
```sh
python --version
python3 --version
```

#### 🔹 Installation de Python et pip
##### Sous Linux
```sh
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip -y
python3 --version
pip3 --version
```

##### Sous Windows
1. Téléchargez et installez [Python](https://www.python.org/downloads/)
2. Cochez **"Add Python to PATH"** lors de l'installation
3. Vérifiez l'installation :
   ```sh
   python --version
   pip --version
   ```
4. Mettez à jour pip :
   ```sh
   python -m pip install --upgrade pip
   ```

---

## ⚙️ 3. Installation de MkDocs

### 🔹 Sous Linux
```sh
pip3 install mkdocs
mkdocs --version
```

### 🔹 Sous Windows
```sh
pip install mkdocs
mkdocs --version
```

---

## 🔄 4. Utilisation de Git

### 🖥️ Cloner le projet
```sh
git clone https://github.com/CiscoDerm/E-Corp.git
```

### ✏️ Contribuer à la documentation
1. **Créer une nouvelle branche** :
   ```sh
   git checkout -b feat/ma-branche
   ```
2. **Ajouter et commit ses modifications** :
   ```sh
   git add .
   git commit -m "feat: Ajout de l'ad"
   ```
3. **Pousser les modifications** :
   ```sh
   git push origin feat/ma-branche
   ```
4. **Créer une merge request (Pull Request)** sur GitHub/GitLab.

---

## 🚀 5. Test rapide de MkDocs

1. **Aller dans le dossier du projet** :
   ```sh
   cd E-corp
   ```
2. **Lancer le serveur local** :
   ```sh
   mkdocs serve
   ```
3. **Ouvrir dans un navigateur** :
   [http://127.0.0.1:8000](http://localhost:8000)

---

## 🎯 6. Conclusion
Vous avez maintenant **MkDocs** installé et prêt à l'emploi sur **Linux** et **Windows** ! Vous savez également comment utiliser **Git** pour contribuer à un projet et gérer vos modifications efficacement. 🚀

