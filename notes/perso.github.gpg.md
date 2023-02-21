---
id: jii2t0ifhpyt03s58kpd58z
title: Gpg
desc: ''
updated: 1676985524626
created: 1676985339817
---

# sources 

https://www.francoz.net/doc/gpg/x218.html

http://irtfweb.ifa.hawaii.edu/~lockhart/gpg/


# création d'une clé

```
gpg --gen-key
``` 

# export clé publique 

```
gpg --export --armor --export-options export-minimal <ID DE LA CLE>
```

# configuration gpg git

```
gpg --list-secret-keys --keyid-format LONG
/c/Users/olduh/.gnupg/pubring.kbx
sec rsa4096/7CC395649D0C9DD4 2018-11-04 [SC] [expires: 2034-10-31]
1C88270EEDFD540135E07BE27CC395649D0C9DD4
uid [ unknown] b3b00 olivier.duhart@gmail.com
ssb rsa4096/8EC2C54ECBB3315F 2018-11-04 [E] [expires: 2034-10-31]
```

configure pgp key
```git config user.signingkey 9D0C9DD4```

force commit signing per repo
```git config commit.gpgsign true```

```
[user]
        name = b3b00
        email = olivier.duhart@gmail.com
        signingkey = 9D0C9DD4
[commit]
	gpgsign = true        
```