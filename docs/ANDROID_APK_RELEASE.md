# Publish Nexus Ops for Android — direct GitHub APK download

> **Status:** No verified APK is currently included in this public showcase or in the previously uploaded Flutter source-upgrade package. This is a publication checklist, not proof of a release.

This GitHub repository is intentionally a **public product showcase**; the proprietary Flutter source stays private. A GitHub Release may publish the final compiled APK without publishing the source code.

## Exact download URL to use once the APK is published

```text
https://github.com/yashransubhe-prog/Nexus-Ops/releases/latest/download/NexusOps.apk
```

This URL resolves directly to the binary attached to the most recent eligible GitHub Release **only after a release with an asset named exactly `NexusOps.apk` exists**. It is not yet a functional download.

## Prepare a real release on the development machine

1. Open the full, authorized Nexus Ops Flutter project locally. The source-only upgrade package must first be applied to the working app, and the full Android/Firebase project configuration must be present.
2. Run `flutter pub get`, `flutter analyze`, and `flutter test`. Fix any build or test failures.
3. Validate Firebase project selection, login, Firestore rules, Storage rules, and permissions against a test project. Remove hardcoded credentials.
4. Configure a proper Android release signing key (keep the keystore and passwords **private**, not in this public repository).
5. Build a release APK on a machine with Flutter, Android SDK and JDK installed:

```powershell
flutter build apk --release
```

6. Install and smoke-test `build/app/outputs/flutter-apk/app-release.apk` on a real Android phone (app starts, login, tasks, assignment, offline/error cases). Test the exact release artifact rather than just a debug build.
7. Rename a **copy** of the tested file to `NexusOps.apk`, using the same build bytes:

```powershell
Copy-Item ".\build\app\outputs\flutter-apk\app-release.apk" ".\NexusOps.apk"
```

8. In the **public Nexus-Ops GitHub repository**, open **Releases → Draft a new release**; choose a tag such as `v1.0.0`; attach `NexusOps.apk`; mark it as a release (not a prerelease); and publish. Publish a brief changelog and mention any known setup/login requirements. Do not upload source ZIPs, Firebase environment files, keystores, or customer data.

If GitHub CLI (`gh`) is authenticated and the user has tested the APK, the following command can replace the manual release steps:

```powershell
gh release create v1.0.0 ".\NexusOps.apk" --repo yashransubhe-prog/Nexus-Ops --title "Nexus Ops — Android v1.0.0" --notes "Initial Android APK release."
```

## Switch the README to the direct-download button

Once the release URL is confirmed to work on an Android phone, replace the temporary release-status area near the top of `README.md` with:

```md
<div align="center">

### Get Nexus Ops for Android

[![Download Nexus Ops APK](https://img.shields.io/badge/⬇_Download-Nexus_Ops_APK-2DD4BF?style=for-the-badge&logo=android&logoColor=white)](https://github.com/yashransubhe-prog/Nexus-Ops/releases/latest/download/NexusOps.apk)

**Android APK · Direct download · GitHub Releases**

</div>
```

To avoid font/icon issues, the button can alternatively be a simple Markdown link:

```md
[**⬇ Download Nexus Ops APK for Android**](https://github.com/yashransubhe-prog/Nexus-Ops/releases/latest/download/NexusOps.apk)
```

## Mobile installation notes

- GitHub releases are accessible in Chrome, Edge and other Android browsers.
- Android may require permission to install apps downloaded outside the Play Store. Users should install only files they trust.
- A publicly downloadable APK can be analyzed and reverse-engineered. **Private source control does not make a distributed APK secret**; do not embed sensitive credentials in its binary.
- Make no unverified security, compliance or production-readiness promises in the README.
