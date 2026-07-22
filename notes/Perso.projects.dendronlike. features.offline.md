---
id: Perso.projects.dendronlike. features.offline
title: Perso.projects.dendronlike. features.offline
desc: Offline mode
updated: 1784706840324
created: 0
---
# Use dendr-Online offline

## What

### reading content offline

When offline it should still be possible to browse :
  - tree notes
  - stash notes
  - bookmarks : list and read available content

### adding content offline

It should also be possible to add content 
  - notes for sure (tree or stash)
  - bookmark is more questionnable 

This implies a way to sycnhronize content back to server (and GitHub) when network comes back.
Even though dendrOnline is single user concurrent modifications may occur. for example this scenarion with 2 clients (same user)  Phone and Laptop
   1. Phone sync data and store for offline use (v1)
   2. Phone is shutdown for some reason and stays with v1
   3. Laptop does some changes => v2.laptop
   4. Phone is back but no network , still v1
   5. Phone add some content  => v2.phone
   6. Phone back online and synchronize => need some conflict resolution

## How

### login 

### data availibility
Of course we need some client side storage. This is already the cas for : 
   - stash notes (localstorage) : both categories and notes' content
   - tree (localstorage) : only for tree structure, not notes' content




