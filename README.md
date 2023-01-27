# Introduction à Git

Définir la branche locale de git par défaut à `main`

```sh
git config --global init.defaultBranch main
```

Pour plus d'info sur le changement de `master` à `main` voir [renommage de repo GitHub.](https://github.com/github/renaming)

## Céer un repository

1. Se créer un compte GitHub.

2. Créer un nouveau repository avec le bouton `New Repository`.

3. Appeler le repo `git-test`, cocher `Add a README file` et le créer en cliquant sur le bouton `Create repository`.

Cela nous redirige vers la page du repo sur GitHib.

4. Pour cloner le repo, cliquer sur le bouton vert `Code`, sélectionner l'option `HTTPS`ry copier le lien en-dessous.

Nous allons maintent utiliser le terminal.

5. Créer un nouveau dossier `repos` avec la commande `mkdir`

6. Cloner le repository avec la commande `git clone` suivie du lien que nous avons copié

La commande devrait ressembler à :

```sh
git clone git@github.com:USER-NAME/REPOSITORY-NAME.git
```

7. Aller dans le dossier `git-test` que nous venons de télécharger avec la commande `cd`.

8. Utiliser la commande `git remote -v` pour afficher l'URL du repository
