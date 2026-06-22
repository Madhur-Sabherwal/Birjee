# Birje — Game Design Document (v1)

**Tagline:** Hop, Flap, Sparkle!
**Genre:** Hyper-casual one-tap endless flyer (gravity + tap impulse), with a cosmetic-collection meta-loop.
**Platforms:** Android + iOS (portrait, touch-first). Target 60fps.
**Pitch:** A cozy, joyful "just one more run" flyer. Tap to keep Birje aloft, thread the soft cloud gaps, scoop sparkle stars, and unlock a wardrobe of adorable looks. Calm enough for a 4-year-old, tuned enough to chase a high score on the train.

---

## 1. Core loop

```
See menu → tap to start → flap to stay airborne & thread gaps → grab sparkles
→ build combo → eventually poof → see score + best + shards earned
→ spend shards on a new skin → "one more run"
```

The 30-second promise: a brand-new player should understand the entire game from a single tap, and feel a satisfying near-miss within the first run.

---

## 2. Mechanics (detailed)

**Movement.** Birje holds a fixed horizontal screen position; the *world scrolls left* toward her. Gravity constantly pulls her down. A tap anywhere applies an instant upward impulse. Rapid taps climb; spaced taps glide. There is no ceiling slam — hitting the top or bottom screen edge ends the run with a gentle poof.

**First-tap grace.** Before the first tap, Birje hovers idly (gentle bob) so players can read the screen. The run — and scrolling — begins on first tap. No surprise deaths.

**Obstacles.** Pairs of soft **cloud pillars** with a gap between them, themed as fluffy clouds rather than hard pipes. They scroll in from the right. Touching a pillar ends the run.

**Collectibles.** **Sparkle stars** float in lines and arcs through and around the gaps, rewarding good lines. Occasional **bonus clusters** appear for a bigger combo payoff.

**Forgiveness (feel).** The player collider is ~0.72× the visible body; the pillar collider ~0.82× its width. Near-misses read as near-misses, not cheap deaths — central to the wholesome, low-stress tone.

**Juice.** Tilt toward velocity (nose-up on flap, nose-down on fall), squash/bounce on big flaps, particle burst + chime on star pickup, a soft screen nudge on milestones, and a gentle poof + fade on death. Birje's expression brightens on good runs.

---

## 3. Difficulty & procedural rules

- **Seeded RNG** drives spawns so each run is fresh but reproducible for debugging.
- **Speed ramps** with distance from a calm start (~0.30 of max) to a capped cruise (~0.62 of max). The cap guarantees the game never becomes unfair.
- **Gap shrinks** with distance from ~0.34 of screen height down to a floor of ~0.22, then holds. Bounded so it stays humanly possible on any aspect ratio.
- **Spacing** between pillar pairs is a fraction of width (~0.62), giving consistent rhythm regardless of screen size.
- Density of sparkles and bonus clusters increases slightly with distance to keep later runs rewarding.

All tuning constants live in `world_generator.dart` and `birje_game.dart` for fast iteration.

---

## 4. Scoring & economy

- **Score** = distance ("meters") + sparkle stars × combo multiplier. Long clean flights and big star chains pay off.
- **Combo** builds as you collect stars without dying; it resets on a missed beat/poof. Combo x10 is an achievement.
- **Sparkle Shards** (soft currency) are earned per run: `stars + floor(distance / 40)`. Spent only on cosmetics.
- **Daily reward:** +20 shards on the first run of a new calendar day (tracked locally).
- **Achievements** grant one-time shard bonuses: First Flight (15), Fly 250m (25), Fly 500m (50), Sky Voyager 1000m (120), 50 Sparkles in a run (60), Combo x10 (40).
- **No pay-to-win, ever.** Shards buy looks, not advantages. Everything is earnable by playing.

---

## 5. UI flow (screens)

- **Main Menu:** big title, Birje preview in current skin, Play, Shop, best score, mute toggle. One-tap to play.
- **In-Game HUD:** current score, distance, combo multiplier, pause button, mute. Minimal and unobtrusive.
- **Game Over:** "New Best!" celebration when earned, stats grid (score / distance / stars / shards earned), and Fly Again / Shop / Menu.
- **Shop:** grid of cosmetic skins with price in shards; buy → equip. Shows owned/equipped state. Pure recolours in v1 (classic, bubblegum, matcha, lavender, peach, cloud, sunset, mint, galaxy, gold).
- **Pause:** resume / menu / mute, run frozen.
- **Settings (folded into menu/pause for v1):** sound on/off. Room to add calm mode, haptics, language later.

---

## 6. Art bible

- **Style:** soft cel-shading, thick friendly outlines, rounded everything, gentle drop shadows. Whimsical and instantly likeable.
- **Character:** round fluffy sky-blue body, oversized head, big sparkly eyes with highlights, tiny smile, pink blush cheeks, stubby limbs, two-tone propeller beanie with a spinning prop.
- **World:** pastel gradient sky, soft cloud pillars, parallax cloud and floating-island layers, faint rainbows, twinkling background stars.
- **Palette:** sky blues and lavenders, candy pink accents, warm yellow stars, mint/peach highlights. Nothing harsh or high-saturation-red.
- **Particles:** upward, light, sparkly. Star pickup = small radial twinkle burst; flap = a couple of soft motes; poof = a gentle outward puff.

---

## 7. Audio direction

- **Mood:** cozy, twinkly, major-key, low-tempo. Think music-box and soft mallets. Never tense.
- **SFX list:** flap (soft "boing"/whoosh), star collect (bright chime, pitch rises with combo), milestone (sparkle shimmer), poof/death (gentle descending "aww," not a buzzer), shard/coin (light ka-ching for shop).
- All sound is mutable and off-by-default-safe (stub ships silent; see README to add clips).

---

## 8. Accessibility

- Whole-screen tap target; no precise gestures.
- No timers in menus; nothing punishes slow reading.
- Obstacles distinguished by shape + shadow, not colour alone.
- Forgiving hitboxes; bounded difficulty so it never becomes impossible.
- Roadmap: calm mode (slower ramp), high-contrast star outline, haptic option, larger-text option.

---

## 9. Scope: v1 vs roadmap

**In v1 (ship this):** one-tap flyer, procedural pillars + sparkles, combo scoring, shard economy, 10 cosmetic skins, 6 achievements, daily reward, local save, full juice, silent-safe audio hooks.

**Parked for post-launch (don't build yet):** power-ups (shield bubble, magnet, slow-mo), multiple biomes/themes, alternate characters, weather/wind gusts, light-touch rewarded ads, leaderboards/cloud save, seasonal events. Each is designed to slot into the existing architecture without rewrites.

The discipline of this split is the single most important thing protecting the launch.
