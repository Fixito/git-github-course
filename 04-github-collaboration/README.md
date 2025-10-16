# Module 4 : Collaboration avec GitHub

## Objectifs d'apprentissage

À la fin de ce module, vous serez capable de :

- Comprendre et utiliser les branches
- Créer et fusionner des branches
- Travailler avec des pull requests
- Collaborer avec d'autres développeurs
- Gérer les conflits de base

---

## 1. Pourquoi utiliser des branches ?

Imaginez que vous travaillez sur un site web en production (utilisé par des clients). Vous voulez ajouter une nouvelle fonctionnalité, mais vous ne voulez pas casser le site pendant le développement.

### Sans branches 😰

```
main: ✅ → ⚠️ → ❌ → ⚠️ → ✅
      OK   Bug  Crash Bug   OK (après 1 semaine de galère)
```

Les clients voient tous vos bugs pendant le développement !

### Avec branches 😊

```
main:     ✅ → ✅ → ✅ → ✅ → ✅
                           ↗
feature:        ⚠️ → ❌ → ⚠️ → ✅
                (développement isolé)
```

Les clients ne voient que le code stable. Vous développez tranquillement dans votre branche !

### Qu'est-ce qu'une branche ?

Une **branche** est une version parallèle de votre code qui permet de :

- Développer de nouvelles fonctionnalités sans toucher au code principal
- Tester des idées sans risque
- Travailler en équipe sur différentes fonctionnalités en même temps

**Analogie :** Imaginez un livre avec plusieurs brouillons. La version finale publiée est la branche `main`, et vos brouillons sont des branches de développement.

---

## 2. Créer et utiliser des branches

### Visualiser les branches

Voir les branches existantes :

```bash
git branch
```

Résultat :

```
* main
```

L'astérisque `*` indique la branche sur laquelle vous êtes actuellement.

### Créer une nouvelle branche

Créer une branche appelée `add-contact-page` :

```bash
git branch add-contact-page
```

Vérifier :

```bash
git branch
```

Résultat :

```
  add-contact-page
* main
```

La branche est créée, mais vous êtes toujours sur `main`.

### Changer de branche

Se déplacer vers la nouvelle branche :

```bash
git checkout add-contact-page
```

ou avec la nouvelle syntaxe (Git 2.23+) :

```bash
git switch add-contact-page
```

Résultat :

```
Switched to branch 'add-contact-page'
```

### Raccourci : créer et changer de branche en une commande

```bash
git checkout -b add-contact-page
```

ou

```bash
git switch -c add-contact-page
```

**Traduction :** "Créer une branche `add-contact-page` et me déplacer dessus"

---

## 3. Travailler sur une branche

### Exemple pratique : Ajouter une page de contact

1. **Créer et se déplacer sur une nouvelle branche** :

```bash
git checkout -b add-contact-page
```

2. **Créer le fichier `contact.html`** :

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Contact</title>
  </head>
  <body>
    <h1>Contactez-nous</h1>

    <form>
      <div>
        <label for="name">Nom : </label>
        <input type="text" id="name" name="name" />
      </div>

      <div>
        <label for="email">Email : </label>
        <input type="email" id="email" name="email" />
      </div>

      <div>
        <label for="message">Message : </label>
        <textarea id="message" name="message"></textarea>
      </div>

      <button>Envoyer</button>
    </form>
  </body>
</html>
```

3. **Ajouter et commiter** :

```bash
git add contact.html
git commit -m "Add contact page"
```

4. **Vérifier l'historique** :

```bash
git log --oneline
```

Vous devriez voir :

```
abc123d (HEAD -> add-contact-page) Add contact page
xyz789e (main) Initial commit
```

### Comprendre HEAD

**HEAD** = Pointeur qui indique où vous êtes actuellement dans l'historique Git

- `HEAD -> add-contact-page` = Vous êtes sur la branche `add-contact-page`
- `main` = La branche principale est restée au commit précédent

---

## 4. Fusionner une branche (Merge)

Une fois votre fonctionnalité terminée, vous voulez l'intégrer dans la branche principale.

### Processus de fusion

1. **Retourner sur la branche principale** :

```bash
git checkout main
```

2. **Fusionner la branche de fonctionnalité** :

```bash
git merge add-contact-page
```

Résultat :

```
Updating xyz789e..abc123d
Fast-forward
 contact.html | 15 +++++++++++++++
 1 file changed, 15 insertions(+)
 create mode 100644 contact.html
```

### Comprendre "Fast-forward"

**Fast-forward** = Git déplace simplement le pointeur `main` vers le dernier commit de la branche

```
Avant merge:
main:     A → B
               ↘
feature:        C → D

Après merge (fast-forward):
main:     A → B → C → D
```

3. **Vérifier** :

```bash
git log --oneline
```

```
abc123d (HEAD -> main, add-contact-page) Add contact page
xyz789e Initial commit
```

Les deux branches pointent maintenant vers le même commit !

4. **Supprimer la branche** (optionnel) :

Une fois fusionnée, la branche n'est plus nécessaire :

```bash
git branch -d add-contact-page
```

---

## 5. Pousser une branche sur GitHub

Si vous travaillez en équipe, vous voudrez pousser votre branche sur GitHub avant de la fusionner.

### Scénario : Créer une Pull Request

1. **Créer une nouvelle branche** :

```bash
git checkout -b add-footer
```

2. **Faire des modifications et commiter** :

```bash
# Créer footer.html et ajouter du contenu
git add footer.html
git commit -m "Add footer section"
```

3. **Pousser la branche sur GitHub** :

```bash
git push origin add-footer
```

ou la première fois :

```bash
git push -u origin add-footer
```

**Explication :**

- `origin` = Le repository distant (GitHub)
- `add-footer` = Le nom de votre branche
- `-u` = Créer une connexion entre votre branche locale et la branche distante

4. **Créer une Pull Request sur GitHub** :

- Allez sur GitHub
- Vous verriez un message : "add-footer had recent pushes"
- Cliquez sur **"Compare & pull request"**
- Ajoutez un titre et une description
- Cliquez sur **"Create pull request"**

---

## 6. Qu'est-ce qu'une Pull Request ?

Une **Pull Request** (PR) est une demande pour fusionner votre code dans la branche principale.

### Pourquoi utiliser des Pull Requests ?

✅ **Revue de code** : Vos collègues peuvent relire votre code avant qu'il soit fusionné

✅ **Discussion** : Vous pouvez discuter des modifications et suggérer des améliorations

✅ **Tests automatiques** : GitHub peut lancer des tests automatiques (CI/CD)

✅ **Historique** : Tout est documenté et traçable

### Processus d'une Pull Request

```
1. Vous créez une branche
   ↓
2. Vous développez votre fonctionnalité
   ↓
3. Vous poussez la branche sur GitHub
   ↓
4. Vous créez une Pull Request
   ↓
5. Vos collègues relisent le code
   ↓
6. Vous faites des modifications si nécessaire
   ↓
7. La Pull Request est approuvée
   ↓
8. Vous fusionnez la branche dans main
   ↓
9. Vous supprimez la branche
```

### Créer une Pull Request sur GitHub

**Étape 1 : Accéder aux Pull Requests**

Sur votre repository GitHub, cliquez sur **"Pull requests"** puis **"New pull request"**

**Étape 2 : Choisir les branches**

- **base:** `main` (la branche de destination)
- **compare:** `add-footer` (votre branche)

**Étape 3 : Remplir les informations**

```markdown
Titre : Add footer section

Description :

## Changements

- Ajout d'un footer avec les liens de navigation
- Ajout des informations de copyright
- Design responsive

## Captures d'écran

[Ajoutez des images si nécessaire]

## Checklist

- [x] Le code fonctionne
- [x] Les styles sont appliqués
- [x] Testé sur mobile
```

**Étape 4 : Créer la Pull Request**

Cliquez sur **"Create pull request"**

**Étape 5 : Merger la Pull Request**

Une fois approuvée, cliquez sur **"Merge pull request"** puis **"Confirm merge"**

---

## 7. Récupérer les modifications des autres (git pull)

Quand vous travaillez en équipe, vos collègues vont aussi pousser du code sur GitHub.

### Récupérer les dernières modifications

```bash
# S'assurer d'être sur la branche main
git checkout main

# Récupérer les modifications
git pull
```

ou de manière plus explicite :

```bash
git pull origin main
```

### Que fait git pull ?

`git pull` = `git fetch` + `git merge`

1. **git fetch** : Télécharge les modifications depuis GitHub (sans les appliquer)
2. **git merge** : Fusionne ces modifications avec votre branche locale

### Workflow en équipe

```bash
# Avant de commencer à travailler
git switch main
git pull  # Récupérer les dernières modifications

# Créer une nouvelle branche
git checkout -b ma-nouvelle-fonctionnalite

# Travailler...
git add .
git commit -m "Add feature"

# Pousser sur GitHub
git push -u origin ma-nouvelle-fonctionnalite

# Créer une Pull Request sur GitHub
# Attendre l'approbation
# Merger sur GitHub

# Revenir sur main et récupérer votre travail mergé
git switch main
git pull

# Supprimer la branche locale
git branch -d ma-nouvelle-fonctionnalite
```

---

## 8. Gérer les conflits (niveau débutant)

Un **conflit** se produit quand deux personnes modifient la même partie d'un fichier.

### Exemple concret : Créons ensemble un conflit

Pour bien comprendre, suivons un exemple pas à pas où nous allons volontairement créer un conflit.

**Situation :** Vous travaillez sur le titre de votre site. En même temps, sur deux branches différentes, vous modifiez le même titre.

#### Étape 1 : Créer un projet de test

```bash
# Sur votre Bureau
cd Desktop
mkdir conflict-test
cd conflict-test
git init
```

#### Étape 2 : Créer un fichier HTML de base

Créez un fichier `index.html` :

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Test conflit</title>
  </head>
  <body>
    <h1>Titre</h1>
    <p>Contenu</p>
  </body>
</html>
```

```bash
git add index.html
git commit -m "Add initial HTML file"
```

#### Étape 3 : Créer une branche et modifier le titre

```bash
# Créer et aller sur une nouvelle branche
git checkout -b feature/update-title
```

Modifiez `index.html` :

```html
<h1>Nouveau titre</h1>
```

```bash
git add index.html
git commit -m "Update title in feature branch"
```

#### Étape 4 : Retourner sur main et modifier LE MÊME titre

```bash
# Retour sur main
git checkout main
```

Modifiez `index.html` - Changez la même ligne :

```html
<h1>Titre principal</h1>
```

```bash
git add index.html
git commit -m "Update title in main branch"
```

#### Étape 5 : Fusionner et créer le conflit ! 💥

```bash
git merge feature/update-title
```

**Résultat :**

```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

🎉 **Vous avez créé un conflit !** C'est normal, Git ne sait pas quelle version garder.

### Comment Git marque un conflit

Ouvrez `index.html`, vous verrez :

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <title>Test Conflits</title>
  </head>
  <body>
    <<<<<<< HEAD
    <h1>Titre principal</h1>
    =======
    <h1>Nouveau titre</h1>
    >>>>>>> feature/update-title
    <p>Contenu original</p>
  </body>
</html>
```

**Explication des marqueurs :**

- `<<<<<<< HEAD` → Début de votre version (branche `main`)
- `=======` → Séparateur entre les deux versions
- `>>>>>>> feature/update-title` → Fin de la version de la branche `feature/update-title`

### Résoudre un conflit

#### Étape 1 : Choisir la bonne version

**Option A :** Garder "Titre principal"

```html
<h1>Titre principal</h1>
```

**Option B :** Garder "Nouveau titre"

```html
<h1>Nouveau titre</h1>
```

**Option C :** Combiner les deux

```html
<h1>Titre principal - Version mise à jour</h1>
```

Choisissez une option et **supprimez tous les marqueurs** (`<<<<<<<`, `=======`, `>>>>>>>`).

#### Étape 2 : Fichier final sans marqueurs

Votre `index.html` devrait ressembler à :

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <title>Test Conflits</title>
  </head>
  <body>
    <h1>Titre principal</h1>
    <p>Contenu original</p>
  </body>
</html>
```

#### Étape 3 : Finaliser la résolution

```bash
# Vérifier le statut
git status
# → Affiche : "both modified: index.html"

# Ajouter le fichier résolu
git add index.html

# Finaliser le merge
git commit -m "Resolve merge conflict in index.html"
```

✅ **Conflit résolu !**

#### Étape 4 : Vérifier l'historique

```bash
git log --oneline --graph --all
```

**Explication de la commande :**

- `git log` → Affiche l'historique des commits
- `--oneline` → Format compact (1 ligne par commit)
- `--graph` → Dessine un graphe ASCII montrant les branches
- `--all` → Affiche toutes les branches (pas seulement la branche actuelle)

Résultat :

```
*   abc123d (HEAD -> main) Resolve merge conflict in index.html
|\
| * def456e (feature/update-title) Update title in feature branch
* | ghi789f Update title in main branch
|/
* jkl012m Add initial HTML file
```

Ce graphe montre :

- Les deux branches qui divergent
- Le commit de résolution du conflit qui les fusionne

### 💡 Résoudre les conflits avec VS Code

Si vous utilisez **VS Code**, c'est encore plus facile :

1. VS Code détecte automatiquement les conflits
2. Vous voyez des boutons cliquables :

   - **Accept Current Change** → Garder la version de `main`
   - **Accept Incoming Change** → Garder la version de la branche
   - **Accept Both Changes** → Garder les deux
   - **Compare Changes** → Comparer visuellement

3. Cliquez sur le bouton de votre choix
4. Enregistrez le fichier
5. Faites `git add` et `git commit`

### Éviter les conflits

✅ **Communiquer** avec votre équipe sur qui travaille sur quoi

✅ **Faire des pull régulièrement** pour avoir le code à jour

✅ **Faire des commits atomiques** pour faciliter la résolution

✅ **Diviser le travail** : chacun travaille sur des fichiers différents

---

## 9. Bonnes pratiques pour les branches

### Nommage des branches

✅ **Bons noms de branches :**

- `add-contact-form`
- `fix-login-bug`
- `update-homepage-design`
- `feature/user-authentication`
- `bugfix/header-alignment`

❌ **Mauvais noms de branches :**

- `test`
- `branch1`
- `nouvelle-branche`
- `fix` (trop vague)

### Conventions de nommage

```
type/description

Types courants :
- feature/   → Nouvelle fonctionnalité
- bugfix/    → Correction de bug
- hotfix/    → Correction urgente en production
- refactor/  → Refactorisation du code
- docs/      → Documentation
```

**Exemples :**

```
feature/add-user-profile
bugfix/fix-header-responsive
hotfix/critical-security-patch
refactor/improve-database-queries
docs/update-readme
```

### Workflow Git Flow (simplifié pour débutants)

```
main (production)
  ↓
develop (développement)
  ↓
feature branches (nouvelles fonctionnalités)
```

**Processus :**

1. Créer une branche depuis `develop`
2. Développer la fonctionnalité
3. Créer une Pull Request vers `develop`
4. Merger dans `develop`
5. Quand `develop` est stable, merger dans `main`

---

## 10. Commandes essentielles pour les branches

| Commande                | Description                       |
| ----------------------- | --------------------------------- |
| `git branch`            | Lister toutes les branches        |
| `git branch <nom>`      | Créer une nouvelle branche        |
| `git switch <nom>`      | Changer de branche                |
| `git switch -c <nom>`   | Créer et changer de branche       |
| `git branch -d <nom>`   | Supprimer une branche (fusionnée) |
| `git branch -D <nom>`   | Supprimer une branche (forcé)     |
| `git merge <nom>`       | Fusionner une branche             |
| `git push origin <nom>` | Pousser une branche sur GitHub    |
| `git pull`              | Récupérer les modifications       |

---

## 11. Exercice pratique

### Mission : Travailler en équipe (simulation)

Pour cet exercice, vous allez simuler le travail d'équipe en créant plusieurs branches.

**Scénario :** Vous créez un site web avec plusieurs pages.

1. **Créer un repository** `team-project` sur GitHub et le cloner

2. **Créer le fichier de base** :

   ```bash
   # Créer index.html avec une structure de base
   git add index.html
   git commit -m "Add base HTML structure"
   git push
   ```

3. **Branche 1 : Ajouter une page À propos** :

   ```bash
   git checkout -b feature/about-page

   # Créer about.html avec une structure de base
   git add about.html
   git commit -m "Add about page"
   git push -u origin feature/about-page
   ```

4. **Retourner sur main et créer une autre branche** :

   ```bash
   git checkout main
   git checkout -b feature/contact-page

   # Créer contact.html avec une structure de base
   git add contact.html
   git commit -m "Add contact page"
   git push -u origin feature/contact-page
   ```

5. **Créer des Pull Requests** sur GitHub pour chaque branche

6. **Merger les Pull Requests** dans l'ordre

7. **Récupérer le code à jour** :

   ```bash
   git checkout main
   git pull
   ```

8. **Créer un conflit volontaire pour s'entraîner** :

   ```bash
   # Créer une branche pour modifier le titre
   git checkout -b feature/update-title

   # Modifier index.html : changer <h1>Bienvenue</h1> en <h1>Bienvenue sur notre site</h1>
   git add index.html
   git commit -m "Update main title"

   # Retourner sur main et modifier le MÊME titre
   git checkout main

   # Modifier index.html : changer <h1>Bienvenue</h1> en <h1>Bienvenue chez nous</h1>
   git add index.html
   git commit -m "Change welcome title"

   # Essayer de fusionner → Conflit !
   git merge feature/update-title

   # Résoudre le conflit dans index.html
   # Choisir une version ou combiner, supprimer les marqueurs
   git add index.html
   git commit -m "Resolve title conflict"
   ```

9. **Vérifier que tout est bien fusionné** :

   ```bash
   git log --oneline --graph
   ```

10. **Supprimer les branches**

### Solution détaillée

<details>
<summary>Cliquez pour voir la solution complète</summary>

```bash
# 1. Créer le repository sur GitHub, puis
git clone https://github.com/VOTRE-NOM/team-project.git
cd team-project
```

```html
<!-- 2. Créer index.html -->
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Projet en équipe</title>
  </head>
  <body>
    <h1>Bienvenue</h1>
  </body>
</html>
```

```bash
git add index.html
git commit -m "Add base HTML structure"
git push
```

```bash
# 3. Créer et aller sur la branche about-page
git checkout -b feature/about-page
```

```html
<!-- Créer about.html -->
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>À propos</title>
  </head>
  <body>
    <h1>À propos de nous</h1>
    <p>Nous sommes une équipe de développeurs.</p>
  </body>
</html>
```

```bash
git add about.html
git commit -m "Add about page"
git push -u origin feature/about-page
```

```bash
# 4. Retour sur main et création de la branche contact
git checkout main
git checkout -b feature/contact-page
```

```html
<!-- Créer contact.html -->
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Contact</title>
  </head>
  <body>
    <h1>Contactez-nous</h1>
    <p>Email: contact@example.com</p>
  </body>
</html>
```

```bash
git add contact.html
git commit -m "Add contact page"
git push -u origin feature/contact-page
```

```bash
# 5-6. Aller sur GitHub et merger les PR
```

```bash
# 7. Récupérer tout
git checkout main
git pull
```

```bash
# 8. Créer un conflit volontaire
git checkout -b feature/update-title
```

```html
<!-- Modifier index.html : changer le titre -->
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Projet en équipe</title>
  </head>
  <body>
    <h1>Bienvenue sur notre site</h1>
  </body>
</html>
```

```bash
git add index.html
git commit -m "Update main title"

# Retourner sur main
git checkout main
```

```html
<!-- Modifier index.html : changer le MÊME titre -->
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Projet en équipe</title>
  </head>
  <body>
    <h1>Bienvenue chez nous</h1>
  </body>
</html>
```

```bash
git add index.html
git commit -m "Change welcome title"

# Fusionner → Conflit !
git merge feature/update-title

# Résoudre le conflit dans index.html
# Ouvrir le fichier, choisir "Bienvenue sur notre site collaboratif"
# Supprimer les marqueurs <<<<<<<, =======, >>>>>>>
git add index.html
git commit -m "Resolve title conflict"
```

```bash
# 9. Voir l'historique avec le graphe
git log --oneline --graph

# 10. Nettoyer les branches locales
git branch -d feature/about-page
git branch -d feature/contact-page
git branch -d feature/update-title
```

</details>

---

## 12. Ressources complémentaires

### 📚 Documentation

- [Documentation Git sur les branches](https://git-scm.com/book/fr/v2/Les-branches-avec-Git-Les-branches-en-bref)

### 🎮 Pratique interactive

- [Learn Git Branching](https://learngitbranching.js.org/?locale=fr_FR) - Exercices interactifs sur les branches

---

## ✅ Checklist : Maîtrisez-vous la collaboration ?

Avant de terminer ce module, vérifiez que vous savez :

- [ ] Créer une branche
- [ ] Changer de branche
- [ ] Visualiser les branches existantes
- [ ] Fusionner une branche
- [ ] Supprimer une branche
- [ ] Pousser une branche sur GitHub
- [ ] Créer une Pull Request
- [ ] Merger une Pull Request
- [ ] Récupérer les modifications avec `git pull`
- [ ] Résoudre un conflit simple
- [ ] Comprendre le workflow de collaboration

✅ **Tout est maîtrisé ?** Vous êtes prêt à travailler en équipe !

---

## 🎉 Félicitations !

Vous avez terminé les modules essentiels sur Git et GitHub ! Vous savez maintenant :

✅ Pourquoi versionner son code (Module 1)
✅ Installer et configurer votre environnement (Module 2)
✅ Utiliser Git au quotidien (Module 3)
✅ Collaborer avec GitHub (Module 4)

Vous êtes prêt à travailler sur des projets professionnels !

---

## ➡️ Pour aller plus loin

Si vous voulez approfondir vos connaissances, consultez le **Module Git avancé** qui couvre des sujets avancés :

- Réécrire l'historique (rebase, amend)
- Techniques avancées de merge
- Git reset et ses variantes
- Stratégies de branching complexes

**[⬅️ Module précédent](../03-git-basics/README.md)** | **[➡️ Module Git avancé](../05-advanced-git/README.md)** (optionnel)
