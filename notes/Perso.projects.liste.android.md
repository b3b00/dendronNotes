---
id: Perso.projects.liste.android
title: Perso.projects.liste.android
desc: Porting to android
updated: 1778136599320
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

the package name may be defined in 
   - build.gradle.kts

```kotlin
android {
    namespace = "com.liste.app" // here 
    compileSdk = 35

    defaultConfig {
        applicationId = "com.liste.app" // and here 
        minSdk = 26
        targetSdk = 35
        versionCode = 1
        versionName = "1.0"
    }
```
   - AndroidManifest.xml 

```xml
    <application
        android:name=".ListeApp"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Liste"
        android:package="com.liste.app"> <!--here -->
```


example 
```bash
adb.exe shell am start -n com.liste.app/.MainActivity
```
## using Copilot. 

Copilot did a pretty decent job. Only thing is that it used checkbox instead of button in the lists.
