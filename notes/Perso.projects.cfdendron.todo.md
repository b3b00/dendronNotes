---
id: z9ut4zvimgofxdj5ry1rpik
title: Perso.projects.cfdendron.todo
desc: TODO list for CF Dendron
updated: 1711635840279
created: 1689943392141
---

# bugs

 - [X] renderHtml does not work on /login !
   - cloing Request was the cause. Simply building `new Request('http://.../sometemplate.tpl)` is ok.

# features


 - [X] stateless session  : [webcrypt-session](https://github.com/toyamarinyon/webcrypt-session)
 - [ ] GitHub OAuth middleware
 - [ ] GitHub API client [Octokit.js](https://github.com/octokit/octokit.js) ?
 - [ ] Htmx views
   - [ ] repository view (with filter)
   - [ ] note tree view (with filter)  ; 
https://stackoverflow.com/questions/31885263/mustache-js-how-to-create-a-recursive-list-with-an-unknown-number-of-sub-lists
   - [ ] note preview. find JS library for markdown to HTML 
     - [ ] try to render link between notes  
     - [ ] try to render images.
   - [ ] edit
   - [ ] add 
   - [ ] delete
