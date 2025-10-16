# Module 3 : Premiers pas avec Git

## Objectifs d'apprentissage

À la fin de ce module, vous serez capable de :

- Créer votre premier dépôt Git
- Comprendre le workflow Git (working directory, staging area, repository)
- Faire vos premiers commits
- Visualiser l'historique de vos commits
- Pousser votre code sur GitHub

---

## Prérequis

Avant de commencer ce module, assurez-vous d'avoir :

- ✅ Installé Git (Module 2)
- ✅ Configuré Git avec votre nom et email (Module 2)
- ✅ Un compte GitHub (nous allons le créer ensemble si nécessaire)

---

---

## 1. Créer un compte GitHub

GitHub est une plateforme qui permet de stocker vos dépôts Git en ligne et de collaborer avec d'autres développeurs.

### Étapes pour créer un compte

1. Rendez-vous sur [https://github.com](https://github.com)
2. Cliquez sur **"Sign up"** (S'inscrire)
3. Entrez votre adresse email (la même que celle configurée dans Git)
4. Créez un mot de passe sécurisé
5. Choisissez un nom d'utilisateur (il sera visible publiquement)
6. Vérifiez votre compte par email
7. Choisissez la version gratuite (Free)

✅ **Vous avez maintenant un compte GitHub !**

---

## 2. Créer un repository

1. Se créer un compte GitHub.

2. Créer un nouveau repository avec le bouton `New Repository`.

3. Appeler le repo `git-test`, cocher `Add a README file` et le créer en cliquant sur le bouton `Create repository`.

Cela nous redirige vers la page du repo sur GitHub.

### Comprendre la structure d'un repository GitHub

Sur la page de votre repository, vous pouvez voir :

- **Code** : Les fichiers de votre projet
- **Issues** : Pour signaler des bugs ou proposer des fonctionnalités
- **Pull requests** : Pour proposer des modifications
- **README.md** : La page d'accueil de votre projet

---

## 3. Cloner le repository sur votre ordinateur

**Cloner** signifie télécharger une copie du repository sur votre ordinateur.

4. Pour cloner le repo, cliquer sur le bouton vert `<> Code`, sélectionner l'option `HTTPS` puis copier le lien en-dessous.

Nous allons maintenant utiliser le terminal.

5. Créer un nouveau dossier `repos` avec la commande `mkdir` :

**Sur Windows (PowerShell) :**

```powershell
cd Desktop
mkdir repos
cd repos
```

**Sur Mac/Linux :**

```bash
cd Desktop
mkdir repos
cd repos
```

6. Cloner le repository avec la commande `git clone` suivie du lien que nous avons copié :

La commande devrait ressembler à :

```sh
git clone https://github.com/VOTRE-NOM-UTILISATEUR/git-test.git
```

Vous verriez quelque chose comme :

```
Cloning into 'git-test'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

✅ **Le repository a été cloné !**

7. Aller dans le dossier `git-test` que nous venons de télécharger :

```bash
cd git-test
```

8. Utiliser la commande `git remote -v` pour afficher l'URL du repository :

```bash
git remote -v
```

Vous verriez :

```
origin  https://github.com/VOTRE-NOM/git-test.git (fetch)
origin  https://github.com/VOTRE-NOM/git-test.git (push)
```

### Comprendre "origin"

On peut remarquer le mot `origin` au début qui est le **nom de la connexion à distance**.

- **origin** = Le nom par défaut donné au repository distant (sur GitHub)
- C'est la convention standard utilisée par tous les développeurs

---

## 4. Le workflow Git : comprendre les 3 zones

---

## 4. Le workflow Git : comprendre les 3 zones

Avant de faire notre premier commit, il est important de comprendre comment Git organise votre travail :

```
┌─────────────────────┐
│  Working Directory  │  ← Vos fichiers sur votre ordinateur
│   (Répertoire de    │
│      travail)       │
└──────────┬──────────┘
           │ git add
           ▼
┌─────────────────────┐
│   Staging Area      │  ← Zone de préparation (fichiers prêts à être commités)
│   (Index)           │
└──────────┬──────────┘
           │ git commit
           ▼
┌─────────────────────┐
│   Local Repository  │  ← Historique Git local
│   (.git)            │
└──────────┬──────────┘
           │ git push
           ▼
┌─────────────────────┐
│  Remote Repository  │  ← GitHub (repository distant)
│   (GitHub)          │
└─────────────────────┘
```

### Les 3 zones expliquées

1. **Working Directory** (Répertoire de travail)

   - C'est votre dossier de projet normal
   - Vous modifiez vos fichiers ici

2. **Staging Area** (Zone de préparation / Index)

   - Les fichiers que vous préparez pour le prochain commit
   - Commande : `git add`

3. **Local Repository** (Dépôt local)

   - L'historique Git stocké dans le dossier `.git`
   - Commande : `git commit`

4. **Remote Repository** (Dépôt distant)
   - Votre repository sur GitHub
   - Commande : `git push`

---

## 5. Utiliser le workflow Git

1. Créer une nouveau fichier dans le dossier `git-test` et l'appeler `hello-world.txt` :

**Sur Windows (PowerShell) :**

```powershell
ni hello-world.txt
```

**Sur Mac/Linux :**

```bash
touch hello-world.txt
```

2. Taper `git status` dans le terminal :

```bash
git status
```

Vous devriez voir :

```
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello-world.txt

nothing added to commit but untracked files present (use "git add" to track)
```

### Comprendre git status

- **Untracked files** (fichiers non suivis) : Git a détecté le nouveau fichier mais ne le suit pas encore
- Le fichier apparaît en **rouge** dans la plupart des terminaux
- Git nous suggère d'utiliser `git add` pour commencer à suivre ce fichier

3. Ajouter le fichier à la staging area avec `git add` :

```bash
git add hello-world.txt
```

### Comprendre git add

Cette commande ajoute le fichier `hello-world.txt` à la **zone de staging** (zone de préparation) de Git.

**Pensez à la staging area comme :**

- Une "salle d'attente" pour vos modifications
- Un "panier" où vous mettez les fichiers avant de les "acheter" (commiter)
- Une zone de préparation où vous choisissez ce qui sera inclus dans le prochain commit

4. Taper à nouveau `git status` :

```bash
git status
```

Vous devriez voir :

```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   hello-world.txt
```

Notre fichier est maintenant affiché en **vert**, ce qui signifie qu'il est dans la zone de staging et prêt à être commité !

5. Créer votre premier commit avec `git commit` :

```bash
git commit -m "Add hello-world.txt"
```

### Comprendre git commit

- **commit** = Créer un "snapshot" (instantané) de vos modifications
- **-m** = Ajouter un message de commit
- **Le message** = Décrire ce que vous avez fait (en anglais par convention)

Vous devriez voir :

```
[main abc1234] Add hello-world.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello-world.txt
```

6. Vérifier le statut avec `git status` :

```bash
git status
```

Résultat :

```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

### Comprendre le message

- **nothing to commit, working tree clean** : Aucune modification en attente, tout est propre ✅
- **Your branch is ahead of 'origin/main' by 1 commit** : Votre branche locale a 1 commit de plus que GitHub

Cela signifie que vous avez fait un commit localement, mais que vous ne l'avez pas encore poussé sur GitHub.

6. Consulter l'historique des commits avec `git log` :

```bash
git log
```

Vous devriez voir :

```
commit abc123def456... (HEAD -> main)
Author: Votre Nom <votre.email@example.com>
Date:   Tue Oct 15 14:30:00 2025 +0200

    Add hello-world.txt

commit xyz789...
Author: GitHub <noreply@github.com>
Date:   Tue Oct 15 14:00:00 2025 +0200

    Initial commit
```

### Comprendre git log

Vous pouvez voir :

- **L'identifiant du commit** (hash) : `abc123def456...`
- **L'auteur** : qui a fait le commit
- **La date** : quand le commit a été fait
- **Le message** : pourquoi cette modification a été faite

⚠️ **Si le terminal est bloqué avec (END) en bas :** Appuyez sur `q` pour quitter.

### Autre option utile de git log

```bash
# Affichage compact (une ligne par commit)
git log --oneline
```

---

## 6. Modifier des fichiers

1. Ouvrir `README.md` dans VS Code :

```bash
code README.md
```

Si VS Code n'est pas dans votre PATH, ouvrez le fichier manuellement avec votre éditeur.

2. Ajouter du contenu dans `README.md` :

```markdown
# Git Test

Bonjour ! Ceci est mon premier projet Git. 🎉

## Ce que j'ai appris

- Créer un repository
- Cloner un repository
- Faire des commits
- Utiliser git status, git add, git commit
```

3. Enregistrer le fichier (`Ctrl + S` ou `Cmd + S`)

4. Vérifier le statut :

```bash
git status
```

Résultat :

```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

On remarque que `README.md` est maintenant affiché comme **modified** (modifié) mais **not staged** (pas dans la staging area).

4. Ajouter `README.md` à la staging area :

```bash
git add README.md
```

5. Vérifier le statut :

```bash
git status
```

`README.md` s'affichera maintenant en **vert** (dans la staging area). Le fichier `hello-world.txt` n'apparaîtra pas car il n'a pas été modifié depuis son dernier commit.

6. Ouvrir `hello-world.txt`, ajouter du texte, puis l'enregistrer :

```
Bonjour le monde !
Ceci est mon premier fichier versionné avec Git.
```

7. Ajouter **tous les fichiers modifiés** à la staging area en une seule commande :

```bash
git add .
```

### Comprendre "git add ."

- Le point `.` signifie "ajouter tous les fichiers modifiés du répertoire courant et de ses sous-répertoires"
- C'est un raccourci pratique pour ajouter plusieurs fichiers en même temps

⚠️ **Attention :** Vérifiez toujours avec `git status` avant de faire `git add .` pour ne pas ajouter des fichiers indésirables !

8. Vérifier que tout est dans la staging area :

```bash
git status
```

Tout devrait maintenant être en vert !

6. Commiter tous les fichiers de la staging area avec un message descriptif :

```bash
git commit -m "Update README and add content to hello-world"
```

### Bonnes pratiques pour les messages de commit

✅ **Bon message :**

- `"Add contact form"`
- `"Fix bug in login validation"`
- `"Update homepage design"`

❌ **Mauvais message :**

- `"update"` (trop vague)
- `"fix"` (qu'est-ce qui est corrigé ?)
- `"asdfgh"` (incompréhensible)

**Règles d'or :**

- Commencer par un verbe à l'impératif (Add, Fix, Update, Remove)
- Être concis mais descriptif
- Écrire en anglais (convention professionnelle)
- Un commit = une modification logique

7. Vérifier le statut :

```bash
git status
```

Résultat : `nothing to commit, working tree clean` ✅

8. Consulter l'historique :

```bash
git log --oneline
```

Vous devriez maintenant voir **trois commits** :

```
def456g Update README and add content to hello-world
abc123d Add hello-world.txt
xyz789e Initial commit
```

---

## 7. Pousser votre travail sur GitHub

Maintenant que nous avons fait plusieurs commits localement, nous allons les **pousser** (push) vers GitHub.

1. Pousser vos commits vers GitHub :

```bash
git push
```

Ou plus précisément :

```bash
git push origin main
```

### Comprendre git push

- **git push** = Envoyer vos commits locaux vers le repository distant (GitHub)
- **origin** = Le nom du repository distant (GitHub)
- **main** = La branche sur laquelle vous poussez

**Que se passe-t-il ?**

```
Local Repository  ────push────>  Remote Repository (GitHub)
(Votre ordinateur)                (GitHub.com)
```

Vous devriez voir :

```
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 620 bytes | 620.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/VOTRE-NOM/git-test.git
   xyz789e..def456g  main -> main
```

✅ **Vos modifications sont maintenant sur GitHub !**

2. Vérifier le statut final :

```bash
git status
```

Résultat :

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

**Traduction :** Votre branche locale est synchronisée avec GitHub. Tout est propre ! ✅

3. Vérifier sur GitHub :

- Ouvrez votre navigateur
- Allez sur `https://github.com/VOTRE-NOM/git-test`
- Actualisez la page

Vous devriez maintenant voir :

- ✅ Votre fichier `hello-world.txt`
- ✅ Votre `README.md` mis à jour
- ✅ Vos commits dans l'historique

---

## 8. Résumé du workflow Git complet

Voici le processus complet que vous venez d'apprendre :

```
1. Modifier des fichiers
   ↓
2. git status (vérifier les modifications)
   ↓
3. git add . (ajouter à la staging area)
   ↓
4. git status (vérifier la staging area)
   ↓
5. git commit -m "Message descriptif" (créer un commit)
   ↓
6. git log (vérifier l'historique)
   ↓
7. git push (envoyer vers GitHub)
   ↓
8. git status (vérifier la synchronisation)
```

### Workflow simplifié au quotidien

Une fois que vous êtes à l'aise, votre workflow ressemblera à :

```bash
# Modifier vos fichiers...

git add .
git commit -m "Votre message"
git push
```

---

## 9. Aide-mémoire des commandes essentielles

## 9. Aide-mémoire des commandes essentielles

| Commande                  | Description                               |
| ------------------------- | ----------------------------------------- |
| `git clone <url>`         | Cloner un repository depuis GitHub        |
| `git status`              | Voir l'état de vos fichiers               |
| `git add <fichier>`       | Ajouter un fichier à la staging area      |
| `git add .`               | Ajouter tous les fichiers modifiés        |
| `git commit -m "message"` | Créer un commit avec un message           |
| `git log`                 | Voir l'historique des commits             |
| `git log --oneline`       | Voir l'historique de manière compacte     |
| `git push`                | Pousser vos commits vers GitHub           |
| `git pull`                | Récupérer les modifications depuis GitHub |

📥 **[Télécharger l'aide-mémoire PDF complet](https://training.github.com/downloads/fr/github-git-cheat-sheet.pdf)**

---

## 10. Bonnes pratiques pour des commits de qualité

### Commits atomiques

Un **commit atomique** est un commit qui contient des modifications liées à **une seule fonctionnalité ou tâche**.

✅ **Bon exemple :**

```bash
git commit -m "Add contact form"          # Un commit
git commit -m "Add form validation"       # Un autre commit
git commit -m "Style contact form"        # Un troisième commit
```

❌ **Mauvais exemple :**

```bash
git commit -m "Add contact form, validation, styling, fix bug in header, update footer"
```

### Pourquoi faire des commits atomiques ?

1. **Facilité de compréhension**

   - Chaque commit a un but clair
   - L'historique est lisible

2. **Facilité d'annulation**

   - Si une fonctionnalité cause un problème, vous pouvez l'annuler facilement
   - Vous ne perdez pas les autres modifications

3. **Facilité de collaboration**
   - Vos collègues comprennent mieux vos changements
   - Les revues de code sont plus faciles

### Règles pour de bons messages de commit

1. **Utiliser l'impératif** : "Add" et non "Added" ou "Adding"
2. **Être concis** : 50 caractères maximum pour le titre
3. **Être descriptif** : Expliquer QUOI, pas COMMENT
4. **Une ligne suffit** pour les petits commits

✅ **Exemples de bons messages :**

- `Add user registration form`
- `Fix login button alignment`
- `Update navigation menu styles`
- `Remove unused CSS classes`
- `Refactor database connection`

### 📚 Convention Conventional Commits

Pour aller plus loin, vous pouvez adopter la convention **Conventional Commits**, utilisée par de nombreux projets open-source professionnels :

**Format :** `<type>(<scope>): <description>`

**Exemples :**

- `feat(auth): add user login form`
- `fix(header): correct alignment issue`
- `docs(readme): update installation steps`
- `style(button): improve hover effect`

📖 **[Documentation Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)**

---

## 11. Exercice pratique

Pour bien assimiler ce que vous avez appris, faites cet exercice :

### Mission : Créer un mini site web versionné

1. **Créer un nouveau repository** sur GitHub appelé `mon-portfolio`
2. **Cloner** le repository sur votre ordinateur
3. **Créer** les fichiers suivants :

   - `index.html` (page d'accueil)
   - `style.css` (styles CSS)
   - `README.md` (description du projet)

4. **Faire 3 commits atomiques** :

   - Commit 1 : `"Add HTML structure"`
   - Commit 2 : `"Add CSS styles"`
   - Commit 3 : `"Update README with project description"`

5. **Pousser** tout sur GitHub

6. **Vérifier** sur GitHub que tout est bien en ligne

### Solution étape par étape

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer le repository sur GitHub, puis le cloner
git clone https://github.com/VOTRE-NOM/mon-portfolio.git
cd mon-portfolio

# 2. Créer index.html et ajouter du contenu
# (Créez le fichier avec votre éditeur)

# 3. Premier commit
git add index.html
git commit -m "Add HTML structure"

# 4. Créer style.css et ajouter du contenu
# (Créez le fichier avec votre éditeur)

# 5. Deuxième commit
git add style.css
git commit -m "Add CSS styles"

# 6. Modifier README.md
# (Modifiez le fichier avec votre éditeur)

# 7. Troisième commit
git add README.md
git commit -m "Update README with project description"

# 8. Pousser tout sur GitHub
git push

# 9. Vérifier
git status  # Doit afficher "working tree clean"
```

</details>

---

## ✅ Checklist : Avez-vous tout compris ?

Avant de passer au module suivant, assurez-vous que vous savez :

- [ ] Créer un compte GitHub
- [ ] Créer un repository sur GitHub
- [ ] Cloner un repository
- [ ] Utiliser `git status` pour voir l'état de vos fichiers
- [ ] Utiliser `git add` pour préparer des modifications
- [ ] Utiliser `git commit` pour enregistrer des modifications
- [ ] Utiliser `git log` pour voir l'historique
- [ ] Utiliser `git push` pour envoyer sur GitHub
- [ ] Comprendre le workflow Git (working directory → staging area → repository → remote)
- [ ] Écrire de bons messages de commit

✅ **Tout est bon ?** Félicitations ! Vous maîtrisez les bases de Git !

---

## ➡️ Prochaine étape

Dans le **Module 4**, nous allons apprendre à collaborer avec d'autres développeurs :

- Travailler avec les branches
- Créer des pull requests
- Gérer les conflits
- Travailler en équipe sur GitHub

**[⬅️ Module précédent](../02-installation-setup/README.md)** | **[➡️ Aller au Module 4 : GitHub Collaboration](../04-github-collaboration/README.md)**
