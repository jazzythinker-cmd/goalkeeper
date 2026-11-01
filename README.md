# Goalkeeper

An Android app for individual goal set-up and tracking: big goals broken into milestones and checkable steps.
Built with Capacitor 8: the whole app is `www/index.html`, wrapped in a native Android project in `android/`.

- App name: **Goalkeeper**
- Package name (app ID): **com.jazzythinker.goalkeeper** — this becomes permanent once you upload to Google Play. Change it in `capacitor.config.json` *and* `android/app/build.gradle` (`applicationId` and `namespace`) before the first upload if you want something else.
- Version: 1.0 (`versionName` in `android/app/build.gradle`). The `versionCode` is set automatically from the GitHub build number.

## What's in the folder

| Path | What it is |
|---|---|
| `www/` | The app itself (HTML, CSS, JS) and bundled fonts |
| `android/` | Native Android project (open this in Android Studio) |
| `assets/` | Source art for the icon and splash screen |
| `store/` | Play Store screenshots (1080 × 1920), feature graphic (1024 × 500) |
| `assets/play-store-icon-512.png` | The 512 × 512 icon Play Console asks for |

## Build in the cloud with GitHub (no Android Studio needed)

Every push to `main` runs **Actions › Build Android app**, which produces two downloads on the run's page:

- `goalkeeper-test-apk`: unzip it and install `app-debug.apk` on your phone to try the app (allow "install unknown apps" when Android asks).
- `goalkeeper-play-store-aab`: unzip it and upload `app-release.aab` to Play Console.

To start a build without changing anything: Actions › Build Android app › Run workflow.
The version code goes up by one with every run, which Google Play needs for each new upload.

Signing uses four repository secrets (Settings › Secrets and variables › Actions):
`GK_KEYSTORE_BASE64`, `GK_KEYSTORE_PASSWORD`, `GK_KEY_ALIAS`, `GK_KEY_PASSWORD`.
Without them the build still runs, but the .aab is unsigned and Play Console won't accept it.

## Alternative: build on your own computer

### One-time setup

1. Install **Node.js 20 or later** (nodejs.org).
2. Install **Android Studio** (developer.android.com/studio). On first launch let it download the Android SDK.
3. In a terminal, inside this folder, run:
   ```
   npm install
   npx cap sync android
   ```

### Try it on your own phone

1. On the phone: Settings › About phone › tap *Build number* 7 times, then Settings › Developer options › turn on *USB debugging*.
2. Connect the phone by USB.
3. Run `npx cap open android`, wait for Android Studio to finish "Gradle sync", choose your phone in the device list and press the green ▶ Run button.

### Build the file you upload to Google Play (.aab)

1. In Android Studio: **Build › Generate Signed App Bundle / APK › Android App Bundle**.
2. Choose **Create new…** to make an upload key (keystore). Pick a strong password.
   **Back up the keystore file and both passwords somewhere safe** (not only on this computer). You need them for every future update.
3. Choose the **release** build. The file appears at `android/app/release/app-release.aab`.
4. Upload that file in Play Console (see the launch kit document for the full Play Console checklist).

## Making changes later

Edit `www/index.html` and push to `main`; GitHub builds a new APK and .aab with a higher version code.

## Notes

- Data is stored on the phone with Capacitor Preferences (Android SharedPreferences). Android's own backup to the user's Google account includes it when backup is on (`allowBackup="true"`).
- Daily reminders use Capacitor Local Notifications. The exact-alarm permission is removed on purpose, so Android may deliver a reminder a few minutes late to save battery.
- Fonts (Bricolage Grotesque, Figtree, IBM Plex Mono) are bundled under the SIL Open Font License, so the app works offline.
