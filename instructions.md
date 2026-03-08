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
Créons un premier sous-répertoire `mkdir depot_01`, puis allons dedans.  
Tapez la commande `git status`
Si vous regardez le contenu du répertoire avec `ls` vous devriez constater que votre répertoire est toujours vide. Mais si vous tapez 