# Git/Github

## Du local au distant

### 1 - Initialisation

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

### 2 - Le fichier .gitignore

Tu peux ignorer des fichiers individuels (secret.txt).

Tu peux ignorer des extensions de fichiers (*.log).

Tu peux ignorer des dossiers entiers (node_modules/).

### 3 - Ajouter un fichier à l'index/staging area

Pour ajouter un fichier précis

`git add README.md`

Pour ajouter tout le contenu du repo

`git add .`

### 4 - Envoyer l'index vers le local repository

`git commit -m "Ajout du fichier readme"`

### 5 - Envoyer le code vers GitHub

`git push`

Si premier push, il faut préciser où envoyer les fichiers :

`git push -u origin main`

Se connecter si besoin

### Spécial : publier d'une branche main privé vers une branche publique

* Ouvrir le terminal intégré de VS Code à la racine du dépôt.
* Vérifier et sauvegarder l’état de `main` (s’assurer qu’il n’y a rien de non committé)

git checkout main
git status

si des modifications non committées :

git add .
git commit -m "WIP: sauvegarde avant synchronisation public"

Créer une sauvegarde locale (sécurise l’état actuel si besoin)

git branch backup-main-$(date +%Y%m%d_%H%M)

Récupérer les remotes et les références distantes

git remote -v
git fetch --all

Si vous n’avez pas encore le remote du dépôt public, ajouter ou mettre à jour son URL (remplacez `<URL_PUBLIC>` par l’URL SSH/HTTPS du repo public) :

git remote add public <URL_PUBLIC>

ou si le remote existe déjà mais incorrect :

git remote set-url public <URL_PUBLIC>

Méthode rapide (pousser la branche `main` telle quelle vers la branche distante `public-branch-wsc`)

Cela fait de la branche distante `public-branch-wsc` la même chose que votre `main` local, sans modifier votre branche `main` locale :

git push public main:public-branch-wsc -u --force-with-lease

`--force-with-lease` force la mise à jour seulement si la branche distante n’a pas été modifiée par quelqu’un d’autre depuis votre dernier fetch.

6. Méthode alternative (mettre d’abord la branche locale `public-branch-wsc` à l’état de `main`, puis pousser)

   Utile si vous voulez voir localement la branche `public-branch-wsc` avant push :

git checkout public-branch-wsc         # si la branche locale existe
git reset --hard main                   # rend la branche public-branch-wsc identique à main
git push public public-branch-wsc -u --force-with-lease

Si la branche locale `public-branch-wsc` n’existe pas :

git checkout -b public-branch-wsc main
git push public public-branch-wsc -u

Si vous préférez merger (conserver l’historique et gérer les conflits)

git checkout public-branch-wsc
git merge main      # résoudre conflits si présents, commit
git push public public-branch-wsc -u

Vérifier que la branche distante a bien la même référence que `main`

comparer le dernier commit

git rev-parse main
git ls-remote --heads public public-branch-wsc

ou localement

git log --oneline -n 5 public-branch-wsc

Précautions

* Ne forcez (`--force` ou `--force-with-lease`) que si vous comprenez que cela réécrit l’historique du dépôt public. `--force-with-lease` est plus sûr que `--force`.
* Ne poussez pas de secrets/clefs privées vers le dépôt public.
* Si d’autres personnes travaillent sur `public-branch-wsc`, prévenir avant d’écraser.

Commandes prêtes à copier-coller pour la méthode la plus directe (depuis la racine du dépôt) :

git checkout main
git add .
git commit -m "Sauvegarde avant push public"     # sauter si rien à committer
git remote add public <URL_PUBLIC>                # si nécessaire
git fetch --all
git push public main:public-branch-wsc -u --force-with-lease

---

## Récupérer un repo distant : Git Clone

récupérer le lien du repo sur github

dans le terminal de commande / vscode, aller dans le dossier où sera télécharger le repo sous forme de dossier :

cd disk:\\\PATH1\\\PATH2... (rappel : "cd .." pour remonter dans le chemin actuel)

puis : `git clone url_du_repo`

Git va télécharger le repo

pour lister les éléments présent :

`ls`

ou

`dir`


---

## Gestion des branches

### 1 - Voir les branches

Pour voir toutes les branches (locales, et distantes) :

`git branch -a`

Pour voir toutes les branches distantes :

`git branch -r`

Travailler avec les branches


### 2 - Créer une branche

`git branch nom_de_la_branche`

Pour basculer sur la nouvelle branche :

`git checkout nom_de_la_branche_ou_je_veux_aller`

Pour créer ET basculer en même temps sur la branche :

`git checkout -b nom_de_la_branche_que_je_créé`


### 3 - Fusionner des branches sur Github

Sur Github, `pull request` permet de fusionner une branche au main ou à un autre branche

#### 1 - Créer la demande

Aller dans le repo > pull requests > Create a pull request

base:main --> la branche qui va recevoir les modifications

compare:new_feature --> la branche qui va être fusionnée à la base

compléter le texte avec 3 sections : Modifications apportées + Contexte + Action demandée

CTA Create pull request

#### 2 - Faire la fusion

Aller dans le repo > pull requests > cta merge pull request

#### 3 - Supprimer la branche

Après le merge et un message de succès ("pull requesgt successfully merged and closed), CTA "Delete branch"

---

## Gestion des conflits

| commande                  | action                                             | quand utiliser                                 |
| ------------------------- | -------------------------------------------------- | ---------------------------------------------- |
| git diff                  | voir TES modif en local                            | avant git add                                  |
| git diff --cached         | voir differences entre index et dernier commit     | avant git commit                               |
| git fetch                 | récupérer les MAJ de github sans les appliquer   | avant git pull pour voir ce qui a changé      |
| git diff main origin/main | comparrer la branche locale et la version distance | après un git fetch pour voir les différences |
| git pull                  | récupérer et appliquer les changement            | pour synchroniser le code avec github          |

### 1 - Vérifier si GitHub a des mises à jour sans toucher à ton code

`git fetch`.

Je veux voir s'il y a du nouveau sur GitHub, mais sans écraser mon travail local ni regarder les différences, j'utilise donc

### 2 - Vérifier les modifications en local

`git diff` est une commande qui te permet de voir les différences entre deux sources :

* Entre ton espace de travail (working directory) et l'index (staging area)
* Entre l'index et le dernier commit
* Entre deux commits
* Entre deux branches

Si après avoir regardé les changements, tu veux les récupérer et les fusionner dans ton code, tu fais :

### 3 - Récupérer les modifications depuis Github

`git pull origin main`

Il récupère les modifications depuis GitHub et les applique à ton code local.

Quand tu veux mettre à jour ton repo local avec les nouvelles modifications de ton collègue.

### 4 - Fusionner des branches en ligne de commande

`git merge` : fusionne une branche dans une autre directement depuis l'ordinateur

**Utilisation ?**

* Quand tu travailles seul et que tu veux fusionner tes branches localement
* Quand tu préfères utiliser le terminal plutôt que l'interface GitHub

`git checkout main` : je me place sur la branche main

`git merge feature` : je fusionne feature dans main

### 5 - Exemple de process

1. git branch : je check les branches dispo
2. git checkout main : je me positionne sur la branche main
3. git merge nouvelle_feature : je fusionne la branche nouvelle_feature à la branche où je suis positionné (main grâce au checkout préccédent)
4. git branch : je revérifie où je suis et le nom des branches pour l'étape suivante
5. git branch -d nouvelle_feature : suppression de la branche devenue inutile

### 6 - Différence entre git merge local et Pull Request

`git merge` fusionne les branches uniquement sur ton repo local. 

Après la fusion, tu dois encore faire un `git push` pour envoyer ces changements sur GitHub.

### 7 - Gestion du conflit

Quand Git signale un conflit, il modifie le fichier concerné en y ajoutant des marqueurs spéciaux. Dans VSCode, tu as la possibilité de résoudre le conflit en cliquant sur "Resolve in Merge Editor"

Dans VS Code : 

Si tu ne choisis pas une des deux versions il te faudra :

* Ignorer les boutons "Accept" et éditer directement le fichier qui contient les marqueurs de conflit.
* Modifier le contenu comme vous le souhaitez
* Supprimer manuellement tous les marqueurs de conflit
* Sauvegarder le fichier

En ligne de commande : 

git pull origin main : si conflit, va afficher un message d'erreur et des hints


* Résolution manuelle classique

  * `git pull` déclenche le conflit.
  * Ouvrir les fichiers contenant les marqueurs de conflit.

    * git status
    * nano nom_du_fichier
  * Éditer en supprimant les marqueurs et en conservant la version désirée ou une combinaison des deux.

    * Supprimer la ligne `<<<<<<< HEAD`.
    * Choisir ce qui doit rester du bloc “contenu local”.
    * Supprimer la ligne `=======`.
    * Choisir ce qui doit rester du bloc “contenu distant”.
    * Supprimer la ligne `>>>>>>> origin/main`.
    * Réordonner ou fusionner manuellement si nécessaire.
    * Vérifier que le fichier final ne contient plus aucun marqueur.
  * `git add <fichier>`
  * `git commit`
  * `git push`
  * Favoriser la version distante

    * `git fetch`
    * `git reset --hard origin/main`
    * Ecrase totalement les modifications locales.
    * Adapté uniquement si la version locale n’a aucune valeur.
  * Favoriser la version locale

    * `git pull --strategy-option ours`
    * Utilise la version locale lors des conflits, mais les autres changements du distant sont intégrés.
    * Commit automatique ou manuel selon le contexte.
    * Attention: ce n’est pas un simple “tout local”, c’est une fusion biaisée.
  * Favoriser la version distante

    * `git pull --strategy-option theirs`
    * Inverse du précédent: en cas de conflit, prend la version distante.
    * Reste une fusion, pas un reset.
  * Rebaser au lieu de merger

    * `git fetch`
    * `git rebase origin/main`
    * Le conflit apparaît.
    * Résolution manuelle classique.
    * `git add <fichier>`
    * `git rebase --continue`
    * `git push --force-with-lease` si nécessaire.
    * Plus propre mais plus risqué si on ne maîtrise pas l’historique réécrit.
  * Utiliser merge sans pull pour contrôler le processus

    * `git fetch`
    * `git merge origin/main`
    * Résolution manuelle classique puis commit.
    * `git push`
  * Annuler le merge en cours si besoin

    * `git merge --abort`
    * Retourne à l’état pré-fusion pour repartir propre.
