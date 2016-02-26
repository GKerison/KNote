# Gradle自定义Android sourceSets

```groovy
sourceSets {
      main {
          manifest.srcFile 'AndroidManifest.xml'
          java.srcDirs = ['src']
          resources.srcDirs = ['src']
          aidl.srcDirs = ['src']
          renderscript.srcDirs = ['src']
          res.srcDirs = ['res']
          assets.srcDirs = ['assets']
          jniLibs.srcDirs = ['libs']
      }
      debug.setRoot('build-types/debug')
      release.setRoot('build-types/release')
  }
```
