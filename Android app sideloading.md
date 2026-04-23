
  ## 1 On your Fire TV, enable developer access:
  
  - Settings -> My Fire TV -> About -> click the device name 7 times
  - Then Developer options -> ADB debugging on
  - Also enable Apps from Unknown Sources if your model exposes it
  - Note the Fire TV IP address from Settings -> Network
  
  ## 2 Build the APK from this repo:
  
```bash
./gradlew :app:assembleDebug
# ./gradlew :app:assembleRelease
```

  The APK should be at: `app/build/outputs/apk/debug/app-debug.apk`, or at `app/build/outputs/apk/release/app-release.apk` for release build.

Optionally sign the release build:
```
./apksigner sign   --ks ~/.android/debug.keystore   --ks-pass pass:android   --ks-key-alias androiddebugkey   --key-pass pass:android   --out ~/Code/android-tv-reader/app/build/outputs/apk/release/app-release-local-signed.apk ~/Code/android-tv-reader/app/build/outputs/apk/release/app-release-unsigned.apk
```

 ## 3 Install it from your machine:

```
  adb connect FIRE_TV_IP:5555
  adb install -r app/build/outputs/apk/debug/app-debug.apk
```

Note: `adb` may not be in your PATH, in which case you can find it at `~/Android/Sdk/platform-tools`

  ## 4 Launch it:
  - Usually it will appear on the Fire TV app row because the manifest includes LEANBACK_LAUNCHER
  - Or launch by command: `adb shell monkey -p com.markopapic.tvreader -c android.intent.category.LEANBACK_LAUNCHER 1`


Error: "Unauthorized APK installation detected. Please do not install performance benchmark APKs without permission from deviceprot@amazon.com"