# Git

## Créer les ssh key



### Command pour créer un repository

```bash
git config --global user.name "John Doe"            # --global to have
git config --global user.email johndoe@example.com  #  it set for all repo
git init  # create the git repo
git remote add origin <url> # add a remote repo with name origin

git clone <url> # create a local repo from an existing remote one
git remote add <short_name> <url> # create a short name for the url
```

### Command de base :

```bash
git status # see the files staged or committed
git add <file> # add a file to the stage(=waiting list for next commit)
git restore --staged <file> # unstage the file
git commit -m"message explicatif" # commit changes to the staged files
git commit -a -m"message explicatif" # commit all changed files
git push origin main # push the changes to the branch main to the remote repository named origin
git pull origin main # pulls the changes from remote to local
```

### Commandes pour les branches

```bash
git branch #dislpays all the branches (* to indicate the current one)
git branch <new_branch_name> # create new branch
git checkout <branch_name> # change branch
git branch -d <branch_name> # delete the branch
git branch -D <branch_name> # delete a branch with modification

# merge branch1 into main
git checkout main 
git merge branch1 
```

### Commandes pour annuler un changement

#### Avant d'avoir commit

```bash
git stash # annule les changement fait mais les sauvgardes dans le "stash"
# |-> equivalent to git stash push
git stash list # voir tout les stash s'il y en a plusieurs
git stash apply <name> # prend l'entrée name du stash et la réapplique 
git stash pop <name>   # prend les changement du stash, les réapplique et supprime ceux si du stash
git stash drop <name> # supprime les changement du stash
```

Si < name > n'est pas spécifier, c'est la dernière entrée qui est choisie.

#### Après avoir commit

```bash
git reset --hard HEAD^ # reset le dossier dans son état avant le dernier commit
git log                 # voir le nom des commits
git reset --hard <name> # reset le dossier dans son état avant le commit <name> 
git reset --soft <name> # permet de voir l'état du code au commit <name> sans rien supprimer
```

### Merge confilct avec des fichiers binaires

```bash
git merge branch1 # va mettre message d'erreur

#Pour garder la version courante 
git add somefile.dll 
git commit –m “using my file”

#Pour garder la version de la branch1
git checkout branch1 somefile.dll # Récupère le fichier de l'autre branch
git add somefile.dll 
git commit –m “using the file from branch1”
```
