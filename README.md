
# Git/Github : Du local au distant

## Initialisation

1. Créer un token

https://github.com/settings/tokens 

2. Pour vérifier l'email :

`git config --global user.email`

3. Pour mettre à jour le mail :

`git config --global user.email "ton-email@exemple.com"`

4. Vérifier le nom d'utilisateur

`git config --global user.name`

5. Si besoin de le mettre à jour :

 `git config --global user.name "TonNomGitHub"`

6. Créer le repo en local
7. Initialiser ce repo

`cd (disk):\\FOLDER1\\FOLDER2`

`git init`

8. Connecter le repo local à github

`git remote add origin https://github.com/ton-nom/ton-repo.git` 

9. Vérifier la connexion

`git remote -v`

10. Pour corriger l'adresser url :

`git remote set-url origin adresse_url_correcte_ici`

## Ajouter un fichier à l'index/staging area

Pour ajouter un fichier précis 

`git add README.md`

Pour ajouter tout le contenu du repo

`git remote set-url origin adresse_url_correcte_ici`

## Envoyer l'index vers le local repository

`git commit`

`git commit -m "Ajout du fichier readme"`

## Envoyer le code vers GitHub

`git push -u origin main` 

Se connecter si besoin 


## Le fichier .gitignore

Tu peux ignorer des fichiers individuels (secret.txt).

Tu peux ignorer des extensions de fichiers (*.log).

Tu peux ignorer des dossiers entiers (node_modules/).
