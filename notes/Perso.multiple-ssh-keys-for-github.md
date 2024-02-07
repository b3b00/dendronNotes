---
id: 45it3uay6z9nvoyht46g72o
title: Perso.multiple-ssh-keys-for-github
desc: 'How to get different ssh key for many git remote accounts'
updated: 1707324000861
created: 1669144677424
---


# Multiple SSH keys for GitHub

# le problème

comment jongler entre plusieurs clé SSH selon les repo github (entre perso et pro par exemple)


## Créer plusieurs clés publiques

```bash
   $  ssh-keygen -f ~/.ssh/id_perso -t rsa -C "perso@gmail.com"
   $  ssh-keygen -f ~/.ssh/id_pro -t rsa -C "pro@pro.com"
```
⚠️ sous windows term + powershell la commande échoue avec un 

    Saving key "~/.ssh/id_perso" failed: No such file or directory

Sous Powershell il faut donc utiliser le chemin complet
```bash
      $ ssh-keygen -f c:/users/perso/.ssh/id_perso -t rsa -C "perso@gmail.com"
```

## ajouter les clés
```bash
   $  ssh-add ~/.ssh/perso.ppk
   $  ssh-add ~/.ssh/pro.ppk
```

## Modifier .ssh/config


    # perso account
```plaintext
    Host github.com-perso # <- -perso pour préciser le compte
            HostName github.com
            User git
            IdentityFile ~/.ssh/id_perso 
```        
    # pro account
```plaintext
    Host github.com-pro # <- -pro pour préciser le compte
            HostName github.com
            User git
            IdentityFile ~/.ssh/id_pro 
```
Autant de fois que de clés.
le suffixe -xxxx n’est pas forcément le nom de l’utilisateur. il sert juste à identifier la clé lors du clone.

## Cloner un repo avec la clé voulue

Remplacer le github.com par github.com-&lt;XXX&gt; où <XXX> est le nom du compte


    git clone git@github.com-perso:perso/myPersonalRepo.git


    git clone git@github.com-pro:GreatestCompany/awesomeCompanyRepo.git
## Modifier la config git du repo pour forcer le user
### forcer le user 
```plaintext    
    $ cd mypersonalRepo
    $ git config user.name "perso"
    $ git config user.email "perso@gmail.com" 
```
### forcer la clé SSH (utilisation de l'alias github)
```plaintext    
[remote "origin"]
	url = git@github.com-perso:perso/MyPersonalRepo.git
```

# Références

[https://gist.github.com/jexchan/2351996](https://gist.github.com/jexchan/2351996)

