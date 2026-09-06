# 🗺️ Zero to Published — the whole sequence, once

**The rest of this library is a dictionary. This file is the map.**

Everything here was walked end to end on a real app: **StockCut — Cut List Optimizer**, a
free offline Android tool, published to Google Play by a solo developer on a personal
account. Every ⭐⭐ and every 🔴 below is something that actually happened, not something
that might.

> **Use it like this.** Copy this file into the new project as `docs/00-plan.md`, delete
> nothing, and tick as you go. The value is not the steps you remember — it is the ones you
> don't.

---

## ⭐⭐ The real calendar — what "a few weeks" actually means

| Day | What happened |
|---|---|
| −6 | Package name checked free, keystore created and backed up |
| −5 | First run on a real phone — **two bugs no emulator had shown** |
| −4 | `app-ads.txt` published |
| **0** | **Paid $25 → account → identity verified → uploaded → published to closed testing** |
| 0 → 18 | ⏱ **Closed test: 12 testers, 14 continuous days.** Nothing shortens this |
| 18 | Applied for production access |
| 21 | Production access **granted** |
| **22** | **Public on Google Play** |
| 22 | AdMob still throttling — the app had never been linked to the listing |
| ~25 | AdMob review clears, ads serve properly |

🔴 **Three weeks minimum from paying, and the clock starts at the account, not the code.**
The code was ready six days before the account existed. **Pay the $25 early.**

---

## 🔴 The traps index — read before anything else

Each is expanded at the step where it bites. These cost real money, real days, or are
simply unrecoverable.

```
① 🔴 LOSE THE KEYSTORE        → the app can NEVER be updated. New package, zero installs.
② 🔴 versionCode ONLY GOES UP → Play remembers the highest it has EVER seen.
③ 🔴 PROMOTING THE TEST BUILD → ships test ad IDs. Earns nothing. Forever. Silently.
④ 🔴 CLICKING YOUR OWN ADS    → permanent account termination, balance forfeited.
⑤ 🔴 ADMOB NOT LINKED         → "Limited ad serving". Requests happen, revenue doesn't.
⑥ 🔴 UPLOADING DURING REVIEW  → cancels and restarts it. Adds days.
⑦ 🔴 THE EMULATOR HIDES INSETS→ gesture nav is short. Test 3-BUTTON navigation.
⑧ 🔴 THE 14-DAY TEST IS LAW   → personal accounts created after 13 Nov 2023.
```

---

# PHASE 0 · Idea and scope — before a line of code

- [ ] **Write the one sentence.** Who is it for, what does it replace, why would they trust it?
      StockCut: *a tradesman working out cut lists on the back of a delivery note.*
- [ ] **Name the ONE thing that must be correct.** Not "it should be good" — the single
      property that, if wrong, makes the app worthless.
      StockCut: *every cut accounts for saw blade width; the arithmetic never lies.*
- [ ] **Decide what it does NOT do**, in writing. StockCut is 1D linear cutting and does not
      do 2D sheet nesting. That sentence went on the store listing and prevented bad reviews.
- [ ] ⭐⭐ **Check the package name is free** — `play.google.com/store/apps/details?id=YOUR.ID`
      returning **404** means available. **Permanent once published.** Never changed, never
      reused, never recovered.
- [ ] **Decide free vs paid NOW.** A free app can never be made paid on Play. In-app
      purchases can be added later; the price cannot.

🔴 **Do not decide monetisation strategy before you have users.** StockCut planned a $4.99
unlock, built the entire paywall, then shipped free — because with zero users there was no
basis for the decision. The paywall code stayed, switched off, at no cost.

---

# PHASE 1 · Specification — the documents that pay for themselves

Write these before building. Each one prevented rework.

| Doc | What it settles | Why it earns its place |
|---|---|---|
| `01-scope.md` | In / out / never | Stops feature drift mid-build |
| `02-architecture.md` | Modules, allowed dependencies | An allowed-list stops "just one more library" |
| `03-app-flow.md` | Every screen, every state | Empty / loading / error designed, not caught |
| `04-uiux-brief.md` | Tokens, touch targets, type scale | One place to change spacing |
| `06-test-plan.md` | ⭐⭐ **The oracle set** (Phase 2) | The heart of correctness |
| `08-release-gates.md` | What blocks a release | Stops "ship it anyway" at 11pm |

- [ ] ⭐⭐ **Pick the internal representation and never deviate.** StockCut stores every
      length as a `Long` at 1/320 mm — so 1 mm = 320 units and 1 inch = 8128 units, and both
      metric and imperial are exact integers. **No floating point anywhere in the maths.** A
      measurement app that accumulates float error is worthless, and the bug is invisible.
- [ ] **Write your correctness as an equation.**
      StockCut: `Σparts + (cutCount × kerf) + offcut + trim == stockLength`, asserted in tests.
      If you cannot write it as an equation, you do not yet understand it.

---

# PHASE 2 · Edge cases and the oracle — where correctness is won

⭐⭐ **The phase most solo projects skip, and the one that separates a tool people trust
from a toy.**

- [ ] **Build an oracle set** — hand-worked examples where you know the right answer,
      computed independently of your code. StockCut has ten, `O-1` … `O-10`.
- [ ] Each oracle becomes a permanent regression test. They are never deleted.
- [ ] 🔴 **Leave a slot for a REAL job from a REAL user.** StockCut's `O-10` is still open,
      waiting for an actual tradesman's cut list. **Your invented examples share your blind
      spots.**

**The edge cases that actually bite** — a test for each, before you need it:

```
□ Zero, one, and two items        □ A part larger than any container (WARN, never drop)
□ Exact fit, no remainder         □ Every item identical
□ Quantity 1 vs 1000              □ Values at a unit-conversion boundary
□ The largest legal value         □ Mid-typing states: "1", "1 ", "1 5", "1 5/"
□ Blank input vs invalid input    □ Switching units with a value already entered
```

🔴 **Blank and invalid are NOT the same failure.** StockCut shipped a bug where a Save
button treated both as "clear the value" — so one typo silently wiped a saved setting and
then re-rendered the field blank, erasing the evidence. **Every commit/validate function
must distinguish "empty" from "wrong".**

- [ ] ⭐⭐ **Property tests for round-trips.** `parse(format(x)) == x` across the whole
      display grid. This one test class caught more than every hand-written case combined.

---

# PHASE 3 · Build

- [ ] Set `minSdk` deliberately. StockCut: 26 (Android 8). Lower means more users and more
      testing — the low-end run in Phase 4 is what makes the number honest.
- [ ] ⭐⭐ **Enable R8 from day one**, not at the end. `isMinifyEnabled` + `isShrinkResources`.
      Turning it on late means debugging obfuscation under release pressure.
- [ ] **Create the release keystore and back it up TODAY.**

🔴 **TRAP ① — the keystore.** If it is lost, the app can never be updated. Not patched, not
renamed, not fixed. The only remedy is a new package name and zero installs.

```
□ Copy to a cloud drive that is not this machine
□ Copy to a second, different place (USB stick, different cloud account)
□ Passwords into a password manager
□ Open the backup from another device to prove it is really there
```

Accepting **Play App Signing** at first upload makes a lost *upload* key recoverable — but
do the backups anyway, before you upload.

- [ ] ⭐⭐ **Make dangerous configuration opt-in, and fail loudly when it cannot be honoured.**

🔴 **TRAP ③ — the silent test-ad build.** Every StockCut build uses Google's *test* ad IDs
unless `-Pstockcut.productionAds=true` is passed. The original code **fell back** to test IDs
when a real ID was missing — producing a build that compiles, signs, uploads, installs and
**earns nothing forever, with no error in Gradle, Play, logcat or AdMob.** Now it throws:

```kotlin
fun adId(key: String, fallback: String): String {
    if (!useProductionAds) return fallback
    val id = localProps.getProperty(key)?.trim()
    if (id.isNullOrEmpty()) throw GradleException("Production ads requested but '$key' is missing")
    if (id.startsWith(TEST_PUBLISHER)) throw GradleException("'$key' is Google's TEST publisher")
    return id
}
```

**Apply the pattern to anything whose wrong value fails silently:** analytics keys, API
endpoints, feature flags.

---

# PHASE 4 · Testing — in the order that finds bugs cheapest

## 4.1 · JVM tests — milliseconds, run constantly
- [ ] Pure logic, the oracle set, property tests, every gate and limit
- [ ] StockCut: ~150 JVM tests, whole suite under a minute

## 4.2 · Instrumented tests — the critical path only
- [ ] The 6–8 journeys that must never break
- [ ] ⭐⭐ Clear app data between tests (`clearPackageData`), or they leak state into each
      other and you lose a day to a test that only fails because of the previous one

## 4.3 · 🔴 The real device — not optional

**TRAP ⑦.** StockCut passed every emulator run, then found **two bugs in ten minutes** on a
real phone — both from the same cause: the emulator used **gesture navigation** and had its
system theme matching the app's.

```
□ 3-BUTTON NAVIGATION  ← highest-value setting. The gesture pill is short enough that a
                          missing inset still looks fine.
□ SYSTEM THEME OPPOSITE THE APP'S  ← status-bar icon colour comes from the SYSTEM setting
□ The keyboard open on EVERY text field
□ Largest system font size
□ Landscape
□ Airplane mode
```

**The four inset bugs, all worth memorising:**

🔴 `Scaffold` does **not** apply window insets to an arbitrary composable in its `bottomBar`
slot. The app's primary action sat *under* the Home and Back keys — tapping its lower half
left the app. Fix: `navigationBarsPadding()`.

🔴 `enableEdgeToEdge()` picks status-bar icon colour from the **system** dark-mode setting,
not your theme. Phone dark + app light = invisible clock and battery.

🔴 `enableEdgeToEdge()` **kills `windowSoftInputMode="adjustResize"`.** The manifest line
sits there doing nothing, the IME arrives only as `WindowInsets.ime`, and the keyboard
covers your form with no scroll range. Fix: `imePadding()`.

🔴 `Modifier.padding(PaddingValues)` **applies spacing but consumes nothing**, so a
following `imePadding()` re-adds insets the padding already covered — ~120dp of dead band.
Fix: `.padding(p).consumeWindowInsets(p).imePadding()`.

## 4.4 · ⭐⭐ The low-end device — the test everyone skips

You probably do not own a 2 GB Android 9 phone. **Emulate one properly:**

```bash
sdkmanager --install "system-images/android-28/google_apis/x86_64"
avdmanager create avd -n LowEnd -k "system-images;android-28;google_apis;x86_64" -d pixel_2
# then in config.ini:
#   hw.ramSize=2048   vm.heapSize=192
#   hw.lcd.width=720  hw.lcd.height=1280   hw.lcd.density=320
```

- [ ] Install the **release** APK, not debug — R8 changes performance materially
- [ ] Cold start: `adb shell am start -W -n PKG/ACTIVITY | grep TotalTime`
- [ ] Watch for `Choreographer: Skipped N frames` and `Resources$NotFoundException`
- [ ] 720×1280 @320dpi is **360 dp wide** — narrower than most phones, and where layout breaks

⭐⭐ **Run it twice — `-gpu swiftshader_indirect` and `-gpu auto`.** StockCut measured
**2.96 s** software-rendered and **1.60 s** with GPU. The renderer, not the app. An alarming
emulator number is often the emulator.

---

# PHASE 5 · Store assets and listing — this IS the funnel

⭐⭐ With no marketing budget, the listing is the entire distribution channel.

- [ ] **App name ≤ 30 chars, containing the phrase people search.**
      `StockCut — Cut List Optimizer` = 29. **The title is the strongest ASO signal Play has.**
- [ ] 🔴 No "Free", "Best", "#1" or emoji in the title — metadata policy, common rejection
- [ ] **Short description ≤ 80 chars** — highest-weight field, and the only text shown before
      "Read more". It must carry the whole proposition alone.
- [ ] **Full description ≤ 4000** — write for a human; keywords follow
- [ ] ⭐⭐ **Use the words people actually TYPE.** StockCut initially missed "calculator" and
      "rod", both very common searches for this kind of tool.
- [ ] 5+ screenshots with captions **burned into the image**
- [ ] Feature graphic 1024×500, icon 512×512
- [ ] **Order screenshots by decision value** — most installs are decided on image one

🔴 **Encoding.** Copy listing text from a UTF-8 editor, never a terminal. Em dashes and
bullets become `â€"`, and no length check catches it. This bit StockCut twice — once inside
the length-checking tool itself.

- [ ] ⭐⭐ **Write a length checker**: character counts, hard-wrap detection, encoding. Play
      preserves line breaks, so a hard-wrapped description looks broken on a phone.

---

# PHASE 6 · Play Console — account to first upload

- [ ] Pay **$25**, create the account, complete **identity verification** (days, not minutes)
- [ ] Create the app: name, package, type **App**, **Free** (permanent)
- [ ] Upload the AAB to **Closed testing**; accept **Play App Signing**
- [ ] Store listing, icon, feature graphic, screenshots
- [ ] All App content declarations

🔴 **Declare the Advertising ID if you use an ad SDK.** AdMob puts `AD_ID` in the merged
manifest, Play scans for it, and "no advertising ID" with AdMob present is a rejection — or
a removal after the fact.

- [ ] ⭐⭐ **Verify what the AAB actually contains**, not what Gradle claims:

```bash
unzip -p app-release.aab base/manifest/AndroidManifest.xml | grep -ao 'ca-app-pub-[0-9]*'
```

Check package, `versionCode`, `versionName`, `targetSdk`, permissions, ad publisher.
**A manifest merge can add things you never wrote** — including permissions from libraries.

🔴 **TRAP ② — versionCode.** Only ever goes up, and Play remembers the highest it has **ever**
seen, including a build you uploaded and discarded. Bump for every upload, even a re-upload
of identical code after a rejection.

---

# PHASE 7 · ⏱ Closed testing — 14 days, and nothing shortens it

🔴 **TRAP ⑧.** Every **personal** account created after **13 Nov 2023** must run a closed
test with **12 testers opted in continuously for 14 days** before it may even apply for
production. Not negotiable; no amount of code changes it.

- [ ] **Over-recruit.** You need 12 — get 15. One person opting out drops the count.
- [ ] ⭐⭐ **Invited ≠ installed.** Only people who install from the opt-in link count.
- [ ] Aim for ≥3 who are genuinely your target user, not just friends
- [ ] **Ship fixes during the test.** Updating does NOT reset the 14 days, and testers
      auto-update. Freezing the app out of fear wastes the window.

⭐ App text can only change by shipping a build. **Store listing text changes need no build.**

---

# PHASE 8 · Production access

- [ ] Apply once the 14 days are complete
- [ ] ⭐⭐ **Answer with specifics** — who tested it, what they said, what changed as a
      result. Boilerplate gets bounced, and Google can cross-check against real usage data.
- [ ] Wait up to **7 days**

⭐ Production access is **per account, not per app.** Later apps skip this entirely.

---

# PHASE 9 · 🔴 Monetisation — where the money quietly does not arrive

**This phase cost a day of confusion, and would have cost far more unnoticed.**

- [ ] 🔴 **DO NOT promote the closed-test AAB.** It carries test ad IDs. Build a new one:

```bash
./gradlew clean :app:bundleRelease -Pstockcut.productionAds=true
unzip -p app-release.aab base/manifest/AndroidManifest.xml | grep -ao 'ca-app-pub-[0-9]*'
# must print YOUR publisher, never 3940256099942544
```

⭐⭐ Also grep **`classes.dex`** — banner and interstitial IDs compile into the dex, not the
manifest. Checking only the manifest checks a third of the problem.

- [ ] 🔴 **TRAP ⑤ — link the AdMob app to the store listing.** Until you do, App settings
      shows `Package name or store ID: —` and AdMob applies **"Limited ad serving"**.
      **Symptom: requests > 0 but impressions 0.** Real ads render on the device and the
      account earns nothing. You cannot do this before the listing is public.

- [ ] 🔴 **When linking, choose "Add to an EXISTING AdMob app".** The flow **defaults** to
      *"Set up as a NEW AdMob app"*, which mints **new ad unit IDs** your shipped binary does
      not use — leaving the live app on the throttled original and the new app earning nothing.

- [ ] AdMob then reviews the app: **2–3 days, serving limited throughout.** Near-zero revenue
      in that window is expected, not a bug.
- [ ] Set the **ad content rating** (Blocking controls). The default is **MA**, which permits
      alcohol, weapons and sexual content. **T** excludes all three.
- [ ] `app-ads.txt` must resolve at a **domain root** — a subdirectory does not count
- [ ] Set up the payment profile early: the address PIN arrives **by post** at $10; payout at $100

🔴 **TRAP ④ — never tap an ad in your own app, and tell your testers.** Clicks from people
who know the developer are exactly what invalid-traffic enforcement looks for. It is
permanent and forfeits the balance. One accidental tap is noise; a pattern is fatal. Send
the message explicitly — friends "helping" is the likeliest cause.

---

# PHASE 10 · Launch

- [ ] Production → Create release → upload → **staged rollout 20%** if offered

⭐ Play often does **not** offer staged rollout on a *first* production release — it goes
straight to 100%. Plan the first 48 hours accordingly.

- [ ] Verify it is genuinely public:
      `curl -o /dev/null -w "%{http_code}" "<store URL>"` → **200**. Unknown package = 404.
- [ ] Watch **Android vitals**: crash < **1.09%**, ANR < **0.47%**. Crossing either demotes
      the listing in search and recommendations.
- [ ] Check **AdMob impressions** — but only after the AdMob review clears

🔴 **TRAP ⑥ — one review at a time.** Uploading anything while a submission is in review
**cancels and restarts it.** Store listing text also goes through review. Batch your
changes and submit once.

⭐ Only **one draft release per track**. If "Create new release" is greyed out, an old draft
exists — edit it, and remove any stale bundle inside it first.

⭐ **Open testing is not a public launch.** It is a wider beta; Play labels those builds
"(Beta)" and the label persists for enrolled accounts until they leave the programme.

---

# PHASE 11 · After launch — discoverability and comprehension

**The two questions that decide whether the app is used, not merely shipped.**

## 11.1 · Will anyone find it?

⭐⭐ **Honest answer for a new app: not for months.** Play ranks on installs, retention,
**ratings** and engagement. Keyword relevance is a small part — and the part you have
already done.

- [ ] ⭐⭐ **Ask your testers for ratings.** Zero ratings is the single biggest handicap and
      the only ranking signal you can legitimately influence on day one. Real users, real
      opinions — **never tell them what to write.**
- [ ] Expect exact-name search to work in ~1–2 weeks; competitive terms take far longer
- [ ] Add the in-app review prompt after a **success** moment — never on launch, never after
      a crash

## 11.2 · Will they understand it in 30 seconds?

- [ ] ⭐⭐ **Seed a worked example on first run.** StockCut ships an "Example: gate frame"
      job, so a new user learns by opening something that already works instead of facing an
      empty screen. **The highest-leverage onboarding decision in the app.**
- [ ] Every empty state: **headline, one line of explanation, exactly one action**
- [ ] Make the payoff reachable in **two taps** from launch
- [ ] 🔴 **Label your units.** StockCut rendered every metric length as a bare number —
      `1800`, not `1800 mm`, and in metre mode just `3`. Obvious to someone who knows the
      job; a puzzle to someone opening the app for the first time.
- [ ] ⭐⭐ **Separate display formatting from input formatting.** The function that fills a
      text field must round-trip through your parser; the one that renders a label need not.
      Keep them as two functions, or you widen a parsing invariant by accident.

## 11.3 · Ongoing

```
□ Vitals weekly                  □ AdMob impressions, app-ads.txt verified
□ Reply to reviews               □ Collect one REAL user job → becomes oracle O-10
```

---

# PHASE 12 · Store recommendations — read them, don't obey them

Play's Release dashboard raises "recommended actions". ⭐⭐ **Some are wrong for your app,
and two of StockCut's contradicted each other.**

| Recommendation | Verdict |
|---|---|
| "Outdated SDK version of X" | ✅ **Real.** Force it forward with a dependency **constraint** — R8 then strips it entirely if nothing uses it |
| "Edge-to-edge may not display" | ⚪ Generic advisory to every targetSdk 35+ app. Its own text says *"alternatively, call enableEdgeToEdge()"* — which the app already did |
| "Deprecated APIs for edge-to-edge" | ❌ **Not our code.** `setStatusBarColor`, `setNavigationBarColor` and `SHORT_EDGES` are what `androidx.activity`'s own `enableEdgeToEdge()` calls on its pre-API-35 path. Acting on it breaks Android 8–14 |
| "Optimised resource shrinking isn't enabled" | ✅ Real, worth ~2.3%. But it is **experimental** — verify on a device, because a stripped ad resource fails **silently** |
| "Upgrade AGP to 9.0" | ⏸ Deferred. Major version, breaking changes, its own test pass. A recommendation, not a requirement |

⭐⭐ **Method: grep your own source before believing a warning.** If your code has zero
matches for the flagged API, it is a library's call site and not yours to fix. **Write the
verdict down** — these reappear on every single release, and re-deriving the answer each
time is pure waste.

---

# Also worth knowing

- **Android developer verification** (announced 15 Jul 2026): package name and signing keys
  must be registered against a verified identity or the app is removed from Play. For an app
  created through Play Console with identity verification already done, this is registered
  **automatically** — check the status, and do **not** hit "Register package name", which is
  for apps distributed outside Play.
- **Pre-launch reports do not persist.** They are generated when you upload to a testing
  track and are gone later. If you want one, upload to **Internal testing** — it goes live in
  minutes with no review queue.
- **A BILLING permission makes Play print "In-app purchases"** on the listing even when
  nothing is for sale. If a dormant billing library is present, strip the permission at merge
  time with `tools:node="remove"` rather than unpicking the code.

---

# The 90-second version

```
BEFORE CODE   package name free · keystore backed up twice · write the one equation
BUILD         R8 on from day one · dangerous config opt-in and FAILS LOUD
TEST          oracle set · property tests · REAL PHONE 3-BUTTON NAV · low-end 2GB emulator
STORE         30-char title with the searched phrase · 80-char short desc · UTF-8 editor
ACCOUNT       $25 → identity → upload → 12 testers × 14 days → apply → 7 days
MONEY         rebuild with real ad IDs · grep the DEX · LINK ADMOB TO THE LISTING
LAUNCH        verify HTTP 200 · vitals 1.09% / 0.47% · one review at a time
AFTER         ask for ratings · seed a worked example · label your units
```

**The three that are unrecoverable: the keystore, the package name, and your AdMob account.**
Everything else is a bad afternoon.
