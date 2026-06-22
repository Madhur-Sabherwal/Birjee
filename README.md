# Birje — *Hop, Flap, Sparkle!* ✨

A wholesome, original one-tap endless sky-flyer. Guide **Birje** — a round, fluffy sky-blue creature in a propeller beanie — between soft cloud pillars, scoop up sparkle stars, and chase your best flight through a pastel world of floating islands and rainbows.

- **Genre:** hyper-casual one-tap flyer (gravity + tap impulse)
- **Audience:** everyone, 4+
- **Sessions:** ~1–5 minutes
- **Engine:** Flutter + [Flame](https://flame-engine.org)
- **Data:** 100% local. No accounts, no servers, no analytics, no tracking.

> ☕ If Birje makes you smile, you can support development: **https://buymeacoffee.com/MaadhurSabherwal**

---

## Two ways to try it

1. **Instant prototype (`birje_prototype.html`)** — open it in any modern browser (desktop or phone). Fully playable: physics, scoring, skins shop, achievements, daily reward, sound. Great for feeling the game loop in seconds. This is the fastest way to test difficulty and feel.
2. **Flutter project (this folder)** — the shippable version that builds to real Android & iOS apps. Same design, ported to Flame components.

The HTML prototype and the Flutter app are intentionally kept in sync on mechanics so you can tune in the browser, then mirror changes in Dart.

---

## Run the Flutter app

### Prerequisites
- Flutter SDK (stable channel), Dart `>=3.3.0`. Check with `flutter doctor`.
- Android Studio (Android SDK + an emulator) and/or Xcode (for iOS, macOS only).
- A physical device is best for judging 60fps feel.

### First run
```bash
cd birje/flutter
flutter pub get
flutter run            # pick a connected device/emulator
```

> **Note:** the Dart code targets the Flame **1.x** API and was written outside a Flutter toolchain, so it has not been compiled in this environment. If `flutter pub get` resolves a newer Flame, you may need to adjust a method name or two (Flame occasionally renames lifecycle hooks between minor versions). The structure and logic are complete; treat any analyzer squiggles as quick version-alignment fixes, not redesigns.

### Handy commands
```bash
flutter devices                 # list targets
flutter run -d <deviceId>       # run on a specific device
flutter run --profile           # profile mode — realistic performance
flutter analyze                 # static analysis
flutter clean && flutter pub get  # if the build gets into a weird state
```

### IDE
VS Code (or Cursor) with the Dart + Flutter extensions, or Android Studio. Use the Flutter DevTools performance overlay to watch frame timings while flapping.

---

## Project structure

```
flutter/
├── pubspec.yaml                 # deps: flame, shared_preferences
└── lib/
    ├── main.dart                # entry: portrait lock, GameWidget, tap-to-flap
    ├── birje_game.dart          # BirjeGame (FlameGame): state machine, scoring, economy
    ├── theme.dart               # colour palette + skin catalogue
    ├── world_generator.dart     # seeded procedural pillar/sparkle spawning
    ├── components/
    │   ├── background.dart       # parallax sky, rainbows, sparkle burst particles
    │   ├── birje_player.dart     # player physics + procedural character art
    │   ├── pillar.dart           # cloud-pillar obstacle + forgiving hitbox
    │   ├── sparkle.dart          # collectible star
    │   └── scroll_component.dart # base class: scroll left, auto-despawn
    ├── managers/
    │   ├── save_manager.dart     # shared_preferences persistence (single JSON key)
    │   ├── audio_manager.dart    # SFX hook points (safe no-op until you add clips)
    │   └── ad_manager.dart       # rewarded-ad hook points (disabled by default)
    └── ui/
        └── overlays.dart         # Menu / HUD / Game Over / Pause / Shop widgets
```

### How a tap becomes a flap
`main.dart` wraps the `GameWidget` in an opaque `GestureDetector`, so taps on empty sky reach `BirjeGame.onScreenTap()`. Overlay buttons are normal Flutter widgets layered above and absorb their own taps, so pressing **Shop** doesn't also flap.

### Art with zero image assets
Birje, pillars, stars, rainbows and islands are all drawn procedurally with `Canvas` (circles/arcs/paths). That keeps the download tiny and makes recolouring trivial — every cosmetic skin is just a palette swap (see `theme.dart`). You can later swap any painter for a sprite without touching game logic.

---

## Enabling sound (optional)
Audio is stubbed so the game runs silently out of the box. To turn it on:
1. Add `flame_audio` to `pubspec.yaml` (a commented line is already there).
2. Drop short `.ogg`/`.wav` clips into `assets/audio/` and declare the folder under `flutter:` `assets:` in `pubspec.yaml`.
3. Fill in the `play(...)` bodies in `managers/audio_manager.dart` (call sites already exist: flap, collect, milestone, poof, coin).

Keep clips tiny and soft — gentle chimes and a light "boing," nothing harsh.

---

## Enabling rewarded ads later (optional, ethical)
`managers/ad_manager.dart` is a disabled stub. If you ever monetize:
- Use **rewarded ads only** (optional bonus shards / one continue) — never interstitials that interrupt a run, no banners crowding the play area.
- Add `google_mobile_ads`, set up AdMob unit IDs, and gate everything behind an explicit player tap.
- Core play must stay 100% complete and fair without ever watching an ad. No pay-to-win.

---

## Risk mitigation & edge cases (v1)

**Low-end device performance.** Art is vector-cheap; particle counts are bounded and offscreen components auto-despawn in `ScrollComponent`, so the live object count stays flat no matter how far you fly. If you target very weak hardware, cap `devicePixelRatio` work by keeping painters simple (already done) and avoid adding large blur layers.

**Battery & thermals.** 60fps one-tap games are light, but: pause the engine when the app is backgrounded (Flame pauses with the widget lifecycle), and avoid continuous audio. No networking means no radio wake-ups.

**Screen sizes & aspect ratios.** Everything is expressed as fractions of screen size (gaps, speeds, spawn positions in `world_generator.dart` use `0.xx` of width/height), so tall and short phones both get a fair gap. Test on a short 16:9 and a tall 20:9 device; the gap is bounded so it never becomes impossible on narrow screens.

**Accessibility.** High-contrast, large tap target (the whole screen), no time-pressure menus, no reliance on colour alone for obstacles (pillars have shape + shadow). Forgiving hitbox (player collider is ~0.72 of the visible body; pillar collider ~0.82 of its width) so near-misses feel fair. Future: a "calm mode" with slower start speed and a colour-blind-friendly star outline.

**Scope creep prevention.** v1 is deliberately one mechanic done well. The roadmap (see `docs/GDD.md`) lists tempting additions (power-ups, biomes, characters) explicitly *parked* for post-launch so the first release actually ships.

**App-store policy compliance.** No data collection, no ads, no third-party SDKs in v1 → the simplest possible privacy story (template in `docs/PRIVACY_POLICY.md`). Content is wholesome and unambiguously 4+/PEGI 3.

**Architecture for safe growth.** Game state, world generation, persistence, audio and ads are separated. New power-ups become new `ScrollComponent` subclasses; a new biome is a palette + background variant; a future cloud save would slot behind `SaveManager` without touching gameplay. Nothing in the codebase assumes a backend.

---

## Credits
Design & code: Madhur Sabherwal.
Support the game: **https://buymeacoffee.com/MaadhurSabherwal** ☕

*(This is a personal creative project. It intentionally keeps its own little world — no outside affiliations attached.)*
