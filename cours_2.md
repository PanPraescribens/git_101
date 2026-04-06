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

Évidemment, avec de telles contraintes, on ne pourrait donc presque jamais faire de commits intermédiaires, et l'intérêt d'utiliser un système de gestion de versions deviendrait caduc.  
C'est là qu'intervienne les branches.  

## Utilité
La première utilité des branches est de pouvoir travailler sans "salir" la branche principale, pour que celle-ci soit une source sûre de code stable, prêt à être utilisé.  
Par opposition donc, les branches vont contenir le travail intermédiaire entre deux commit "parfaits".  
Une fois cette distinction comprise, la méthodologie n'est qu'une question d'interprétation de ce concept à plus ou moins grande échelle, pour accomoder l'organisation de l'équipe utilisant git.
Un développeur solo peut immédiatement implémenter et bénéficier de l'approche "github flow", alors que l'approche "git flow", beaucoup plus complexe, existe cependant pour répondre à aux contraintes des équipes développant Linux, par exemple.  

## Définition
Techniquement parlant, une branche n'est rien d'autre qu'un pointeur vers un commit !  
C'est un fichier dans .git/refs/heads portant le nom de la branche (d'où les limitations sur le nom des branches) et contenant le hash du dernier commit dans cette branche (d'où le nom du répertoires *heads*).

Il faut bien prendre garde que si on pense surtout à une branche dans son sens "arboricole", c'est-à-dire la partie qui a divergé de la branche parente, il faut bien se souvenir que git considère la branche dans son intégralité, jusqu'à l'origine du dépôt.
```
O <- A <- B <- E <- F <= master
           \
            C <- D <= dev
```
Ici dev et master sont deux branches, et typiquement quand on pense à dev on pense à B <- C <- D.  
Mais en réalité la branche dev commence à O, comme toute autre branche normalement constituée.  

Dans certains contexte, particulièrement l'utilisation de rebase, cette distinction est critique.  

## Divergences et fusions
La décision et l'ampleur des changements emmenés dans une branche est une question de méthodologie.  
Typiquement si on travaille en équipe, chaque branche simultanée va éviter de toucher des fichiers qu'une autre branche modifie également. 
Parfois c'est assez simple (une équipe travaillant sur une fonctionalité neuve ne risque pas grand chose, par exemple), parfois cela peut être compliqué (modifier une partie centrale du projet qui est utilisée par toutes les autres fonctionalités).  
Dans tous les cas, c'est une question d'organisation et de communication entre développeurs / équipes.  

Idéalement, et quelle que soit la méthode, on crée une branche le temps d'avoir du code acceptable pour refusionner dans la branche parente.  
Plus une branche fille existe longtemps, plus elle diverge de sa branche parente, plus elle a de chance d'avoir des conflits lorsqu'on essaye de la fusionner.

## Basculer de branche
Il est assez rare de pouvoir travailler continuellement sur une branche sans interruption, et donc git permet de basculer d'une branche à une autre.  
Cela permet par exemple de travailler sur deux fonctionalités bien distinctes, ou d'aller faire une correction sur une branche qui en a besoin.  
> Il est intéressant de noter ici la confusion possible entre la notion de branche et la notion de répertoire dans notre espace de travail.
> Idéalement, on veut que nos branches travaillent effectivement sur des zones bien distinctes de l'espace de travail pour minimiser les conflits,
> mais il n'en reste pas moins qu'une branche représente une version de l'ensemble d'un projet. Il est donc important de bien être conscient de la branche dans laquelle on travaille, avant de faire des modifications, pour éviter les problèmes.

### Mettre des modifications de côté pour basculer
Quand on doit basculer de branche alors qu'on a du travail en cours, on peut ne pas vouloir faire un vrai commit. Peu importe la raison, git nous donne un outil pour mettre notre travail dans un "faux" commit : la cache (*stash* en anglais).
Avec `git stash`, git prend toutes les modifications sur des fichiers suivis et sur l'index, et les mets dans un commit spécial, puis les enlève de votre espace de travail en cours, pour que vous puissiez basculer sur une autre branche proprement.  
> Un nouveau fichier non ajouté à l'index n'est pas mis de côté puisqu'il n'existe pas aux yeux de git !  
Une fois que vous avez fini et que vous pouvez revenir sur votre branche pour continuer, `git stash pop` remettra votre espace de travail là où vous en étiez, et vous pourrez continuer comme si de rien était. 
> Le stash est effacé une fois que vous utilisez `pop`.  

## Fusionner avec une branche mère

C'est le but ultime de toute branche. Arriver à un niveau de maturité qui indique une promotion à la branche parente, jusqu'à finir éventuellement (suivant les méthodologies) en branche principale.  

Idéalement on veut fusionner avec le parent en ayant le moins de conflits à résoudre. Donc généralement on a fait des mises à jour de sa branche fille (voir paragraphe [plus loin](#fusionner-avec-une-branche-fille))

Mais il arrive qu'on ait à résoudre malgré tout des conflits.

## Fusion Fast-Forward .. ou pas

Il peut arrive qu'on se retrouve dans la situation idéale où la branche parent n'a eu aucun commit depuis qu'on a commencé la branche.  
Cette situation permet à git de savoir qu'aucun conflit ne peut avoir lieu, et que les commits de la branche sont descendants directs.  
Git peut donc immédiatement déplacer le pointeur de branche sur le dernier commit et ainsi fusionner les deux branches **sans avoir à créer un commit de fusion**.  

Ce dernier point est parfois vu comme négatif et il existe donc des options pour empêcher cela !  
Par exemple `git merge --no-ff dev` indique à git de fusionner la branche dev avec la branche en cours, et si la situation permettait de faire un fast-forward, de quand même créer un commit de fusion.

## Résolution de conflit
Dans le cas où une fusion présente un conflit, c'est généralement qu'un fichier est présent dans deux versions différentes et que git ne sait pas forcément comment résoudre la situation. Il va donc interrompre la fusion (en créant un commit intermédiaire) et déclarer un conflit à résoudre manuellement.  
Vous remarquerez que le prompt change en indiquant `MERGING`.

Par exemple on peut avoir un fichier `style.css` qui contient une ligne indiquant la couleur bleu. Mais dans une version on a utilisé `color: blue` et dans l'autre `color: #0000ff`. Dans les deux cas on a la même information, mais git compare les caractères et ils sont différents. Donc conflit.  

De bonnes pratiques de travail auraient évité le souci (en décidant quelle était la façon normale de renseigner une couleur) et ce conflit est l'occasion pour l'équipe d'aborder le sujet (s'ils ont le temps). Qui a raison entre « blue » et « #0000ff » ?  
« blue » est clair et lisible, mais ambigü : quel bleu, exactement ? L’héxadécimal est précis mais dur à lire pour le novice.
Une discussion a lieu et tout le monde s’accorde pour dire qu'au final, la notation `rgb()` est plus claire et donc une troisième version est créée, plutôt que de se forcer à choisir entre l’un ou l’autre.  

La discussion a eu le bénéfice de clarifier un problème et donner une référence pour les cas futurs où la question se poserait !  
Et l'exemple vous montrer aussi qu'un conflit n'est pas qu'un choix manichéen, il faut mesurer les impacts et parfois choisir une troisième voie qui ne se serait pas révelée sans les visions divergentes des participants.  

...et d'autres fois, git peut résoudre le conflit tout seul et vous éviter tout cela.

Et d'autres fois encore, on n'a juste pas la force, ou bien s'était une erreur, et on peut sortir de la fusion et revenir à l'étape précédente en utilisant `git merge --abort`

## Fusionner avec une branche fille

Cette fusion là est plus utilisée dans le travail au long cours, avec de multiples branches actives en même temps, ce qui va forcément entrainer des mises à jour vers une branche parent commune qui finira par affecter les autres.  

```
     -- C <--      hotfix
    /        \
<- V0.1 <- v0.1.1 <------ v0.2   main
    \          \          /
     B <------- M <- D <-/      feature/a
```
Ici un exemple simple : une modification en urgence de la branche main a été effectuée (via une branche hotfix, malgré tout).  
Ce hotfix a eu lieu pendant que la feature/a était en cours de développement, et donc il y a risque de conflit lors du merge à venir.  
En pratique, on préfère faire “remonter” les changements du parent vers l’enfant, pour résoudre les éventuels conflits le plus tôt possible.  

Si on utilise un merge classique, on va créer un commit de fusion M dans lequel on résoudra (ou pas) les conflits et à partir duquel on pourra continuer le développement pour pouvoir ensuite fusionner la feature avec le minimum de difficultés.

```
     -- C <--      hotfix
    /        \
<- V0.1 <- v0.1.1 <--------- v0.2   main
               \            /
                B' <- D' <-/      feature/a
```
La solution la plus commune ces temps-ci est le rebase, qui au prix d’une supercherie chronologique, va garder une certaine lisibilité au niveau du graphe des branches, en faisant comme si le développement de la branche avait en fait eu lieu depuis la dernière version de main (ici v0.1.1).  
Ici on lance donc `git rebase main feature/a` qui indique qu'on veut reconstruire la branche `feature/a` en la faisant repartir de `main`.  
Git est assez intelligent pour deviner que vous parlez de reconstruire à partir du *dernier* commit de `main` (`v0.1.1`), puisque `feature/a` était déjà issue de 'main' (`v0.1`).   
La manœuvre n’empêche pas les conflits, elle déplace simplement le lieu de leur résolution.  
Le commit B’ est une copie du commit B, mais un objet distinct dans le dépôt et peut être modifié avant sa création dans une phase de résolution de conflit.  
B' a donc bien fusionné les changements apportés dans la v0.1.1 dans la branche de fonctionalité, pour continuer le développement sur des bases tout aussi saines que dans le cas d'une fusion classique.  
Le commit B disparait du log de feature/a.