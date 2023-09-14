---
id: h56hcsfcmwivattnykhzlwe
title: Postman.Net
desc: ''
updated: 1692893603305
created: 1692429431471
---

# Goals

Replace Postman desktop app for :
  - better performance :
    - loading collection
    - preparing run ? WTF ? Why this so long even for large collections ?
  - add missing features
    - replay failed tests !

# Not goals

This is not intended to be on par with features. Some features may be dropped at least at first :
  - grpc API
  - cloud sync !

## chanlenges 
### scripts

Should be able to play PM scripts including :
  - special pm object
  - chai.js assert
  - module import (cheerio ...)

### Authentication

manage a subset of PM Auth methods
 - OAuth2 is a must
   - authorization code
   - client credentials
  