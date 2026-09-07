# 📱 Screen Inventory — everything a complete app actually has

> ⭐⭐ **Use this at step ① and again at ⑯.** At ① it stops you forgetting a whole surface;
> at ⑯ it is the "what is actually missing" list. Most half-finished apps are not missing
> features — they are missing **screens nobody wrote down**.

**Columns:** ⭐⭐ = you will regret not having it · ⭐ = expected by users · (blank) = only if it
applies. **STORE** = the review process checks it. **LAW** = legally required somewhere you serve.

---

# 0 · ⭐⭐ Does this follow the web inventory? Partly.

> If you have read [`03-Web-Developer/Reference/Page-Inventory.md`](../../03-Web-Developer/Reference/Page-Inventory.md),
> **four surfaces carry over almost unchanged and three are completely different.**

```
⭐ THE SAME, MORE OR LESS:
   · the auth surface        · the product surface
   · settings                · legal (different enforcer, same content)

⭐⭐ COMPLETELY DIFFERENT:

 ① THE PUBLIC SURFACE IS NOT IN YOUR APP.
    Web has a hero, an about page, pricing, FAQ. ⭐ A mobile app has
    NONE of that — the STORE LISTING is your entire marketing surface,
    and it is made of screenshots and 170 characters, not pages.
    ⇒ §1 replaces the whole web "public surface".

 ② SYSTEM PAGES BECOME LIFECYCLE SCREENS.
    ⭐ There is no 404 and no 500. Instead you need an offline screen,
    a force-update screen, and a "this no longer exists" screen for
    someone arriving from a push or a deep link. ⇒ §6.

 ③ THE INVISIBLE LAYER IS BUILD CONFIG, NOT META TAGS.
    No <title>, no sitemap, no Open Graph. Instead: an icon set, a
    launch screen, permission usage strings, deep-link config, and
    the store privacy form. ⇒ §8.

⭐⭐ AND ONE SURFACE THE WEB DOES NOT HAVE AT ALL:
    PERMISSION PRIMING (§2). The OS dialog is one tap and it is
    PERMANENT if they say no. You need a screen before it.
```

---

# 1 · ⭐⭐ The store listing — your only marketing surface

> **You get about three seconds in a search result, and you cannot A/B test your way out of a
> bad first screenshot.** This is the mobile equivalent of the whole web landing page, and it
> is usually written the night before submission.

| Asset | | What it is for | ⭐ The common mistake |
|---|---|---|---|
| **App name** | ⭐⭐ STORE | Searchable, and it is what sits under the icon | Clever instead of findable. ⭐ Nobody searches your brand name — they search the problem |
| **Subtitle / short description** | ⭐⭐ STORE | The one line under the name in results | Repeating the app name. Use it to say what it does |
| **⭐⭐ First screenshot** | ⭐⭐ STORE | **Seen more than everything else combined.** Most people never swipe | A login screen. Show the app **doing the thing**, with a caption |
| **Screenshots 2–5** | ⭐ STORE | The core loop, in order | Raw screenshots with no caption. Add one line of text to each |
| **Preview video** | | Autoplays in results on iOS | Making it 30 seconds. The first 3 decide everything |
| **Long description** | ⭐ STORE | ⭐ Only the first 2–3 lines are read before "more" | Burying what it does under a paragraph of adjectives |
| **Keywords** (iOS) | ⭐ STORE | 100 characters, comma-separated, no spaces | Wasting them on your own app name — it is already indexed |
| **App icon** | ⭐⭐ STORE | The thing they tap for months | ⭐ Text in the icon. It is unreadable at 60px |
| **Category + age rating** | ⭐⭐ STORE | | A rating that does not match your content — that is a rejection |
| **⭐ Demo account** | ⭐⭐ STORE | For the reviewer, if anything is behind login | ⭐⭐ Giving one that does not work. **This is a top rejection reason** and it costs a full review cycle |
| **What's New** | ⭐ | Every release | "Bug fixes and improvements" forever |

```
⭐⭐ THE TEST: show a stranger ONLY your icon, name, subtitle and first
   screenshot for three seconds.
   ⇒ Can they say what it does? If not, that is why installs are low —
     nothing inside the app can fix a listing nobody stops at.
```

⭐ **This table is the inventory — what each asset is for and what goes wrong.**
For the **actual text to write**, fill-in-the-blank:
[Play-Store-Templates.md](Play-Store-Templates.md).

---

# 2 · ⭐⭐ First run — the surface web does not have

> **The app opens and must explain itself.** There is no landing page, no about page, nothing
> the user read before installing except a screenshot.

| Screen | | What it needs | ⭐ The common mistake |
|---|---|---|---|
| **Launch / splash** | ⭐⭐ | ⭐ Fast. It is a launch screen, not a brand moment | An animated logo that adds two seconds to every cold start |
| **Onboarding** | ⭐ | ⭐⭐ Get them to the core action, not through a tour | 4 swipe-through slides nobody reads. **Skippable, or skip it entirely** |
| **⭐⭐ Permission priming** | ⭐⭐ | A screen BEFORE the OS dialog that explains **why**, with a "not now" | ⭐⭐ Triggering the OS prompt cold on launch. **If they deny, it is permanent** — they must go to Settings, and they never will |
| **Value-first state** | ⭐ | ⭐ Can they do anything before signing up? | A signup wall on screen one. On mobile that is an uninstall |
| **Empty first state** | ⭐⭐ | The account exists and has nothing in it — with the button that creates the first thing | Showing the same "no results" screen as a filtered-to-zero list |

```
⭐⭐ THE PERMISSION RULE, WHICH HAS NO WEB EQUIVALENT:

  ❌ App opens → OS asks for notifications → user taps Don't Allow
     ⇒ ⭐ THAT IS PERMANENT. The feature is dead for that user forever.

  ✅ App opens → user does something where the permission makes sense
     → YOUR screen explains why → they tap "Enable" → THEN the OS asks
     ⇒ ⭐ And if they say "not now", you can ask again later.

  ⭐ Ask at the moment of use. Never on launch. Always with a reason.
```

---

# 3 · The auth surface — same eight as web, with mobile twists

| Screen | | Mobile-specific note |
|---|---|---|
| **Sign up** | ⭐⭐ | ⭐ Sign in with Apple is **required** if you offer any other social login |
| **Log in** | ⭐⭐ | ⭐ Redirect back to the deep link they came from, after a cold start |
| **Forgot / reset password** | ⭐⭐ | The reset link opens in a browser then must return to the app — ⭐ test this whole loop, it breaks constantly |
| **Verify email** | ⭐ | Needs a resend. The first one lands in spam and they are stuck |
| **OAuth / magic link return** | ⭐ | ⭐ Returning to the app from Safari/Chrome, including from a cold start |
| **Biometric unlock** | | ⭐ For re-auth only, never as the only auth. Always a fallback |
| **Logout** | ⭐⭐ | ⭐⭐ Clears secure store, the query cache, files AND downloaded images |

---

# 4 · The product surface — closest to web

| Screen | | ⭐ The mobile difference |
|---|---|---|
| **Home / main** | ⭐⭐ | Answers "what now?" in one screen with no hover and no sidebar |
| **The core loop screen** | ⭐⭐ | ⭐ Reachable in ≤ 3 taps from cold start |
| **List views** | ⭐⭐ | ⭐⭐ Virtualised, pull-to-refresh, and **both** empty states |
| **Detail view** | ⭐⭐ | ⭐ Must work when opened cold from a push or a deep link, with no back stack |
| **Create / edit** | ⭐⭐ | ⭐⭐ Draft saved **as they type** — Android kills backgrounded apps freely |
| **Delete / destructive** | ⭐⭐ | Confirmation proportional to damage; undo where possible |
| **Search** | ⭐ | A no-results state that suggests what to do |
| **Camera / picker / upload** | | ⭐ The denied-permission path, and the "no photos at all" path |
| **Notification inbox** | | Only if push actually matters to this product |

---

# 5 · Settings — same as web, plus four mobile-only

```
⭐ SAME AS WEB: profile · change email · change password · sessions ·
  connected accounts · billing · team · export · delete account

⭐⭐ MOBILE-ONLY ADDITIONS:

  □ ⭐⭐ NOTIFICATION SETTINGS, plus a deep link to the OS settings
     screen — because you cannot re-ask for a permanently denied
     permission from inside the app
  □ ⭐ PERMISSIONS STATUS — show what is granted, with a way to fix it
  □ ⭐ OFFLINE / DOWNLOADED CONTENT — what is cached, and clear it
  □ ⭐ APP VERSION + BUILD NUMBER, visible. ⭐⭐ You will ask a user for
     this on your first support ticket, and without it you are guessing
  □ Open-source licences screen (⭐ many libraries require it)
  □ Restore purchases (⭐⭐ Apple REQUIRES this if you sell anything)

⭐⭐ ACCOUNT DELETION IS STRICTER THAN WEB:
   Apple requires it to be IN-APP and reachable — not "email us", not
   "log in on the website". If users can create an account in your app,
   they must be able to delete it in your app.
```

---

# 6 · ⭐⭐ Failure and lifecycle screens — replaces web's system pages

> **There is no 404 and no 500.** These are what you need instead, and every one of them is
> a screen people will actually hit.

| Screen | | Why it exists |
|---|---|---|
| **⭐⭐ Offline** | ⭐⭐ | Per screen. ⭐ An infinite spinner on a lost connection is the single most common mobile bug |
| **Error with retry** | ⭐⭐ | Branch by cause: 401 ≠ 404 ≠ 500 ≠ offline. ⭐ Never clear the form |
| **⭐⭐ Force update** | ⭐⭐ | **Your only kill switch.** A broken version is live for 1–3 days while review runs — this screen is how you stop it. ⭐ Build it in v1, not when you need it |
| **Maintenance / service down** | ⭐ | Better than every screen failing separately |
| **"This no longer exists"** | ⭐ | ⭐⭐ Someone taps a push or a deep link for something deleted. This is the mobile 404 |
| **No permission** | ⭐ | Explains why, and links to Settings |
| **⭐ App-switcher snapshot** | ⭐⭐ | A screen you never designed. ⭐⭐ The OS screenshots your app when backgrounded — **blur it if anything sensitive is on screen** |
| **Session expired** | ⭐ | Re-auth without losing what they were doing |

```
⭐⭐ THE FORCE-UPDATE SCREEN IS NOT OPTIONAL, AND IT IS THE ONE PEOPLE
   SKIP. Web can roll back in three minutes. You cannot.
   ⇒ ⭐ A remote flag your app checks on launch, and a screen that says
     "please update" with a store link. Test it ONCE, before you need
     it at 2am.
```

---

# 7 · Legal and store compliance

| Item | | Note |
|---|---|---|
| **Privacy policy** | ⭐⭐ LAW STORE | ⭐ URL must be live and reachable — **a 404 here is a rejection** |
| **Terms / EULA** | ⭐⭐ STORE | |
| **⭐⭐ Data safety / privacy nutrition form** | ⭐⭐ STORE | Must match what the app **actually** collects, ⭐ including what your SDKs collect without you asking |
| **In-app account deletion** | ⭐⭐ LAW STORE | See §5 |
| **Permission usage strings** | ⭐⭐ STORE | ⭐ "To scan the barcode on your receipt" — not "This app needs camera access". The second is a rejection |
| **UGC: report · block · filter · contact** | ⭐⭐ STORE | ⭐⭐ If users can post anything, **Apple requires all four**. Not three |
| **Correct payment rail** | ⭐⭐ STORE | ⭐ Digital ⇒ IAP. Physical ⇒ not IAP. Backwards is an automatic rejection |
| **Open-source licences** | ⭐ | A settings screen listing them |

---

# 8 · The invisible layer — build config, not meta tags

```
⭐⭐ NONE OF THIS IS A SCREEN, AND ALL OF IT WILL BLOCK YOU.

  □ ⭐ APP ICON in every required size, and it reads at 60px
  □ Launch screen matching the first real screen (⭐ so it does not
     flash a different colour)
  □ ⭐⭐ PERMISSION USAGE STRINGS in Info.plist / AndroidManifest —
     every permission you request, explained
  □ ⭐ DEEP LINKS CONFIGURED: universal links (iOS) + app links
     (Android), verified, and tested FROM A COLD START
  □ Push credentials (APNs key, FCM) — ⭐ and tested on a real build,
     not the simulator
  □ ⭐⭐ APP SIGNING set up, and the upload key BACKED UP somewhere you
     will still have in two years. Lose it and you cannot update.
  □ Target SDK / minimum OS versions decided deliberately
  □ Bundle ID / package name — ⭐ permanent, choose carefully
  □ Version + build number strategy
  □ ⭐ Sentry with NATIVE SYMBOLICATION, and a real crash tested
  □ ⭐⭐ OTA (EAS Update) path tested end to end BEFORE you need it
  □ ⭐ SPF, DKIM and DMARC if the backend sends any email
```

---

# ⭐⭐ What to cut for v1

```
⭐⭐ SHIP WITHOUT THESE — nobody will notice on day one:
   onboarding slides · notification inbox · biometric unlock ·
   offline downloads · team/multiplayer · 2FA · preview video ·
   deep links (unless sharing IS the product) · dark mode

⭐ SHIP WITHOUT THESE ONLY IF YOU ADD THEM SOON:
   search · export · connected accounts · What's New content

⭐⭐ YOU CANNOT SHIP WITHOUT THESE:
   a first screenshot that shows the app working · a demo account
   that works · the full auth surface including the reset loop ·
   the core loop in ≤3 taps · every list with BOTH empty states ·
   an offline state on every networked screen ·
   ⭐⭐ THE FORCE-UPDATE SCREEN · permission priming before every OS
   prompt · in-app account deletion · privacy policy live ·
   the data-safety form matching reality · the correct payment rail ·
   everything in §8

⭐ THE TEST: what breaks for a real user if this does not exist on
  launch day? If the honest answer is "nothing", it is not v1.
  ⭐⭐ EXCEPT force-update — that one breaks nothing until the day it
    is the only thing standing between you and a week of bad reviews.
```

---

**Back:** [folder index](../README.md) · **The workflow:** [PROMPTS.md](../PROMPTS.md) ·
**The store text:** [Play-Store-Templates.md](Play-Store-Templates.md) · **One real launch, end to end:**
[14-Zero-to-Published.md](../14-Zero-to-Published.md) ·
**Security:** [05-Security.md](../05-Security.md) · **Legal:**
[12-Legal-and-Compliance.md](../12-Legal-and-Compliance.md) · **Launch:**
[10-Ship-Checklist.md](../10-Ship-Checklist.md) · **Web equivalent:**
[`03-Web-Developer/Reference/Page-Inventory.md`](../../03-Web-Developer/Reference/Page-Inventory.md)
