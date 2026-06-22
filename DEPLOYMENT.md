# Birje — Deployment Plan (Android + iOS)

This is the practical path from "runs on my phone" to "live on both stores," for a solo developer.

---

## 0. Pre-build checklist (both platforms)

- [ ] **App identity:** display name **Birje**; bundle/app ID e.g. `com.<you>.birje` (pick a domain-style ID you'll keep forever — it can't change after publishing).
- [ ] **Version:** set in `pubspec.yaml` as `version: 1.0.0+1` (name `1.0.0`, build number `+1`). Bump the build number every upload.
- [ ] **Icons:** generate from a 1024×1024 master (see Graphics section). Easiest path: add `flutter_launcher_icons` as a dev dependency and run it to produce all sizes + Android adaptive icon.
- [ ] **Splash:** optional but nice — `flutter_native_splash` with a pastel background and a centered Birje. Keep it under a second.
- [ ] **Orientation:** already locked to portrait in `main.dart`.
- [ ] **Audio/ads:** confirm they're disabled/stubbed for v1 (they are) so there are no missing-asset or SDK surprises at review.
- [ ] **Test pass:** run in `--profile` on a real low-end Android and a real iPhone; play to ~1000m; check the pause/resume, shop buy/equip, daily reward across a date change.

---

## Android — build & release

### Signing (one-time)
```bash
keytool -genkey -v -keystore ~/birje-upload.jks \
  -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```
Create `android/key.properties` (do **not** commit it):
```
storePassword=********
keyPassword=********
keyAlias=upload
storeFile=/absolute/path/to/birje-upload.jks
```
Wire it into `android/app/build.gradle` (`signingConfigs.release` reads `key.properties`; set `buildTypes.release.signingConfig = signingConfigs.release`). Standard Flutter signing setup.

### Build the release App Bundle
```bash
flutter clean
flutter pub get
flutter build appbundle --release
# output: build/app/outputs/bundle/release/app-release.aab
```
(You can also `flutter build apk --release --split-per-abi` for sideload testing, but the **AAB** is what Play wants.)

### Google Play Console
1. Create a Play Console account (one-time US$25), create the app, choose **Game**, free.
2. Complete **Store listing**, **Content rating** (IARC questionnaire → expect "Everyone/PEGI 3"), **Target audience** (you may select includes-children → triggers the Families policy; since you collect no data and show no ads, this is straightforward, but answer honestly), **Data safety** (declare: no data collected, no data shared), **Privacy policy URL** (host the template from `PRIVACY_POLICY.md`).
3. **App content** declarations: ads = No (v1), in-app purchases = No (v1).
4. Upload the AAB to **Internal testing** first; add your own account as a tester; install via the opt-in link and verify.
5. Promote to **Closed → Open testing** as desired, then **Production**. First review can take a few days.

---

## iOS — build & release (macOS + Xcode required)

### Apple Developer Program
- Enrol (US$99/year). Create an **App ID** (`com.<you>.birje`) and let Xcode manage signing (automatic signing with your team is fine for a solo dev).

### Build
```bash
flutter build ipa --release
# open the generated archive in Xcode Organizer, or:
open build/ios/archive/Runner.xcarchive
```
Or open `ios/Runner.xcworkspace` in Xcode → set the team & bundle ID → **Product ▸ Archive** → **Distribute App ▸ App Store Connect**.

### App Store Connect
1. Create the app record (name **Birje**, primary language, bundle ID, SKU).
2. Upload the build (via Xcode Organizer or Transporter). Wait for it to finish processing.
3. **TestFlight:** add yourself + friends as testers; for external testers a light Beta App Review is required.
4. Fill the listing (below), set **Age Rating** via the questionnaire (expect 4+), **App Privacy** (select *Data Not Collected*), attach the privacy policy URL.
5. Submit for review. Be ready to answer the "does it use IDFA / ads?" prompt → No for v1.

---

## Store listing content (both stores)

- **Title:** Birje (App Store allows a subtitle: "Hop, Flap, Sparkle!").
- **Short description / promo (Android 80 chars):** e.g. "A cozy one-tap sky-flyer. Flap, collect sparkles, unlock cute looks!"
- **Long description:** lead with the one-sentence hook, then a short bullet list (one-tap fun, sparkle collecting, cute unlockable skins, no ads, no sign-in, plays offline), then a friendly closing line. Avoid keyword stuffing; write for a human (see `LAUNCH.md` for ASO).
- **Category:** Games → Casual (secondary: Arcade).
- **Content rating:** Everyone / 4+ / PEGI 3.
- **Contact + privacy URL:** required on both.

---

## Required graphics (specs so you can generate them)

- **Master app icon:** 1024×1024 PNG, no alpha for iOS, no rounded corners (stores mask them). Birje's face centered on a soft pastel-gradient background; keep detail away from the extreme edges.
- **Android adaptive icon:** foreground (Birje) + background (pastel) layers, 108×108 dp safe zone — `flutter_launcher_icons` handles export from the master.
- **Feature graphic (Play):** 1024×500 PNG. Birje flying through cloud gaps with the title; leave the center clear of tiny text (Play overlays UI).
- **Screenshots:** capture from a real device. Provide phone sizes for both stores (e.g. iPhone 6.7" ≈ 1290×2796 and 6.5"; Android phone ≥ 1080px on the short side, portrait). Show: gameplay with a star burst, a high combo moment, the shop, and a "New Best!" screen. 4–6 shots; light captions help conversion.
- **Optional promo/preview video:** a 15–20s screen recording of a clean run — strong for store conversion and for social.

Tip: design icon and feature graphic from the same palette as `theme.dart` so the brand reads consistently.

---

## Common rejection reasons (and how Birje avoids them)

- **Privacy declarations don't match behavior** → we collect nothing; declare *Data Not Collected* on both and host the matching policy. Don't add an analytics SDK without updating these.
- **Broken/placeholder content or crashes on launch** → test the release build (not just debug) on real devices before submitting.
- **Misleading metadata / screenshots** → use real in-game captures only.
- **Kids-category data/ads rules** → since v1 has no ads and no data collection, you sidestep the strict Families/Kids monetization rules. If you later add ads, you must use child-appropriate, certified ad SDKs and re-declare.
- **iOS: mentioning other platforms or "beta/demo"** → keep listing copy clean and final.
- **Incomplete IAP/ads declarations** → answer "No" for v1; revisit only when you actually ship them.

---

## Release order (recommended)

1. Internal/TestFlight build → fix anything obvious.
2. Soft launch to friends & family.
3. Android Open testing + iOS external TestFlight for a wider beta.
4. Production on both, ideally same week, so any cross-posting points to both stores.
