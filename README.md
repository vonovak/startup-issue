# Welcome to your Expo app 👋

necessary modifications steps:

- run `npx patch-package`

- modify the following files:

```settings.gradle
include ':react-native-startup-time'
project(':react-native-startup-time').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-startup-time/android')
```

```app/build.gradle
implementation project(':react-native-startup-time')
```

```mainApplication.kt
...
// react-native-startup-time
import com.github.doomsower.RNStartupTimePackage
import android.os.SystemClock

class MainApplication : Application(), ReactApplication {

  companion object {
    var START_MARK: Long = 0L
  }

  override val reactNativeHost: ReactNativeHost = ReactNativeHostWrapper(
        this,
        object : DefaultReactNativeHost(this) {
          override fun getPackages(): List<ReactPackage> {
            val packages = PackageList(this).packages
            // Packages that cannot be autolinked yet can be added manually here, for example:
             packages.add(RNStartupTimePackage(START_MARK));
            return packages
          }

          ...
      }
  )

  override fun onCreate() {
    START_MARK = SystemClock.elapsedRealtime()
    ...
  }

 ...
}
```
