# Module 1 : Pourquoi versionner son code ?

## Objectifs d'apprentissage

À la fin de ce module, vous serez capable de :

- Comprendre ce qu'est le versioning de code
- Identifier les problèmes que le versioning résout
- Expliquer les apports concrets du versioning dans un projet

---

## 1. Qu'est-ce que le versioning ?

Le **versioning** (ou contrôle de version) est une pratique qui consiste à **enregistrer et suivre les modifications apportées à des fichiers au fil du temps**.

Imaginez que vous travaillez sur un projet de site web. Sans versioning, vous pourriez avoir des fichiers comme :

```
mon-site.html
mon-site-v2.html
mon-site-v3-final.html
mon-site-v3-final-vraiment.html
mon-site-v3-final-vraiment-last.html
```

😵 **Problème :** C'est le chaos ! Quelle est la bonne version ? Quelles modifications ont été faites entre chaque version ?

Avec le versioning, vous n'avez qu'**un seul fichier** `mon-site.html`, et l'historique complet des modifications est automatiquement enregistré.

---

## 2. Les apports du versioning

### 🔒 2.1. Sauvegarde sécurisée

**Sans versioning :**

- Vous supprimez accidentellement une fonctionnalité importante
- Votre disque dur tombe en panne
- ➡️ Vous perdez tout votre travail

**Avec versioning :**

- Chaque modification est sauvegardée
- Vous pouvez revenir à n'importe quelle version précédente
- Vos fichiers sont sauvegardés sur un serveur distant (GitHub, GitLab, etc.)
- ✅ **Votre travail est protégé**

### 📝 2.2. Suivi des modifications

**Avec le versioning, vous pouvez :**

- Voir **qui** a modifié un fichier
- Voir **quand** la modification a été faite
- Voir **quoi** exactement a été modifié (lignes ajoutées/supprimées)
- Comprendre **pourquoi** grâce aux messages de commit

**Exemple pratique :**

```
Commit 1 - 10/10/2025 par Marie
Message : "Ajoute le formulaire de contact"
Fichiers modifiés : contact.html, style.css

Commit 2 - 11/10/2025 par Thomas
Message : "Corrige le bug d'envoi du formulaire"
Fichiers modifiés : contact.html, script.js
```

Grâce à cet historique, vous savez exactement ce qui s'est passé et quand !

### 📦 2.3. Versions figées livrables

Lors du développement d'une application, vous devez créer des **versions stables** à livrer à vos clients ou à déployer en production.

**Sans versioning :**

- Vous ne savez pas quelle version est en production
- Difficile de reproduire un bug signalé par un client
- Impossible de savoir quelles fonctionnalités sont dans quelle version

**Avec versioning :**

- Vous créez des **tags** ou des **releases** (ex: v1.0.0, v1.1.0, v2.0.0)
- Vous savez exactement quel code est en production
- Vous pouvez facilement revenir à une version précédente en cas de problème
- ✅ **Gestion professionnelle des versions**

**Exemple :**

```
v1.0.0 (15/09/2025) - Version initiale du site
v1.1.0 (01/10/2025) - Ajout du formulaire de contact
v1.2.0 (15/10/2025) - Ajout de la newsletter
```

### 🔄 2.4. Synchronisation des environnements de travail

Imaginez que vous travaillez sur plusieurs ordinateurs ou en équipe.

**Sans versioning :**

- Vous devez envoyer vos fichiers par email ou clé USB
- Risque d'écraser le travail des autres
- Impossible de travailler en même temps sur les mêmes fichiers
- Confusion totale sur "qui a la bonne version"

**Avec versioning :**

- Tous les membres de l'équipe travaillent sur le même dépôt central
- Chacun peut télécharger la dernière version du projet
- Les modifications de chacun sont intégrées de manière contrôlée
- Vous pouvez travailler sur différentes fonctionnalités en parallèle
- ✅ **Travail collaboratif fluide et synchronisé**

**Scénario concret :**

1. **Lundi matin au bureau :** Vous téléchargez la dernière version du projet
2. **Lundi soir :** Vous poussez vos modifications sur le serveur
3. **Mardi matin chez vous :** Vous téléchargez vos modifications d'hier
4. **Votre collègue** peut aussi télécharger vos modifications et continuer votre travail

---

## 3. Git : l'outil de versioning le plus populaire

**Git** est le système de contrôle de version le plus utilisé au monde. Il a été créé en 2005 par Linus Torvalds (créateur de Linux).

### Pourquoi Git ?

- ✅ **Gratuit et open-source**
- ✅ **Rapide et performant**
- ✅ **Fonctionne hors ligne** (vous n'avez pas besoin d'internet pour versionner votre code)
- ✅ **Utilisé par les plus grandes entreprises** (Google, Microsoft, Facebook, etc.)
- ✅ **Compatible avec GitHub, GitLab, Bitbucket** (plateformes de partage de code)

### Git vs GitHub

⚠️ **Attention à ne pas confondre :**

- **Git** = Le logiciel de versioning installé sur votre ordinateur
- **GitHub** = Un site web pour stocker et partager vos dépôts Git en ligne

**Analogie :**

- Git = Microsoft Word (le logiciel)
- GitHub = Google Drive (le service de stockage en ligne)

---

## 4. Cas d'usage concrets pour des débutants

### 🎓 Cas 1 : Projet d'étudiant

Vous développez votre portfolio pour le montrer à des employeurs.

**Sans Git :**

```
portfolio/
  ├── index-backup.html
  ├── index-v2.html
  ├── index-final.html
  ├── index-final2.html
  └── style-old.css
```

**Avec Git :**

```
portfolio/
  ├── index.html
  └── style.css
```

- Un historique complet de toutes vos modifications

### 💼 Cas 2 : Projet en entreprise

Vous travaillez avec 3 collègues sur un site e-commerce.

**Avec Git :**

- Marie travaille sur le système de paiement
- Thomas travaille sur le catalogue de produits
- Julie travaille sur le design
- Vous travaillez sur le panier d'achat

Chacun peut travailler en parallèle, et Git vous aide à fusionner tout le travail ensemble !

### 🚨 Cas 3 : Bug en production

Un client signale un bug critique sur le site en production.

**Avec Git :**

1. Vous identifiez quand le bug est apparu grâce à l'historique
2. Vous voyez exactement quelle modification a causé le problème
3. Vous pouvez revenir à la version précédente en attendant de corriger le bug
4. ✅ **Problème résolu rapidement !**

---

## 5. Récapitulatif

Le versioning avec Git vous permet de :

| Apport                        | Bénéfice concret                               |
| ----------------------------- | ---------------------------------------------- |
| **Sauvegarde**                | Ne plus jamais perdre votre travail            |
| **Suivi des modifications**   | Savoir qui a fait quoi et quand                |
| **Versions figées livrables** | Gérer professionnellement vos releases         |
| **Synchronisation**           | Travailler en équipe ou sur plusieurs machines |

---

## 📚 Ressources complémentaires

- [Article : Pourquoi utiliser Git](https://www.atlassian.com/fr/git/tutorials/why-git)

---

## ➡️ Prochaine étape

Dans le **Module 2**, nous allons installer tous les outils nécessaires :

- Git sur votre ordinateur
- Configuration de votre environnement de développement

**[➡️ Aller au Module 2 : Installation et Configuration](../02-installation-setup/README.md)**
