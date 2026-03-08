Ce document va vous permettre de refaire les manipulations que nous allons faire lors du TP.  

# Télécharger un dépôt distant
A priori si vous lisez ceci, vous venez d'exécuter la commande `git clone https://github.com/PanPraescribens/TP_git_101.git` qui aura donc créé un répertoire `TP_git_101`. Vous êtes dans le répertoire en train de lire ce fichier `instructions.md`  

Une option utile pour nous aujourd'hui est de spécifier le nom du répertoire qui va contenir le code téléchargé :  
`git clone https://github.com/PanPraescribens/TP_git_101.git TP_git_101_instructions` va créer un répertoire appelé `TP_git_101_instructions`. Nous ne toucherons pas au contenu de ce répertoire.

# Configurer votre git
`git config --global --list` va afficher la configuration en cours pour votre utilisateur sur la machine. Cette configuration va s'appliquer à tous les dépôts que vous utilisez sur cette machine.  

## Configuration de base
Pour travailler nous avons besoin de configurer les options d'utilisateur `user.name` et `user.email` qui sont utilisées pour signer vos *commits*.
Si leur valeur n'est pas déjà renseignée, vous pouvez les écrire avec les commandes suivantes (pas besoin d'être dans un répertoire git) :  
```bash
git config --global user.name toto
git config --global user.email toto@school.org 
```
> NB : l'option `--global` indique à git que vous voulez utiliser ces valeurs par défaut pour tout dépôt de code que vous allez consulter sur cette machine.  

# Créer un dépôt git
Placez vous dans un répertoire *qui n'est pas déjà un dépôt git*.
Si vous lisez ces instructions dans le répertoire `TP_git_101_instructions`, il va falloir que vous remontiez d'un niveau, et que vous créiez un répertoire dans lequel nous allons pouvoir travailler.  
Appelons le `TP_git_101`. Nous pouvons créer un répertoire avec la commande bash `mkdir TP_git_101`.  

Avant de continuer nous devons donc avoir la hiérarchie suivante :  
```
/TP_git_101_instructions
    instructions.md
    .git
/TP_git_101
```

Assurez-vous que vous êtes bien dans le répertoire `TP_git_101` avant de continuer.  
Créons un premier sous-répertoire nommé `depot_01`, puis allons dedans.  
Tapez la commande `git status` qui va vous retournez l'erreur
```
fatal: not a git repository (or any of the parent directories): .git
```
Ce qui est normal.  
Maintenant nous allons déclarer ce répertoire comme dépôt git avec la commande `git init`.  

Si vous regardez maintenant le contenu du répertoire avec `ls` vous devriez constater que votre répertoire est toujours vide. Mais si vous tapez `git status` vous aurez le message 
```
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```
Ceci vous indique que vous êtes bien dans un dépôt git. Avez git bash, vous devez aussi constater que votre prompt a changé avec `(master)` qui apparait en bleu horizon en fin de ligne. Ceci est le nom de la branche en cours (on ne va pas utilisez les branches pour l'instant, mais c'est un bon moyen de savoir que vous êtes à l'intérieur d'un dépôt git).  

## Qu'est-ce qui a changé ? 
Comment git sait-il que le répertoire est un dépôt de code ?  
Et bien en fait, si vous tapez `ls -la` vous allez voir apparaitre le répertoire `.git` qui est le dépôt git à proprement parler.  

> **N'allez pas toucher le contenu du répertoire .git !**  
> C'est lui qui contient *tout* votre travail et toutes les versions que vous aurez sauvegardées, et mêmes celles que vous allez abandonner.  

Nous n'allons pas étudier le contenu de ce répertoire en détail aujourd'hui,  
mais il est important de comprendre que c'est lui qui permet à git de faire son travail. Toutes les commandes que vous allez exécuter dans ce dépôt interagissent avec le contenu de ce répertoire.  

# Travailler dans votre dépôt
En utilisant la commande `git init`, vous indiquez que tout le contenu de ce répertoire est à surveiller, et potentiellement à sauvegarder.  
Mais git **ne fait rien sans instruction explicite**.  
Aux travers des différentes commandes existantes, vous allez indiquer à git quels sont les fichiers dont vous désirez suivre l'évolution, vous indiquerez quand vous souhaitez les sauvegarder, vous pourrez choisir d'en sauvegarder un que vous venez de modifier mais pas un autre pour l'instant, etc.  
Contrairement à de nombreux outils modernes qui font tout le travail dans votre dos, git ne fait absolument rien tout seul, et demande une certaine rigueur dans votre méthode de travail.  
En retour vous aurez rarement de mauvaises surprises.  

## Créer un fichier
Créons un fichier et travaillons dedans :  
```bash
echo "Hello World!" > hello.txt
```
Cette commande vient de créer un fichier dans votre répertoire courant, dont le contenu est "Hello World!".  
Si vous tapez maintenant la commande `git status`, git devrait vous indiquer :
```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```
La partie "Untracked files" nous indique notre fichier nouvellement créé, et git nous informe qu'il n'est pas "suivi". Il n'a jamais été enregistré dans le dépôt.  
Remarquez comme git nous indique également la commande à exécuter pour faire suivre un fichier.  

## Faire suivre un fichier
Comme dit précédemment, si on ne dit rien à git, il ne fait rien. Donc si on veut qu'il suive l'évolution de notre fichier `hello.txt` il va d'abord falloir informer git que c'est un fichier interessant (nous verrons plus tard que git peut également ignorer des fichiers, à notre demande, de la même manière qu'il ignore tout le contenu du répertoire `.git`)

Ajoutons le fichier hello.txt à l'index.
```bash
git add hello.txt
```
Si vous voyez le message 
```
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
```
soyez les bienvenus dans le monde merveilleux de l'encodage de texte ! Vous pouvez ignorer le problème pour le moment.  
> Mais en gros on a créé un fichier texte avez git-bash, qui contient un retour à la ligne de type UNIX (LF), mais nous travaillons sur une machine Windows qui utilise des retours à la ligne (CRLF).  
> Git a une option de configuration `core.autocrlf` qui va automatiquement convertir un type dans l'autre, et ce message nous en informe, puisque git ne fait rien sans nous prévenir avant !  

Après avoir ajouté le fichier `hello.txt` à l'index nous pouvons maintenant voir que `git status` nous renvoie un message différent :
```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt
```

Nous n'avons toujours pas de *commit*, mais nous avons un fichier prêt à être sauvegardé.  
Ou pour être plus précis, git a détecté un changement sur le fichier suivi hello.txt, le changement étant la création de ce fichier.  
Notre commit à venir contiendra donc le contenu de ce fichier.  
> Encore une fois, notez comme git vous donne la commande si vous souhaitez revenir en arrière :
> `git rm --cached hello.txt` vous permet de dire à git que vous avez changé d'avis et que vous ne souhaitez pas que les changements sur le fichier hello.txt soient enregistrés dans le commit à venir.  

À ce stade nous avons 
* créé un fichier
* dit à git que nous aimerions enregistrer les changements sur ce fichier  

Nous n'avons pas 
* sauvegardé le fichier hello.txt dans l'historique git

## Ajouter des changements à l'historique
Si `git add` nous permet de sélectionner ce que nous voulons sauvegarder, il faut maintenant indiquer à git que nous voulons créer une sauvegarde : un ***commit***.  
Un commit est un point de sauvegarde, qui contient tous les changements que nous avons ajoutés manuellement à ce que git appelle ***l'index***.
Vous pouvez voir le contenu de l'index lorsque vous exécutez la commande `git status` dans la partie ***"Changes to be committed:"***

```
git commit
```
Si vous exécutez la commande ainsi, sans options, git va ouvrir votre editeur de texte par défaut.  
> `git config core.editor` vous montrera l'éditeur configuré  
 
git vous donne des instructions très explicites, donc lisez-les pour bien comprendre : 
```

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#	new file:   hello.txt
#
```
Vous pouvez ne rien mettre, et fermer l'éditeur, et git annulera le commit.  
Le format de vos messages est aussi assez important. Ici nous allons utiliser une option intéressante et mettre le message suivant. Nous allons mettre un résumé de notre commit en première ligne, sauter une ligne, puis ajouter un petit paragraphe.  

```
Ajoute hello.txt

La première ligne est un résumé du commit, et après un saut de ligne,
vous pouvez également rédiger un message un peu plus détaillé qui
n'apparaitra pas dans la vue résumée de l'historique.
```

Lorsque vous avez fini de taper votre texte, sauvegardez le fichier et fermer l'éditeur de texte. Au moment où vous fermez l'éditeur, git reprend la main et créé le commit.  

## Voir l'historique
Avant de continuer, consultons l'état du dépôt avec `git status`  
```
On branch master
nothing to commit, working tree clean
```
Ici git nous indique que tout va bien, et que tout le contenu du dépôt est sauvegardé. Pas de fichiers modifiés, pas de fichiers dans l'index, etc.  

Maintenant voyons l'historique avec `git log` :  
```
commit 610aaf3129d86c5ec2f3dca053b4129731501731 (HEAD -> master)
Author: PanPraescribens <fibojoly@hotmail.com>
Date:   Sun Mar 8 16:08:23 2026 +0100

    Ajoute hello.txt

    La première ligne est un résumé du commit, et après un saut de ligne,
    vous pouvez également rédiger un message un peu plus détaillé qui
    n'apparaitra pas dans la vue résumée de l'historique.
```

On peut voir pas mal de détails pas forcément compréhensibles ou utiles pour le moment : 
* hash du commit : une référence qui identifie de manière unique le commit et donc les changements qu'on a fait, à l'intérieur du dépôt git.
* (HEAD -> master) : des références qui servent à se réperer à l'intérieur de l'historique. Ici on indique que ce commit est le dernier en date ("tête") sur la branche master. 
* Author : l'auteur du commit, renseigné avec ce que vous avez configuré précédemment dans `user.name` et `user.email`
* La date
* Le message de commit : ici affiché dans son ensemble.

Pour un affichage plus compact, essayez la commande `git log --oneline`
```
610aaf3 (HEAD -> master) Ajoute hello.txt
```
Qui contient :
* Les 7 premiers caractères du hash mentionné précédemment
* Référence de branche
* Résumé du message de commit, la première ligne seulement

## Et on continue...
Vous venez de voir les commandes essentielles de git :
* git status    : la situation de votre dépôt
* git log       : l'historique de votre dépôt
* git add       : pour ajouter un changement à votre commit
* git commit    : pour sauvegarder l'état de votre dépôt

### Modification d'un fichier suivi
Modifions maintenant le fichier hello.txt pour voir ce qui se passe :  
```
echo "Bonjour tout le monde!" >> hello.txt
```
Puis lisons le résultat de la commande `git status`
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
Là encore voyez les différentes options que git vous propose :  
* `git add hello.txt` va vous permettre de sauvegarder les modifications dans votre prochain commit
* `git restore hello.txt` vous permet d'effacer les changements ! Attention, il n'y aura aucun moyen de les récupérer !

Et remarquez bien que git vous indique que rien n'a été ajouté à l'index pour le moment. Si vous faitez `git commit`, vous devriez obtenir le même message que `git status`, vous indiquant que vous n'avez rien ajouté à l'index, donc aucun commit ne peut être créé.  
Git sait que vous avez modifié le fichier hello.txt, mais sans lui dire explicitement, il ne va pas mettre ces changements dans votre prochain commit.  

### Ajout d'un nouveau fichier
Si vous créer un nouveau fichier, git vous l'indiquera explicitement dans le message de statut.  
```
echo "Autre fichier" > test.txt
git status
```
va vous renvoyer le message suivant : 
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
vous pouvez voir séparément les fichiers non-suivis (test.txt qui vient d'être créé), les fichiers suivis modifiés (hello.txt).
Et encore une fois git vous indique que rien n'est sélectionné pour faire un commit.

### Suppression d'un fichier
Effacez le fichier hello.txt puis affichez à nouveau le status et vous aurez :
```
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    hello.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
qui vous indique à nouveau un changement sur un fichier suivi, à savoir ici la suppression.
Comme tout changement, si vous souhaitez que git le prenne en compte, il va falloir l'ajouter à l'index avec `git add hello.txt`
Si vous le faites et que vous affichez à nouveau le statut vous aurez :
```
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    hello.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt
```
Vous remarquerez qu'on a maintenant bien la section "Changes to be commited" qui indique qu'un commit peut être créé pour prendre en compte ce changement. 

### Un autre commit
Ajoutons le fichier test.txt à l'index avec `git add test.txt` et regardons le status : 
```
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    hello.txt
        new file:   test.txt
```
qui confirme les changements que nous allons maintenant sauvegarder.  

Utilisons `git commit -m "Suppression de hello.txt et création de test.txt"` qui va directement créer un commit avec un message court.  

Utilisons `git log --oneline --stat` qui va nous afficher l'historique :
```
07b5500 (HEAD -> master) Suppression de hello et ajout de test
 hello.txt | 1 -
 test.txt  | 1 +
 2 files changed, 1 insertion(+), 1 deletion(-)
610aaf3 Ajoute hello.txt
 hello.txt | 1 +
 1 file changed, 1 insertion(+)
```
L'option `--oneline` nous affiche la forme succinte de notre message, pendant que l'option `--stat` nous donne des infos sur les changements.  

Notez également que le dernier commit en date est **en haut**.

### Voyage dans le temps
Pour finir ce TP, utilisons une commande avancée, qui va illustrer clairement l'intérêt de faire des commits.  
```
git reset --hard HEAD~1
```
Git va vous indiquer un message tout simple du genre 
```
HEAD is now at 610aaf3 Ajoute hello.txt
```
mais si vous exécutez la commande `ls` dans votre répertoire vous devriez constater que votre fichier `hello.txt` est revenu, et votre fichier `test.txt` a disparu.  
Et si vous exécutez `git log` vous devriez voir que l'historique a changé !  
Nous avons demandé à git de transformer notre espace de travail et de revenir au commit précédent, en recréant les fichiers effacés, effaçant les fichiers qui n'existaient pas, etc.