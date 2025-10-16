# Module 5 : Git Avancé

## Objectifs d'apprentissage

À la fin de ce module, vous serez capable de :

- Modifier l'historique des commits avec `git commit --amend`
- Utiliser `git rebase` pour réécrire l'historique
- Fusionner des commits avec `squash`
- Séparer un commit en plusieurs commits
- Comprendre les dangers de `git push --force`
- Utiliser `git revert` pour annuler des commits en toute sécurité

---

## Prérequis

Avant de commencer ce module, assurez-vous d'avoir :

- ✅ Terminé les modules 1 à 4
- ✅ Être à l'aise avec les commandes Git de base
- ✅ Avoir déjà travaillé sur plusieurs projets avec Git

---

## ⚠️ Avertissement important

Les commandes de ce module sont **puissantes mais dangereuses** si elles sont mal utilisées.

**Règle d'or :** Ne modifiez **JAMAIS** l'historique de commits qui ont déjà été poussés sur un repository partagé avec d'autres développeurs.

---

## 1. Préparation : Créer un repository de test

Pour ce module, nous allons créer un repository de test avec plusieurs commits.

### Étape 1 : Créer le repository

1. Ouvrir le terminal et se placer sur le Bureau :

**Sur Windows (PowerShell) :**

```powershell
cd Desktop
mkdir git-advanced-test
cd git-advanced-test
git init
```

**Sur Mac/Linux :**

```bash
cd Desktop
mkdir git-advanced-test
cd git-advanced-test
git init
```

### Étape 2 : Créer plusieurs fichiers et commits

2. Créer 4 fichiers et faire 3 commits :

**Sur Windows (PowerShell) :**

```powershell
ni test1.md
git add test1.md
git commit -m "Create first file"

ni test2.md
git add test2.md
git commit -m "Create send file"

ni test3.md
git add test3.md
git commit -m "Create third file and create fourth file"
```

**Sur Mac/Linux :**

```bash
touch test1.md
git add test1.md
git commit -m "Create first file"

touch test2.md
git add test2.md
git commit -m "Create send file"

touch test3.md
git add test3.md
git commit -m "Create third file and create fourth file"
```

3. Vérifier l'historique des commits :

```bash
git log --oneline
```

Vous devriez voir vos 3 commits.

---

## 2. Modifier le dernier commit avec `git commit --amend`

### Le problème

Vous venez de faire un commit, mais vous réalisez que :

- Vous avez oublié d'ajouter un fichier
- Votre message de commit contient une faute de frappe

### La solution : `git commit --amend`

Cette commande permet de **modifier le dernier commit** en le remplaçant par un nouveau.

### Exercice pratique

1. Créer le fichier `test4.md` que nous avons oublié :

**Sur Windows (PowerShell) :**

```powershell
ni test4.md
```

**Sur Mac/Linux :**

```bash
touch test4.md
```

2. Ajouter le fichier et modifier le dernier commit :

```bash
git add test4.md
git commit --amend
```

3. L'éditeur s'ouvre. Vous pouvez modifier le message de commit si nécessaire.

   - Corriger "Create third file and create fourth file" en "Create third and fourth files"
   - Enregistrer et fermer l'éditeur

4. Vérifier le résultat :

```bash
git log --oneline
git status
```

✅ **Le dernier commit a été modifié et inclut maintenant le fichier `test4.md` !**

### ⚠️ Important

**N'utilisez `git commit --amend` QUE sur des commits qui n'ont PAS encore été poussés sur GitHub !**

Si vous modifiez un commit déjà poussé, vous allez réécrire l'historique et créer des conflits pour vos collaborateurs.

---

## 3. Modifier plusieurs commits avec `git rebase -i`

### Le problème

Vous avez remarqué une faute de frappe dans le 2ème commit : "Create send file" au lieu de "Create second file".

### La solution : `git rebase -i` (rebase interactif)

La commande `git rebase -i` permet de modifier, réorganiser, fusionner ou supprimer plusieurs commits.

### Exercice pratique

1. Lancer le rebase interactif pour modifier les 2 derniers commits :

```bash
git log --oneline
git rebase -i HEAD~2
```

Un éditeur s'ouvre avec la liste des commits :

```
pick abc1234 Create send file
pick def5678 Create third and fourth files

# Commandes :
# p, pick = utiliser le commit
# r, reword = utiliser le commit, mais modifier le message
# e, edit = utiliser le commit, mais s'arrêter pour le modifier
# s, squash = fusionner ce commit avec le précédent
# d, drop = supprimer le commit
```

2. Remplacer `pick` par `edit` pour le premier commit :

```
edit abc1234 Create send file
pick def5678 Create third and fourth files
```

3. Enregistrer et fermer l'éditeur.

Git s'arrête sur le commit à modifier et affiche :

```
You can amend the commit now, with
  git commit --amend

Once you're satisfied with your changes, run
  git rebase --continue
```

4. Modifier le message du commit :

```bash
git commit --amend
```

5. Dans l'éditeur, corriger "Create send file" en "Create second file".

6. Continuer le rebase :

```bash
git rebase --continue
```

7. Vérifier le résultat :

```bash
git log --oneline
```

✅ **La faute de frappe a été corrigée !**

### Comprendre l'ordre des commits

⚠️ Dans `git rebase -i`, les commits sont affichés **dans l'ordre chronologique** (du plus ancien au plus récent), contrairement à `git log` qui affiche les commits du plus récent au plus ancien.

---

## 4. Fusionner des commits avec `squash`

### Pourquoi fusionner des commits ?

Pendant le développement, vous faites souvent beaucoup de petits commits :

- "Add button"
- "Fix button color"
- "Fix button position"
- "Final fix for button"

Avant de pousser sur `main`, il est préférable de fusionner ces commits en un seul : "Add contact button".

### La solution : `squash`

`squash` permet de fusionner plusieurs commits en un seul pour garder un historique propre.

### Exercice pratique

1. Fusionner les 2 premiers commits :

```bash
git rebase -i --root
```

L'éditeur s'ouvre avec **tous les commits** depuis la création du repository :

```
pick abc1234 Create first file
pick def5678 Create second file
pick ghi9012 Create third and fourth files
```

2. Modifier pour fusionner le 2ème commit dans le 1er :

```
pick abc1234 Create first file
squash def5678 Create second file
pick ghi9012 Create third and fourth files
```

3. Enregistrer et fermer.

4. Un nouvel éditeur s'ouvre pour créer le message du commit fusionné :

```
# Ceci est la combinaison de 2 commits.
# Le message du 1er commit est :
Create first file

# Le message du 2ème commit est :
Create second file
```

5. Modifier le message pour :

```
Create first and second files
```

6. Enregistrer et fermer.

7. Vérifier le résultat :

```bash
git log --oneline
```

✅ **Les 2 premiers commits ont été fusionnés en un seul !**

---

## 5. Séparer un commit en plusieurs commits

### Le problème

Le commit "Create third and fourth files" fait **deux choses différentes**. C'est une mauvaise pratique : un commit devrait faire **une seule chose**.

### La solution : `git reset` avec `rebase -i`

### Exercice pratique

1. Lancer le rebase interactif :

```bash
git rebase -i HEAD~1
```

2. Changer `pick` en `edit` :

```
edit ghi9012 Create third and fourth files
```

3. Enregistrer et fermer.

4. Utiliser `git reset HEAD^` pour "défaire" le commit tout en gardant les fichiers :

```bash
git reset HEAD^
```

Cette commande :

- Annule le dernier commit
- Garde les fichiers dans le Working Directory
- Vide la Staging Area

5. Vérifier l'état :

```bash
git status
```

Vous devriez voir `test3.md` et `test4.md` comme fichiers non suivis.

6. Créer 2 commits séparés :

**Sur Windows (PowerShell) :**

```powershell
git add test3.md
git commit -m "Create third file"

git add test4.md
git commit -m "Create fourth file"
```

**Sur Mac/Linux :**

```bash
git add test3.md
git commit -m "Create third file"

git add test4.md
git commit -m "Create fourth file"
```

7. Continuer le rebase :

```bash
git rebase --continue
```

8. Vérifier le résultat :

```bash
git log --oneline
```

✅ **Le commit a été séparé en 2 commits distincts !**

### Comprendre `git reset`

`git reset` existe en 3 versions :

| Commande                       | HEAD       | Staging Area     | Working Directory |
| ------------------------------ | ---------- | ---------------- | ----------------- |
| `git reset --soft HEAD^`       | ✅ Déplacé | ❌ Inchangée     | ❌ Inchangé       |
| `git reset HEAD^` (par défaut) | ✅ Déplacé | ✅ Réinitialisée | ❌ Inchangé       |
| `git reset --hard HEAD^`       | ✅ Déplacé | ✅ Réinitialisée | ✅ Écrasé ⚠️      |

⚠️ **`git reset --hard` est dangereux** car il **supprime définitivement** vos modifications non commitées !

---

## 6. Travailler avec les repos distants : `git push --force`

### Le contexte

Vous travaillez en équipe sur un projet GitHub. Vous avez modifié l'historique localement avec `rebase` ou `amend`.

### Le problème

Quand vous essayez de pousser, Git refuse :

```
! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/...'
```

### La tentation : `git push --force`

Cette commande **force** Git à écraser l'historique distant avec votre historique local.

### ⚠️ DANGER : Démonstration

Nous allons volontairement créer un problème pour comprendre les dangers.

1. Créer un repository sur GitHub nommé `git-advanced-test`

2. Lier votre repository local à GitHub :

```bash
git remote add origin https://github.com/VOTRE-NOM/git-advanced-test.git
git branch -M main
git push -u origin main
```

3. Modifier l'historique avec rebase :

```bash
git rebase -i --root
```

4. Dans l'éditeur, supprimer le dernier commit en changeant `pick` en `drop` :

```
pick abc1234 Create first and second files
pick def5678 Create third file
drop ghi9012 Create fourth file
```

5. Enregistrer et fermer.

6. Essayer de pousser :

```bash
git push
```

❌ Git refuse car l'historique local et distant divergent.

7. Forcer le push :

```bash
git push --force
```

8. Vérifier sur GitHub et localement :

```bash
git log --oneline
```

💥 **Le fichier `test4.md` et son commit ont été détruits définitivement sur GitHub !**

Si un collègue avait basé son travail sur ce commit, son code serait cassé.

---

## 7. La solution sûre : `git revert`

### Le problème

Vous avez poussé un commit par erreur et vous voulez l'annuler.

### La mauvaise solution

❌ `git reset` + `git push --force` (détruit l'historique)

### La bonne solution

✅ `git revert` (crée un nouveau commit qui annule les modifications)

### Exercice pratique

1. Créer un nouveau fichier et le pousser :

**Sur Windows (PowerShell) :**

```powershell
ni test5.md
git add test5.md
git commit -m "Create fifth file"
git push origin main
```

**Sur Mac/Linux :**

```bash
touch test5.md
git add test5.md
git commit -m "Create fifth file"
git push origin main
```

2. Oups ! Ce fichier ne devait pas être ajouté. Annulons ce commit :

```bash
git revert HEAD
```

3. Un éditeur s'ouvre avec le message par défaut :

```
Revert "Create fifth file"

This reverts commit abc1234.
```

4. Enregistrer et fermer.

5. Pousser le commit d'annulation :

```bash
git push origin main
```

6. Vérifier l'historique :

```bash
git log --oneline
```

Vous verrez :

```
def5678 Revert "Create fifth file"
abc1234 Create fifth file
...
```

✅ **Le fichier a été supprimé SANS détruire l'historique !**

### `git revert` vs `git reset`

|                    | `git revert`             | `git reset`             |
| ------------------ | ------------------------ | ----------------------- |
| **Historique**     | Conservé                 | Détruit                 |
| **Nouveau commit** | Oui                      | Non                     |
| **Sécurité**       | ✅ Sûr                   | ⚠️ Dangereux            |
| **Usage**          | Annuler un commit public | Annuler un commit local |

---

## 8. `git push --force-with-lease` : L'alternative plus sûre

### Le problème de `git push --force`

`git push --force` écrase **aveuglément** le repository distant, même si quelqu'un d'autre a poussé des commits entre-temps.

### La solution : `git push --force-with-lease`

Cette commande vérifie d'abord si le repository distant a été modifié.

- ✅ Si personne n'a poussé : Le push fonctionne
- ❌ Si quelqu'un a poussé : Le push est refusé avec un message d'erreur

### Exemple

```bash
git push --force-with-lease origin main
```

Si un collègue a poussé un commit avant vous :

```
! [rejected]        main -> main (stale info)
error: failed to push some refs
```

Vous devez alors :

1. Récupérer les modifications :

```bash
git pull --rebase
```

2. Résoudre les éventuels conflits

3. Pousser à nouveau :

```bash
git push --force-with-lease origin main
```

### Quand utiliser `--force` ?

Cas d'usage **légitimes** (rares) :

- ✅ Mettre à jour une Pull Request sur votre propre branche
- ✅ Supprimer des informations sensibles (mots de passe, clés API)
- ✅ Après avoir demandé et obtenu l'accord de toute l'équipe

---

## 9. Récapitulatif : Dangers et bonnes pratiques

### ⚠️ Commandes dangereuses

| Commande             | Danger                       | Quand l'éviter            |
| -------------------- | ---------------------------- | ------------------------- |
| `git commit --amend` | Réécrit le dernier commit    | Après un `git push`       |
| `git rebase`         | Réécrit l'historique         | Sur une branche partagée  |
| `git reset --hard`   | Supprime les modifications   | Toujours vérifier avant ! |
| `git push --force`   | Écrase le repository distant | Presque toujours          |

### ✅ Bonnes pratiques

1. **Ne modifiez JAMAIS l'historique public**

   - Si le commit est sur GitHub et partagé : ❌ N'utilisez pas `rebase`, `amend`, `reset`
   - Si le commit est uniquement local : ✅ Vous pouvez modifier

2. **Travaillez sur vos propres branches**

   - Sur `feature/ma-fonctionnalite` : ✅ Vous pouvez tout faire
   - Sur `main` partagée : ❌ Soyez prudent

3. **Communiquez avec votre équipe**

   - Prévenez si vous devez modifier l'historique
   - Expliquez pourquoi c'est nécessaire

4. **Préférez `git revert` à `git reset`**

   - `revert` : ✅ Sûr, conserve l'historique
   - `reset` : ⚠️ Dangereux, détruit l'historique

5. **Utilisez `--force-with-lease` au lieu de `--force`**

   - `--force-with-lease` : ✅ Vérifie avant d'écraser
   - `--force` : ⚠️ Écrase sans vérification

6. **Ne poussez pas après chaque commit**
   - Faites plusieurs commits locaux
   - Nettoyez l'historique avec `rebase -i`
   - Poussez une seule fois

---

## 10. Cas pratiques : Quand utiliser quelle commande ?

### Scénario 1 : J'ai oublié un fichier dans mon dernier commit (non poussé)

✅ **Solution :**

```bash
git add fichier-oublie.md
git commit --amend --no-edit
```

---

### Scénario 2 : J'ai une faute de frappe dans un commit d'il y a 3 commits (non poussés)

✅ **Solution :**

```bash
git rebase -i HEAD~3
# Changer "pick" en "reword" pour le commit concerné
```

---

### Scénario 3 : J'ai fait 5 petits commits et je veux les fusionner avant de pousser

✅ **Solution :**

```bash
git rebase -i HEAD~5
# Changer "pick" en "squash" pour les 4 derniers commits
```

---

### Scénario 4 : J'ai poussé un commit par erreur sur `main`

✅ **Solution :**

```bash
git revert HEAD
git push origin main
```

❌ **NE PAS FAIRE :**

```bash
git reset HEAD^
git push --force
```

---

### Scénario 5 : Je dois mettre à jour ma Pull Request

✅ **Solution :**

```bash
git rebase -i origin/main
# Nettoyer les commits
git push --force-with-lease origin ma-branche
```

---

### Scénario 6 : J'ai committé un mot de passe par accident (déjà poussé)

⚠️ **Solution (complexe) :**

1. Supprimer le mot de passe du code
2. Révoquer immédiatement le mot de passe (le changer)
3. Utiliser `git filter-branch` ou `BFG Repo-Cleaner` pour nettoyer l'historique
4. Forcer le push : `git push --force`
5. Prévenir toute l'équipe de re-cloner le repository

**Prévention :** Utilisez des fichiers `.env` et ajoutez-les au `.gitignore` !

---

## Exercice final (optionnel)

### Objectif

Créer un historique Git propre avec toutes les techniques apprises.

### Instructions

1. Créer un nouveau repository `git-advanced-exercise`

2. Créer 6 fichiers avec 6 commits :

   - `header.html` - "Add header"
   - `footer.html` - "Add footer"
   - `style.css` - "Add styles"
   - `script.js` - "Add script"
   - `README.md` - "Add readme"
   - `contact.html` - "Add contact page"

3. Utiliser `git rebase -i` pour :

   - Fusionner les commits `header` et `footer` en "Add HTML structure"
   - Fusionner les commits `style` et `script` en "Add assets"
   - Garder `README` et `contact` séparés

4. Résultat final attendu (4 commits) :

   ```
   Add contact page
   Add readme
   Add assets
   Add HTML structure
   ```

5. Pousser sur GitHub

6. Ajouter un fichier `test.txt` par erreur et le pousser

7. Utiliser `git revert` pour annuler ce commit

✅ **Vous maîtrisez maintenant Git avancé !**

---

## Ressources complémentaires

- [Documentation officielle Git - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [Atlassian - Git Rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - Pour corriger les erreurs Git

---

## ⬅️ Navigation

[← Retour au Module 4 : Collaboration avec GitHub](../04-github-collaboration/README.md)

---

**🎉 Félicitations ! Vous avez terminé la formation Git & GitHub !**
