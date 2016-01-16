---
layout: post
title: Removing unused language resources from an Android APK when developing with Xamarin
comments: true
---

If you're developing an Android app and add the `Google Play Services` or `Android Support Library` to you're app,
you're probably suddenly seeing an enormous increase in supported languages when uploading the app to Google Play.  
This happens, because those libraries ship with built-in multi language support and Google Play 
has no way of knowing that those additional language files aren't coming from your app itself.  

If you're developing with Java and Gradle, the solution is simple, just add this to your build.gradle file:

```
defaultConfig {
  minSdkVersion 15
  targetSdkVersion 22
  versionCode 75
  versionName "1.0.0"

  resConfigs "en", "de" <--
}
```

This way only German and English resources are included in your final APK.

If you're developing with Xamarin for Android, you're out of luck, there isn't a built-in way to do this.  
Fortunately, after some research, I've found a semi-manual solution that I've also posted on [StackOverflow](http://stackoverflow.com/a/34787251/488695)

1. Download `Apktool` from https://ibotpeaches.github.io/Apktool/
2. Create your final .apk with Xamarin and decompile it with `apktool d MyApp.apk`
3. Go into the `MyApp` directory that `Apktool` has created and look for the `res` directory
4. Remove all `values` directories that end with a language identifier that you don't need, e.g if your app only supports the German language, remove `values-fr`, `values-es`, etc..., but not `values-de`. **Don't** remove non-language directories, e.g `values-v11`!
5. Recompile your app with `apktool b MyApp`
6. The recompiled app package is now in `MyApp/dist/MyApp.apk`. Take this file and sign it with the signtool, then zipalign it. 7. Upload the apk to Google Play

I'm sure there is a way to automate this with a script, but I haven't yet created one.
