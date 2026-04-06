# Commandes utiles
On va d'abord revoir les commandes que nous avons utilisées au cours des TP, avec certaines options utiles pour notre usage.
## Commandes de base
> Si vous n'avez jamais vu la syntaxe :  
> les paramètres entre `[A]` sont optionnels,  
> `B | C` indique plusieurs choix possibles,  
> et `<C>` indique une valeur que vous devrez remplir  
> 
> Par exemple `git init [-b <branch_name> | --initial-branch=<branch_name>]` vous indique que la commande `git init` peut prendre un paramètre optionnel, que ce paramètre peut s'écrire sous la forme `-b <branch_name>` ou `--initial-branch=<branch_name>`, et que vous devrez remplacer `<branch_name>` par une valeur, ici le nom de la branche.

* `git init [--bare] [-b <branch_name> | --initial-branch=<branch_name>]`  
  `--bare` est utile pour créer un dépôt dans lequel on ne va pas travailler. Il n’y aura pas d’espace de travail, juste les fichiers habituellement présents dans le sous-répertoire .git
  `-b` est utile si on veut changer le nom de la branche principale.
* `git add <pattern>`  
  Ajoute les fichiers sélectionnés par `<pattern>` à l'index.
* `git commit [-a | --all] [-m <message> | --message=<message>]`  
  Crée un commit qui va contenir tous les changements ajoutés à l'index.
  `-a` vous permet d'automatiquement ajouter tous les changements à des fichiers déjà connus, ce qui évite d'avoir à utiliser `git add`; notez bien que cette option va donc ignorer les fichiers nouvellement créés.  
  `-m` peut être utilisé plusieurs fois, chaque -m représentant un paragraphe ; auquel cas le premier message sera celui utilisé dans les versions courtes de vos logs.
* `git status`  
  Affiche un état des lieux : dans quelle branche vous trouvez-vous, est-ce que vous êtes en retard ou en avance par rapport à la branche distante, est-ce que l'espace de travail contient des fichiers modifiés, nouvellement créés, effacés ; quels changements ont été ajoutés à l'index.
* `git log [--oneline] [--stat] `  
  Log sert à afficher l'historique des commits de la branche en cours.  
  L'affichage est hautement configurable, et permet toutes sortes d'opérations, parmis lesquelles :  
  `--oneline` permet d'afficher un résumé en une ligne du commit  
  `--stat` rajoute un bloc sur les fichiers modifiés dans le commit  

## Commandes de dépôt distant

* `git clone <repository> [<directory>]`  
  vous permet de clôner un dépôt situé à l'adresse `<repository>` indiquée, et si vous indiquez un `<directory>` il le placera dans ce répertoire.
* `git push [-u | --set-upstream] <remote> <branch>`  
  Envoie les commits de la branche en cours vers un dépôt distant `<remote>`. Vous pouvez préciser le nom de la branche distante également.  
  En pratique, `<remote>` s'appelle souvent `origin` et on préfère utiliser le même nom de branche partout.  
  `-u` est utilisé lors du premier `push` vers un dépôt distant, si vous ne l'avez pas clôné, et va automatiquement configurer le lien entre la branche actuelle et celle distante, pour que les push et pull suivants n'aient pas besoin d'utiliser des paramètres. Cette opération doit être faite pour chaque nouvelle branche.
* `git pull`   
  Met à jour la branche locale en récupèrant tout nouveau commit sur la branche en cours présents sur le dépôt distant, et en les fusionnant avec la branche locale. 
* `git fetch`  
  Va chercher les modifications sur le dépôt distant, comme la commande `git pull`, mais ne les fusionne pas automatiquement !  
  À la place, git crée des branches locales nommées `<remote>/<branch>` et vous laisse ensuite le soin de les fusionner vous-mêmes à votre convenance.
* `git remote [-v | --verbose]`  
  Liste les dépôts distants configurés
  * `git remote add <name> <source>`  
    Ajoute un dépôt situé à l'endroit indiqué par `<source>`, nommé `<name>`.  
    `<source>` serait l'URL que vous auriez pu utiliser dans `git clone`
  * `git remote remove <name>`  
    Vous permet d'effacer un dépôt distant, typiquement si vous avez faire une erreur lorsque vous l'avez ajouté manuellement.
  
## Commande de branches

* `git branch [(-r | --remotes) | (-a | --all)]`  
  Sans option, va donner la liste des branches locales.  
  Si on précise `-r`, va donner les branches distantes, `-a` donnera toutes les branches.  
  * `git branch (-d | -D) <branch>`  
    Efface la branche <branch>. Si vous utilisez `-d`, la branche en question doit avoir été fusionnée avec sa branche d'amont (*upstream*). Cela vous évite d'effacer votre travail !  
    `-D` efface la branche sans autre forme de procès.
  * `git branch (-m | -M) [<oldbranch>] <newbranch>`  
    Vous permet de renommer une branche. Utile en cas d'erreur, ou bien si votre dépôt distant utilise `main` mais que vous avez `master` en local.
  * `git branch <name>`  
    Crée la branche <name>. 
    > Attention, la commande ne bascule pas sur la branche nouvellement créée !  
* `git switch <branch>`  
  Change le contenu de l'espace de travail pour refléter l'état de la branche cible.  
  > Attention, git ne peut changer que ce qu'il connait.  
  > Les changements non commités peuvent empêcher la bascule. Les fichiers non-suivis ne sont pas touchés, mais peuvent créer des problèmes.  
* `git stash`  
  Enregistre les changements en cours dans une "cache" (*stash* en anglais) puis remet l'espace de travail au propre (comme un `git reset --hard HEAD`), ce qui a pour effet de permettre de basculer de branche, sans nécessairement avoir fait un commit.  
  NB : les fichiers non-suivis ne sont pas affectés, puisqu'ils ne bloquent généralement pas la bascule de branche.
  * `git stash --list`  
    Vous permet d'afficher la liste des caches existantes, si vous avez oubliez où vous en étiez.  
  * `git stash pop`  
    Opération vous permettant de récupérer le contenu mis de côté via `git stash`. À lancer dans la branche où vous aviez mis vos changements de côté, normalement.  
    Après exécution, le contenu de la cache est effacé.  
* `git merge <branch>`  
  Prend la branche `<branch>` et essaye de la fusionner avec la branche actuelle.  
  En cas de conflit, l'opération est interrompue pour permettre de modifier les fichiers en conflit.  
  * `git merge --abort`  
    En cas de conflit lors d'un merge, vous permet d'annuler l'opération et de revenir à votre espace de travail tel qu'il était au dernier commit. 

## Commandes avancées

Nous n'avons pas explicitement vu ces commandes, mais j'ai pu les utiliser pour débloquer certains d'entre vous lorsqu'ils avaient fait une erreur de manipulation, pour rétablir leur dépôt dans un état "propre".  
Elles sont très puissantes, mais également très dangereuses.  
Elles sont le sujet des cours que j'ai donné à vos camarades de deuxième année.  
* `git restore <file>`  
  Vous permet de restaurer le fichier `<file>` dans son dernier état sauvegardé.  
  Les modifications sont définitivement perdues.
  * `git restore --staged <file>`  
  La commande à utiliser si vous avez ajouté les changements fait à un fichier à l'index, mais que vous ne vouliez pas.  
* `git reset [--hard] <commit>`
  Vous permet de revenir à un point donné dans votre historique, comme si vous n'aviez pas fait les commits suivants. L'option `--hard` forcera un état propre, c'est-à-dire qu'aucun changement en cours ne sera conservé.
  `<commit>` représente le hash du commit auquel vous voulez revenir. Vous pouvez également utiliser un tag.  
> Si vous avez lancé cette commande par erreur, vous pouvez faire un `git pull` pour recupérer tous vos commits disparus depuis votre dépôt distant.
* `git rebase`  
  Une commande très puissante de manipulation de l'historique. Elle permet de "déplacer" des commits, des branches entières, comme si leurs changements avaient été effectués en partant d'un autre commit.  
  Très utilisée dans les méthodes de travail actuelles, pour mettre à jour une branche sans faire un merge, qui est plus "sâle".  
  La syntaxe un peu particulière en fait une source de catastrophes intarissable... 
* `git reflog`  
  Si `git log` affiche l'historique de votre branche, cette commande affiche *votre* historique. C'est-à-dire toutes les opérations que vous avez effectué, comme basculer de branche, fusionner, faire un rebase, etc. Cela vous permet de récupérer par exemple des commits autrement "disparus" et revenir en arrière, avant *la catastrophe*.

# Branches
On a d'abord travaillé avec la branche `master` (nom par défaut donné par git),mais en réalité, dans les pratiques modernes de développement, on ne touche que rarement à cette branche principale.  

Elle représente un état parfait de notre code. Idéalement, chaque commit dans la branche principale devrait être une version qui compile, qui passe tous les tests, qui a été validée par les utilisateurs, qui tourne sur toutes les machines ciblées, etc.  

Évidemment, avec de telles contraintes, on ne pourrait donc presque jamais faire de commits intermédiaires, et l'intérêt d'utiliser un système de gestion de versions devient presque caduc.  
C'est là qu'intervienne les branches.  
