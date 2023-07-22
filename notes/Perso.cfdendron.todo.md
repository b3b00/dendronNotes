---
id: z9ut4zvimgofxdj5ry1rpik
title: todo
desc: ''
updated: 1690010676614
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
   - [ ] note tree view (with filter)  
   - [ ] note preview. find JS library for markdown to HTML 
     - [ ] try to render link between notes  
     - [ ] try to render images.
   - [ ] edit
   - [ ] add 
   - [ ] delete