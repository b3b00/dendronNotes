---
id: Perso.projects.liste.android
title: Perso.projects.liste.android
desc: Porting to android
updated: 1778135581218
created: 0
---
# Port app from PWA to native Android.

## quirks

### XML files
when adding default resources from studio it generates XML files with leading comments. this prevents compilation.

## starting an app from adb

```bash
adb.exe shell am start -n <PACKAGE_NAME>/<ACTIVITY>
```

where : 
- `<PACKAGE_NAME>` is the app package name. use àdb dumpsys` to find will help
- `<ACTIVITY>` is the class name of the activity, not neccessarily the FQN name, may simply be the class name with a leading dot '.'

example 
```bash
adb.exe shell am start -n com.liste.app/.MainActivity
```
## using Copilot. 

Copilot did a pretty decent job. Only thing is that it used checkbox instead of button in the lists.
