---
id: z9ut4zvimgofxdj5ry1rpik
title: todo
desc: ''
updated: 1689952675017
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
   - [ ] preview 
   - [ ] edit
   - [ ] add 
   - [ ] delete