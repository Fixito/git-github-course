# Un regard plus approfondi sur Git

## Changer l'historique

Supposons donc que vous soyez à l'aise pour écrire de bons messages de validation et que vous travaillez avec des branches pour avoir un bon flux de travail Git. Mais personne n'est parfait, et pendant que vous écrivez un beau code, quelque chose ne va pas ! Peut-être que vous vous engagez trop tôt et qu'il vous manque un fichier. Peut-être que vous gâchez l'un de vos messages de validation et omettez un détail vital.

### setup

- Créer un repository

- Faire les commandes suivantes :

```sh
  touch test{1..4}.md
  git add test1.md && git commit -m 'Create first file'
  git add test2.md && git commit -m 'Create send file'
  git add test3.md && git commit -m 'Create third file and create fourth file'
```

### Changer le dernier commit

Donc, si nous regardons le dernier commit que nous avons fait une bêtise, si on tape `git status` et `git log`, on peut voir que nous avons oublié d'ajouter un fichier !

- Ajouter le fichier manquant et éxécuter

```sh
  git add test4.md
  git commit --amend
```

Ce qui s'est passé ici, c'est que nous avons d'abord mis à jour la zone de staging pour inclure le fichier manquant, puis nous avons remplacé le dernier commit par notre nouveau pour inclure le fichier manquant. Si nous le voulions, nous aurions pu changer le message du commit et il aurait écrasé le message du commit précédent.

**N'oubliez pas de modifier uniquement les commits qui n'ont été poussés nulle part !**

La raison est que git `commit --amend` ne se contente pas de modifier le dernier commit, il remplace ce commit par un tout nouveau.

Cela signifie que vous pourriez potentiellement détruire un commit sur lequel d'autres développeurs basent leur travail.

Lorsque vous réécrivez l'historique, assurez-vous toujours que vous le faites de manière sûre et que vos collègues sont conscients de ce que vous faites.

### Changer plusieurs commits

Supposons maintenant que nous ayons des commits plus loin dans notre historique que nous voulons modifier. C'est là que la belle commande `rebase` entre en jeu !

`rebase -i` est une commande qui nous permet de nous arrêter de manière interactive après chaque commit que nous essayons de modifier, puis d'apporter les modifications que nous souhaitons.

Nous devons dire à cette commande quel est le dernier commit que nous voulons éditer. Par exemple, git `rebase -i HEAD~2` nous permet de modifier les deux derniers commits. Voyons à quoi cela ressemble en action, allez-y et tapez :

```sh
  git log
  git rebase -i HEAD~2
```

Vous devriez remarquer que lors du rebasage, les commits sont répertoriés dans l'ordre inverse par rapport à la façon dont nous les voyons lorsque nous utilisons `log`.

Prenons une minute pour parcourir toutes les options que l'outil interactif nous offre. Examinons maintenant les messages de validation en haut de l'outil.

Si nous voulions modifier l'un de ces commits, nous changerions le mot pick pour qu'il soit édité pour le commit approprié.

Si nous voulions supprimer un commit, nous le retirerions simplement de la liste, et si nous voulions changer leur ordre, nous changerions leur position dans la liste. Voyons à quoi ressemble une modification !

```
  edit eacf39d Create send file
  pick 92ad0af Create third file and create fourth file
```

Cela nous permettrait de modifier la faute de frappe dans le commit `Create send file` pour `Create second file`.

- Effectuer des modifications similaires dans l'outil de rebase interactif, mais ne pas copier/coller le code ci-dessus car cela ne fonctionnera pas.

-Enregistrer et quitter l'éditeur, ce qui nous permettra d'éditer le commit avec les instructions suivantes :

```
  You can amend the commit now, with
    git commit --amend

  Once you're satisfied with your changes, run
    git rebase --continue
```

Modifions donc notre commit en tapant `git commit --amend`, en corrigeant la faute de frappe dans le titre, puis en terminant le rebase en tapant `git rebase --continue`. C'est tout !

Jetons un œil à votre travail en tapant `git log` et en consultant l'historique modifié. Cela semble simple, mais c'est un outil très dangereux s'il est mal utilisé, alors soyez prudent.

Plus important encore, **rappelez-vous que si vous devez rebaser des commits dans un repository partagé, assurez-vous que vous le faites pour une très bonne raison et que vos collèguessoient au courant**.

### Squasher des commits

Utiliser `squash` pour nos commits est un moyen très pratique de garder notre historique Git bien rangé. Il est important de savoir `squash` (fusionner des commits), car ce processus peut être la norme dans certaines équipes de développement.

Le squashing permet de nettoyer de faire comprendre plus facilement aux autres l'historique de notre projet. Ce qui se passe souvent lorsqu'une fonctionnalité est mergée, c'est que nous nous retrouvons avec des historiques complexes de toutes les modifications qu'une branche de fonctionnalité a apportées à une branche principale.

Ces commits sont importants pendant le développement d'une fonctionnalité, mais ne sont pas vraiment nécessaires lorsque nous parcourons l'historique complet de notre branche principale.

Disons que nous voulons `squash` le deuxième commit dans le premier commit de la liste, qui est `Create first file`.

- Commençons par rebaser jusqu'à notre commit racine en tapant `git rebase -i --root`.

- Maintenant, ce que nous allons faire est de choisir ce premier commit, comme celui dans lequel le deuxième commit est `squashed` :

```sh
  pick e30ff48 Create first file
  squash 92aa6f3 Create second file
  pick 05e5413 Create third file and create fourth file
```

- Renommer le commit en `Create first and second file`, puis terminer le rebase. C'est tout!

- Exécuter `git log` et voyons comment les deux premiers commits ont été fusionnées ensemble.

### Séparer un commit

Avant de plonger dans les Remotes, nous allons jeter un œil à une commande Git pratique appelée `reset`.

Jetons un coup d'œil au commit `Create third file and create fourth file`. Pour le moment, nous utilisons des fichiers vierges pour plus de commodité, mais disons que ces fichiers contiennent des fonctionnalités et que le commit en décrit trop à la fois. Dans ce cas, nous pourrions le diviser en deux commits plus petits en utilisant, encore une fois, l'outil de `rebase` interactif.

Nous ouvrons l'outil comme la dernière fois, changeons `pick` pour `edit` le commit que nous allons diviser.

Maintenant, cependant, ce que nous allons faire est de lancer `git reset HEAD^`, qui réinitialise le commit à celui juste avant HEAD.

Cela nous permet d'ajouter les fichiers, de les ajouter et de les valider individuellement. Tous ensemble, cela ressemblerait à ceci :

```sh
  git reset HEAD^
  git add test3.md && git commit -m 'Create third file'
  git add test4.md && git commit -m 'Create fourth file'
```

Commençons par regarder d'un peu plus près ce qui s'est passé ici. Lorsque nous avons exécuté `git reset`, nous avons réinitialisé la branche actuelle en pointant HEAD sur le commit juste avant.

Dans le même temps, `git reset` a également mis à jour l'index (la zone de staging) avec le contenu de l'endroit où HEAD est maintenant pointé. Ainsi, notre zone de staging a également été réinitialisée à ce qu'elle était lors de la validation précédente - ce qui est formidable - car cela nous a permis d'ajouter et de valider les deux fichiers séparément.

Supposons maintenant que nous voulions nous déplacer là où HEAD pointe mais que nous ne voulions pas toucher la stagign area. Si nous voulons laisser l'index seul, nous pouvons utiliser `git reset --soft`. Cela n'effectuerait que la première partie de `git reset` où le HEAD est déplacé pour pointer ailleurs.

La dernière partie de la réinitialisation que nous voulons aborder est `git reset --hard`. Ce que cela fait, c'est qu'il exécute toutes les étapes de `git reset`, en déplaçant le HEAD et en mettant à jour l'index, mais il met également à jour le répertoire de travail dans l'éditeur.

Ceci est important à noter car cela peut être dangereux car cela peut potentiellement détruire des données. Un hard reset écrase les fichiers dans le répertoire de travail pour le faire ressembler exactement à la zone de staging où HEAD finit par pointer.

De la même manière que `git commit --amend`, un hard reset est une commande destructive qui écrase l'historique. Cela ne signifie pas que nous devons complètement l'éviter si nous travaillons avec des repo partagés en équipe avec d'autres développeurs. Nous devons cependant nous assurer de savoir exactement pourquoi nous l'utilisons et que nos collègues savent également comment et pourquoi nous l'utilisons.

### Travailler avec des repos distants

Jusqu'à présent, nous avons travaillé avec des repos distants chaque fois que nous avons push ou pull de notre propre repo GitHub tout en travaillant sur les différents fichiers du programme.

#### git push --force

Disons que nous ne travaillons plus sur un projet tout seul, mais avec quelqu'un d'autre. Nous souhaitons pousser une branche sur laquelle nous avons apporté des modifications vers un repo distant. Normalement, Git ne nous laissera pousser nos modifications que sinous avons déjà mis à jour notre branche locale avec les derniers commits de cette remote (repo distant).

Si nous n'avons pas mis à jour notre branche locale et que nous essayons de `git push` un commit qui créerait un conflit sur le repo distant, nous obtiendrons un message d'erreur. C'est vraiment une bonne chose ! Il s'agit d'un mécanisme de sécurité pour vous empêcher d'écraser les commits créés par les personnes avec lesquelles nous travaillons, ce qui pourrait être désastreux. Nous obtenons l'erreur car notre historique est obsolète.

Nous pouvons effectuer une brève recherche et trouver la commande `git push --force`. Cette commande écrase le repo distant avec notre propre historique local. Alors que se passerait-il si nous l'utilisions tout en travaillant avec d'autres ? Eh bien, voyons ce qui se passerait si nous travaillions avec nous-mêmes.

- Taper les commandes suivantes dans le terminal et lorsque l'outil de rebase interactif apparaît, supprimer notre validation en sélectionnant `drop` pour `Create fourth file` :

```sh
  git push origin main
  git rebase -i --root
  git push --force
  git log
```

Cheh, c'est intéressant, nous ne voyons plus notre quatrième fichier sur notre repo local.

Vérifions notre dépôt GitHub, est-ce que notre fichier test4.md est là ?

Non! Nous venons de le détruire, ce qui dans ce scénario est le danger - nous pourrions potentiellement détruire le travail de ceux avec qui nous collaborons ! `git push --force` est une commande **très dangereuse, et elle doit être utilisée avec prudence lors de la collaboration avec d'autres**.

Au lieu de cela, nous pouvons corriger notre erreur d'historique obsolète en mettant à jour vonotretre historique local à l'aide de `fetch`, `merge`, puis en essayant de `push` à nouveau.

`git pull` fait une `git fetch` et une `git merge` en une seule commande, tandis que `git fetch` ne fusionne pas les modifications locales avec les modifications distantes. Il télécharge simplement les données du repo distant sans les fusionner avec le repo local.

Considérons un scénario différent :

```sh
  touch test4.md
  git add test4.md && git commit -m "Create fifth file"
  git push origin main
  git log
```

Nous regardons notre message de commit et réalisons que nous avons fait une erreur. Nous voulons annuler ce commit et sommes à nouveau tentés de forcer le push. Mais attendez, rappelez-vous, c'est une commande **très dangereuse**.

Si nous envisageons de l'utiliser, vérifiez toujours s'il est approprié et si nous pouvons utiliser une commande plus sûre à la place. Si nous collaborons avec d'autres et que nous voulons annuler un commit que nous venons de faire, nous pouvons à la place utiliser `git revert` !

```sh
  git revert HEAD
  git push origin main
```

Souvenons-nous quand nous travaillions avec HEAD, c'est-à-dire le commit actuel que nous visualisons, lors du rebasage. Ce que cela ferait, c'est que cela annulerait les modifications apportées à HEAD ! Ensuite, nous pousserions notre nouveau commit vers la branche sur laquelle nous travaillons, qui dans cet exemple est main même si normalement notre travail serait très probablement sur une branche de fonctionnalité.

Alors maintenant que nous avons appris les différents dangers de `git push --force`, vous vous demandez probablement pourquoi il existe et quand l'utiliser. Un scénario très courant dans lequel les développeurs utilisent `git push --force` est pour mettre à jour les pull requests.

La conclusion de cette section est que l'option `--force` ne doit être utilisée que lorsque vous êtes certain qu'elle est appropriée. Il existe également des scénarios moins courants, par exemple lorsque des informations sensibles sont téléchargées accidentellement dans un repo et que vous souhaitez en supprimer toutes les occurrences.

Il convient de mentionner particulièrement `git push --force-with-lease`, une commande qui, dans certaines entreprises, est l'option par défaut.

La raison en est qu'il s'agit d'une sécurité intégrée ! Elle vérifie si la branche vers laquelle vous essayez de pousser a été mise à jour et vous envoie une erreur si c'est le cas. Cela vous donne la possibilité, comme mentionné précédemment, de récupérer le travail et de mettre à jour votre repo local.

## Dangers et bonnes pratiques

Passons en revue les dangers que nous avons abordés jusqu'à présent Si vous regardez en arrière dans cette leçon, vous verrez un fil conducteur. `amend` (modifier), `rebase` (rebaser), `reset` (réinitialiser), `push --force` sont tous particulièrement dangereux lorsque vous collaborez avec d'autres. Ces commandes peuvent détruire le travail créé par vos collègues. Alors garde cela en tête. Lorsque vous essayez de réécrire l'historique, vérifiez toujours les dangers de la commande particulière que vous utilisez et suivez ces meilleures pratiques pour les commandes que nous avons couvertes :

1. Si vous travaillez sur un projet d'équipe, assurez-vous que la réécriture de l'historique est sûre et que les autres savent que vous le faites.

2. Idéalement, n'utilisez ces commandes que sur les branches avec lesquelles vous travaillez vous-même.

3. Utiliser le flag `-f` pour forcer quelque chose devrait vous effrayer, et vous feriez mieux d'avoir une très bonne raison de l'utiliser.

4. Ne poussez pas après chaque commit. La modification de l'historique publié doit être évitée dans la mesure du possible.

5. Concernant les commandes spécifiques que nous avons couvertes :

   1. Pour `git amend`, ne modifiez jamais les commits qui ont été poussés vers des repos distants.

   2. Pour `git rebase`, ne rebasez jamais un repo sur lequel d'autres peuvent travailler.

   3. Pour `git reset`, ne réinitialisez jamais les commits qui ont été poussés vers des repos distants.

   4. Pour `git push --force`, utilisez-le uniquement lorsque cela est approprié, utilisez-le avec prudence et, de préférence, utilisez par défaut `git push --force-with-lease`.
