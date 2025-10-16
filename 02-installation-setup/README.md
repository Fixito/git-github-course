# Module 2 : Installation et Configuration de Git

## Objectifs d'apprentissage

À la fin de ce module, vous serez capable de :

- Installer Git sur votre ordinateur (Windows, Mac, Linux)
- Configurer Git avec vos informations personnelles
- Installer et configurer un éditeur de code
- Vérifier que tout fonctionne correctement

---

## 1. Installation de Git

### 🪟 Installation sur Windows

#### Étape 1 : Téléchargement

1. Rendez-vous sur [https://git-scm.com/downloads](https://git-scm.com/downloads)
2. Cliquez sur **"Download for Windows"**
3. Le téléchargement démarre automatiquement

#### Étape 2 : Installation

1. **Lancez l'installateur** téléchargé
2. Cliquez sur **"Next"** pour les premières étapes
3. **Important :** À l'étape "Choosing the default editor", sélectionnez **"Use Visual Studio Code as Git's default editor"** (si vous utilisez VS Code)
4. **Important :** À l'étape "Adjusting your PATH environment", choisissez **"Git from the command line and also from 3rd-party software"**
5. Pour les autres options, laissez les paramètres par défaut
6. Cliquez sur **"Install"** et attendez la fin de l'installation
7. Cliquez sur **"Finish"**

#### Étape 3 : Vérification

1. Ouvrez **PowerShell** ou **l'invite de commande** (CMD)
2. Tapez la commande suivante :

```powershell
git --version
```

3. Vous devriez voir quelque chose comme : `git version 2.42.0`

✅ **Si vous voyez une version, Git est correctement installé !**

---

### 🍎 Installation sur Mac

#### Méthode 1 : Avec Homebrew (recommandé)

1. Ouvrez **Terminal**
2. Installez Homebrew si ce n'est pas déjà fait :

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

3. Installez Git :

```bash
brew install git
```

4. Vérifiez l'installation :

```bash
git --version
```

#### Méthode 2 : Avec l'installateur officiel

1. Rendez-vous sur [https://git-scm.com/downloads](https://git-scm.com/downloads)
2. Téléchargez la version pour Mac
3. Suivez les instructions d'installation
4. Vérifiez avec `git --version` dans le Terminal

---

### 🐧 Installation sur Linux (Ubuntu/Debian)

1. Ouvrez le **Terminal**
2. Mettez à jour les paquets :

```bash
sudo apt update
```

3. Installez Git :

```bash
sudo apt install git
```

4. Vérifiez l'installation :

```bash
git --version
```

---

## 2. Configuration de Git

Maintenant que Git est installé, nous devons le configurer avec vos informations personnelles. Ces informations apparaîtront dans chaque commit que vous ferez.

### Configuration du nom et de l'email

Ouvrez votre terminal (PowerShell, CMD, Terminal) et tapez :

```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"
```

⚠️ **Important :** Utilisez la même adresse email que celle de votre compte GitHub (si vous en avez un).

### Configuration de la branche par défaut

Depuis quelques années, la branche par défaut s'appelle `main` (et non plus `master`).

```bash
git config --global init.defaultBranch main
```

### Configuration de l'éditeur de texte

Si vous utilisez **Visual Studio Code**, configurez-le comme éditeur par défaut :

```bash
git config --global core.editor "code --wait"
```

### Amélioration de l'affichage (optionnel mais recommandé)

**Activer la couleur dans le terminal :**

```bash
git config --global color.ui auto
```

**Configurer les fins de ligne (important sur Windows) :**

```bash
git config --global core.autocrlf true
```

Sur Mac/Linux :

```bash
git config --global core.autocrlf input
```

---

## 4. Vérification de la configuration

Pour vérifier toute votre configuration Git, tapez :

```bash
git config --list
```

Vous devriez voir vos informations :

```
user.name=Marie Dupont
user.email=marie.dupont@gmail.com
init.defaultbranch=main
core.editor=code --wait
color.ui=auto
...
```

Pour voir une configuration spécifique :

```bash
git config user.name
git config user.email
```

---

## 5. Éditeur de code

Pour modifier vos fichiers de code, vous avez besoin d'un bon éditeur. Nous recommandons **Visual Studio Code**.

### Extensions VS Code utiles pour Git

Une fois VS Code installé, installez cette extension :

1. **GitLens** : Visualisation avancée de Git dans VS Code

Pour installer une extension :

- Cliquez sur l'icône Extensions (carré) dans la barre latérale
- Recherchez "GitLens"
- Cliquez sur "Install"

---

## 3. Créer un projet de test

Vérifions que tout fonctionne en créant un petit projet de test.

### Étape 1 : Ouvrir le terminal

**Sur Windows :**

- Appuyez sur `Windows + x` et appuyez sur `i`
- Ou recherchez "PowerShell" dans le menu Démarrer

**Sur Mac :**

- Appuyez sur `Cmd + Espace` et tapez "Terminal"

**Sur Linux :**

- Appuyez sur `Ctrl + Alt + T`

### Étape 2 : Aller sur le Bureau

**Sur Windows (PowerShell) :**

```powershell
cd Desktop
```

**Sur Mac/Linux :**

```bash
cd Desktop
```

💡 **Astuce :** Le bureau est un endroit pratique pour créer vos projets de test car vous pouvez facilement les voir et y accéder.

### Étape 3 : Créer le projet

Créez un dossier pour votre projet de test :

**Sur Windows (PowerShell) :**

```powershell
mkdir mon-premier-projet
cd mon-premier-projet
```

**Sur Mac/Linux :**

```bash
mkdir mon-premier-projet
cd mon-premier-projet
```

### Étape 4 : Créer un fichier

Créez un fichier `index.html` :

```powershell
code index.html
```

Ajoutez ce contenu :

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mon Premier Projet</title>
  </head>
  <body>
    <h1>Bienvenue !</h1>
    <p>Mon environnement Git fonctionne parfaitement ! 🎉</p>
  </body>
</html>
```

### Étape 5 : Initialiser Git

Dans le terminal, dans le dossier `mon-premier-projet` :

```bash
git init
```

Vous devriez voir : `Initialized empty Git repository in ...`

### Étape 6 : Vérifier le statut

```bash
git status
```

Vous devriez voir votre fichier `index.html` comme "untracked file".

✅ **Parfait ! Tout fonctionne !**

---

## 4. Récapitulatif

Vous avez maintenant installé et configuré :

- ✅ **Git** : pour versionner votre code
- ✅ **VS Code** (recommandé) : pour éditer votre code
- ✅ **Configuration Git** : avec votre nom et email
- ✅ **Projet de test** : pour vérifier que tout fonctionne

**Note :** Le serveur web local (Wamp/Xampp) a déjà été installé précédemment.

---

## 5. Checklist finale

Vérifiez que vous pouvez exécuter ces commandes sans erreur :

```bash
# Vérifier la version de Git
git --version

# Vérifier votre nom
git config user.name

# Vérifier votre email
git config user.email
```

✅ **Tout fonctionne ?** Vous êtes prêt pour le Module 3 !

---

## 6. Problèmes courants et solutions

### Problème 1 : "git n'est pas reconnu en tant que commande"

**Solution :** Git n'est pas dans le PATH

- Fermez et rouvrez votre terminal
- Si ça ne fonctionne toujours pas, réinstallez Git en cochant l'option "Add to PATH"

### Problème 2 : Permission denied sur Mac/Linux

**Solution :**

- Utilisez `sudo` devant vos commandes d'installation
- Exemple : `sudo apt install git`

---

## 7. Ressources complémentaires

- [Documentation officielle Git](https://git-scm.com/doc)

---

## ➡️ Prochaine étape

Dans le **Module 3**, nous allons apprendre à utiliser Git au quotidien :

- Créer votre premier dépôt Git
- Faire vos premiers commits
- Travailler avec GitHub

**[⬅️ Module précédent](../01-why-versioning/README.md)** | **[➡️ Aller au Module 3 : Git Basics](../03-git-basics/README.md)**
