---
id: v5vl53dxgz9nlhnn3ro2bjv
title: Perso.projects.dendronlike.todo
desc: DendrOnline TODO
updated: 1744989586375
created: 1659535376026
---
## GitHub

* [ ] save file.
* [x] create file
* [x] load full dendron in session. Allow
  * [x] fasternavigation
  * [x] black links
  * [x] and better links to notes

## UI

* [x] responsive UI to enable smartphones and tablets

* [x] recherche repository insensible à la casse

* [x] recherche de note (case insensitive)

* [x] ajout d'une note

  * [x] se positionner sur la note parent
  * [x] clic sur le bouton ajouter une note
  * [x] NotesService.CreateNote(parent,name)
  * [x] creer le header dendron
  * [x] updateNotes

* [x] sauvegarde d'une note
  * [x] : ~~mise à jour du meta updated~~
  * [ ] ctrl+S ?

* [x] &#x20;suppression de note

* [x] mise ne surbrillance de la note sélectionnée

* [x] masquer le bouton \[save] si la note n'a pas été modifiée

* [x] bouton undo pour annuler les modifications non sauvegardées

* better :
  * [x] icon to add
  * [x] icon to del

* editor and preview :
  * [x] display editor and preview in 2 different tabs for better accessibility
  * [ ] ~~only when coming from smartphone ?~~

# édition

* [x] garder en mémoire les modifications même si on change de note.
  * [x] identifier les notes modifiées dans l'arborescence.
  * [x] mise à jour automatique de l'arborescence au cours de la saisie.

# More features

## [Links](https://wiki.dendron.so/notes/3472226a-ff3c-432d-bf5d-10926f39f6c2/) [#11](https://github.com/b3b00/dendrOnline/issues/11)

* [x] \[Back links] (<https://wiki.dendron.so/notes/2l54qrzcil50bufntojzbpo/>) : add a back link panel.
* [x] Links to notes : set URLs to travel through linked notes.

## note history

* [ ] display note history
* [ ] display note content in past : view only, no edition

## favorite repository

* [x] &#x20;Allow user to define a favorite repository so he does not have to select it when he come back.

## check list in view mode

Allows checking boxes in live mode. Uses a hash of the item to identify the \[ ] markdown.

Adds a check action to facilitate the entry of `-[ ]`.

