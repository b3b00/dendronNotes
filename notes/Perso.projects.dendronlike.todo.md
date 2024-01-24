---
id: v5vl53dxgz9nlhnn3ro2bjv
title: TODO
desc: DendrOnline TODO
updated: 1698738070599
created: 1659535376026
traitIds:
  - Perso
---

## GitHub

- [X] save file.
- [X] create file

## UI
- [] responsive UI to enable smartphones and tablets

- [X] recherche repository insensible à la casse 

- [X] recherche de note (case insensitive)

- [X] ajout d'une note 

  - [X] se positionner sur la note parent
  - [X] clic sur le bouton ajouter une note  
  - [X] NotesService.CreateNote(parent,name)
  - [X] creer le header dendron
  - [X] updateNotes 


- [X] sauvegarde d'une note
   - [X] : mise à jour du meta updated
   - [ ] ctrl+S ?

- [X] suppression de note
- [X] mise ne surbrillance de la note sélectionnée


- [X] utiliser une partial pour la barre de boutons
   - [X] masquer le bouton [save] si la note n'a pas été modifiée
   - [ ] ~~barrer/debarrer les icônes d'affichage~~
   - [X] bouton undo pour annuler les modifications non sauvegardées


- better :
   - [X] icon to add 
   - [X] icon to del
   - [X] [google material icon & symbols](https://fonts.google.com/icons)

```html 
<link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined" rel="stylesheet" />

<span class="material-symbols-outlined">Add_Box</span>
<span class="material-symbols-outlined">Delete</span>
```



- editor and preview :
   - [X] display editor and preview in 2 different tabs for better accessibility
   - [ ] only when coming from smartphone ?

# édition

- [X] garder en mémoire les modifications même si on change de note.
   - [X] identifier les notes modifiées dans l'arborescence.
   - [X] mise à jour automatique de l'arborescence au cours de la saisie. 

# More features

## [Links](https://wiki.dendron.so/notes/3472226a-ff3c-432d-bf5d-10926f39f6c2/)
- [Back links] (https://wiki.dendron.so/notes/2l54qrzcil50bufntojzbpo/) : add a back link panel.
- Links to notes : set URLs on travel through linked notes. 


