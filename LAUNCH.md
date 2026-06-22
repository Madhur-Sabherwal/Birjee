# Birje — Launch & Post-Launch Strategy

Realistic for a solo developer doing this on the side, in Australia, with little or no budget.

---

## 1. Beta testing plan

- **Round 1 — couch test (week 1):** hand your phone to 5–10 friends/family, *say nothing*, and watch. Where do they hesitate? Do they understand "tap to flap" instantly? Is the first death fair? This catches more than any analytics would.
- **Round 2 — closed beta:** Android **Internal/Closed testing** + iOS **TestFlight**. Aim for 15–30 testers. Ask three questions only: *Was it fun? Did anything feel unfair? Would you play again?*
- **Round 3 — small public beta:** Play **Open testing** + external TestFlight. Post in a couple of friendly indie/casual-game communities asking specifically for feedback (not installs-for-installs).
- **What to tune from feedback:** difficulty ramp (start too hard?), gap floor, combo payoff, shard prices (skins reachable but not instant), and whether the daily reward pulls people back.

---

## 2. ASO basics (App Store Optimization)

- **Icon is ~60% of the battle.** A clear, charming Birje face on a clean pastel background, readable at thumbnail size. A/B nothing else until the icon pops.
- **Title/subtitle:** "Birje" + "Hop, Flap, Sparkle!". The tagline carries casual keywords (flap, sparkle) naturally.
- **Keywords (iOS keyword field / Android via description):** casual, one tap, flappy-style flyer, cute, relaxing, offline, no wifi, sparkle, sky, kids-friendly, arcade, high score. Use them in natural sentences for Play; use the dedicated field for iOS.
- **Screenshots:** first two must sell the fantasy — gameplay mid-sparkle-burst and a big combo. Add 4–6 word captions ("Tap to fly!", "Collect sparkles!", "Unlock cute looks!", "No ads. No sign-in.").
- **"No ads, no sign-in, plays offline"** is a genuine differentiator in casual — say it plainly.

---

## 3. No/low-budget marketing

- **Short vertical clips** (15–20s) of a clean run with a near-miss and a sparkle chain. Post to TikTok / YouTube Shorts / Instagram Reels / a relevant subreddit. One good loop can do more than a hundred dollars of ads.
- **Devlog angle:** "I made a cozy game in Flutter" stories do well in indie-dev and Flutter communities; share a build-in-public thread, screenshots, and the launch.
- **Friendly communities:** casual/cozy-game subreddits, indie Discords, Flutter/Flame channels (show the procedural art), local Perth/Aus indie dev groups. Contribute, don't spam; share when there's something to look at.
- **Launch-day kit:** one tweet/thread, one short clip, both store links, a one-line pitch, and a "made by one person" note (people root for solo devs).
- **Press is mostly not worth chasing** for a tiny casual title — your time is better spent on clips and iteration. A polished press one-pager is cheap to keep on hand though.

---

## 4. Privacy policy

Birje v1 collects nothing, so the policy is short. A ready-to-host template is in `PRIVACY_POLICY.md` — put it on any free static page (GitHub Pages works) and paste the URL into both store consoles.

---

## 5. Post-launch: monitor, update, and *when* to add things

- **Watch:** crash-free rate (Play Console / Xcode Organizer — both available without adding any SDK), ratings, and the actual words in reviews. Reviews are your roadmap.
- **First patch (within ~2 weeks):** fix any crash, and tune difficulty/economy from real feedback. Nothing big — just make the core feel right.
- **Then, one addition at a time** from the parked roadmap (see GDD): e.g. first a **power-up** (magnet or shield), later a **second biome**, later **more skins**. Ship small and often; each update is a re-engagement and ASO refresh opportunity.
- **Ads — only if/when you choose to, and gently:** rewarded-only (optional bonus shards / one continue), never interstitials mid-run, never banners over gameplay. If you add them, update Data Safety/App Privacy declarations and, for kids contexts, use certified child-safe ad SDKs. Core game stays free and complete.
- **Leaderboards/cloud save:** only if retention justifies the added privacy/back-end surface. Architecture leaves room (`SaveManager`), but it's firmly post-v1.

---

## 6. Realistic timeline (solo, part-time)

- **Week 0:** polish pass, icon + screenshots, privacy page hosted.
- **Week 1:** internal/TestFlight builds, couch testing, fix obvious issues.
- **Weeks 2–3:** closed/open beta, tune difficulty & economy.
- **Week 4:** submit both stores (build a couple of days of review buffer).
- **Launch week:** post clips + devlog, link both stores.
- **Weeks 5–6:** first patch from reviews.
- **Ongoing:** one small content/feature update every few weeks from the parked list.

**Prioritization rule:** anything that doesn't make the *first run* more delightful gets parked. Ship the cozy core, then grow it.
