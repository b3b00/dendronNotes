---
id: 69drgtvtsl85z40stwkearg
title: Perso.projects.classements.share
desc: Share data (readonly)
updated: 1711635855489
created: 1709890487941
---

github issue [#27](https://github.com/b3b00/classements/issues/27)

## description 
given a season and a team :

 - [X] share a link to access in readonly
    - [X] team view :
      - [X] no meeting deletion
      - [X] sheet detail is accessible
    - [X] players view
    - [X] sheet view : no edition available

## link

`GET /#/open/<some opaque token>`

`POST /share  season=? team=?`

```sql
CREATE TABLE shareLinks (id TEXT PRIMARY KEY, account TEXT NOT NULL, season TEXT NOT NULL, team TEXT NOT NULL);

CREATE TABLE virtualSession (
	id TEXT,
	link TEXT,
	CONSTRAINT VIRTUALSESSION_PK PRIMARY KEY (id),
	CONSTRAINT virtualSession_shareLinks_FK FOREIGN KEY (id) REFERENCES shareLinks(id)
);
```



## bypass authentication

find a way to bypass authentication

### virtual session

use a virtual session associated to account. 

**beware security.**


## UI

no header (team selection and navigation buttons)
