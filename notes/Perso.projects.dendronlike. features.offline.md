---
id: Perso.projects.dendronlike. features.offline
title: Perso.projects.dendronlike. features.offline
desc: Offline mode
updated: 1784718318148
created: 0
---
# Use dendr-Online offline

## What

### reading content offline

When offline it should still be possible to browse :
  - tree notes
  - stash notes
  - bookmarks : list and read available content

When coming online todo bookmarks' content must be relaoded 

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

**bookmarks**
When offline a bookmark if added is necessarily incomplete, it only contains the link.
So when offline adding a bookmark should : 
  1. ask user for a short description to be displayed while not complete
  2. mark bookmark as incomplete. when synced backend will retrieve all missing informations and return it to  FE
  3. incomplete bookmark must be clearly identified in the bookmarks view. Maybe a different border (red or violet) 


## How

### login 

### data availibility
Of course we need some client side storage. This is already the cas for : 
   - localstorage 
       - stash notes  : both categories and notes' content
       - tree : only for tree structure, not notes' content
   - indexeddb :
       - bookmark articles' content




