# 📝 Play Store Templates — the text you need, in order

> ⭐ **What each asset is for, and what makes it good:**
> [Screen-Inventory.md §1](Screen-Inventory.md). This file is the text itself.

**Fill-in-the-blank versions of every piece of writing a Play launch demands.**
Companion to ⭐⭐ [14-Zero-to-Published.md](../14-Zero-to-Published.md), which is the sequence.

Each was written for a real launch and survived review. `[SQUARE BRACKETS]` = replace.

> 🔴 **Edit these in a UTF-8 editor (VS Code), never a terminal.** They contain em dashes
> (—) and bullets (•). A tool reading them as Windows ANSI turns each into two junk
> characters, and no length check will catch it.

---

## 1 · Store listing

### App name — max 30 characters

```
[BRAND] — [WHAT IT IS]
```

**Rules that get apps rejected:** no "Free", "Best", "#1", no emoji, no ranking claims.

⭐⭐ **The second half must be the phrase people search**, not a clever name. "StockCut" is
unsearchable; "Cut List Optimizer" is what a tradesman types. You need both, and you have
30 characters for the pair.

### Short description — max 80 characters

**The highest-weight ASO field, and the only text shown before "Read more".** It carries
the whole proposition alone.

```
[WHAT IT DOES] for [MATERIALS/DOMAIN]. [CREDIBILITY WORD], [OBJECTION 1], [OBJECTION 2].
```

Worked example (74/80):
```
Cutting calculator and cut list optimizer for metal, timber, pipe and rod.
```

**The formula:** the searched phrase, the specific nouns your user thinks in, and the two
objections that stop installs on utility apps — for StockCut, "offline" and "no sign-up".

### Full description — max 4000 characters

```
[ONE LINE THAT NAMES THE PAINFUL STATUS QUO]

[TWO SENTENCES: what it does, and the one detail that proves you know the job]

WHAT IT DOES
• [Benefit, not feature]
• [The credibility detail — the thing only a practitioner would think of]
• [What it shows you]
• [What it warns you about, instead of failing quietly]

[VARIANT/UNITS SECTION — if your users are split across systems, say so explicitly]

BUILT FOR [THE REAL CONTEXT, NOT THE OFFICE]
• Works with no signal
• No account, no sign-up, no password
• Your data stays on your device
• [Accessibility/environment detail — large type, gloves, sunlight, noise]

WHO IT IS FOR
[Every job title you can name], and anyone [the defining activity].
Use it as a [SEARCH TERM] for [MATERIAL], [MATERIAL] or [MATERIAL].

HOW IT WORKS
1. [Step]  2. [Step]  3. [Step]  4. [The payoff]

[PRICE SECTION — state it plainly, including how ads behave]

WHAT IT IS NOT
[The adjacent thing people will assume it does. Naming it prevents bad reviews.]
```

⭐⭐ **"WHO IT IS FOR" is doing double duty** — it reassures a human *and* carries the long
tail of search terms naturally. List the job titles and the materials.

⭐⭐ **"WHAT IT IS NOT" is the highest-value paragraph in the listing.** One sentence naming
the adjacent thing you *don't* do prevents the one-star review that says "doesn't work".

---

## 2 · Release notes

### First production release
```
[APP] [does the core thing] with [the key quality]. Works fully offline.
```

### Bug-fix release
```
Fixes [the user-visible symptom, in their words]. [Second fix, same style].
```
⭐ Describe the **symptom**, not the cause. "Fixes a white flash when opening in dark mode",
never "adds values-night theme".

### Release that changes something on the listing
```
Removes an incorrect "[LABEL]" from the store listing. [APP] is free and always has been.
```

### Internal-only release (no user-visible change)
```
Internal improvements to app size and dependencies.
```

---

## 3 · Recruiting the 12 testers

🔴 You need **12 opted in and installed for 14 continuous days**. Ask **15**.

```
I've built [APP] — [one sentence on what it does].

Before Google will let me publish it publicly, I need 12 people to install it and keep
it for two weeks. That's a Google rule, not a favour I'm asking for feedback.

Two things:
1. Tap this link and install from it — [LINK]. Installing from anywhere else doesn't count.
2. Please keep it installed for two weeks. If you uninstall, the count drops and the
   clock effectively restarts.

Use it if it's useful. Tell me if something's broken.
```

⭐⭐ **"Invited" does not count.** Only people who actually install from the opt-in link do.
Chase the installs, not the acceptances.

---

## 4 · 🔴 The ad-safety message — send it the day you go live

**The single most important message in this file.** Clicks from people who know the
developer are exactly what invalid-traffic enforcement looks for, and termination is
permanent with the balance forfeited.

```
[APP] is live on the Play Store now — thanks for testing it. Keep using it normally,
that's what helps most.

One thing: please don't tap the ads. Google treats clicks from people who know the
developer as invalid traffic, and repeated ones can get my ad account banned
permanently. Just ignore them.
```

⭐ **Do not tell them to uninstall.** Uninstalls cost you real installs and retention to
avoid a hypothetical. One accidental tap is noise; a *pattern* is fatal.

---

## 5 · Asking for ratings

⭐⭐ **Zero ratings is the biggest ranking handicap a new app has, and the only signal you
can legitimately influence on day one.**

```
If [APP] has been useful, would you mind leaving a rating on the Play Store?
It genuinely helps people find it. [LINK]
```

🔴 **Never suggest what to write or what score to give.** Review manipulation is a
suspension offence, and it is also the fastest way to make honest feedback useless to you.

---

## 6 · Production access application

Google asks three things. **Specifics pass; boilerplate gets bounced.** They can
cross-check every claim against real usage data.

### "Who is the intended audience?" (~300 chars)
```
[JOB TITLES] who [THE DEFINING ACTIVITY]. They work [WHERE], often [CONSTRAINT — no signal,
gloves, noise], and need [THE OUTCOME] before [THE MOMENT IT MATTERS].
```

### "How does your app provide value?" (~300 chars)
```
[THE STATUS QUO IT REPLACES]. [APP] does [THE CORE COMPUTATION], accounting for [THE
DETAIL PRACTITIONERS CARE ABOUT], and shows [THE OUTPUT] so [WHO] can [ACT ON IT].
```
⭐⭐ **Do not overclaim.** If your algorithm is a heuristic, say "as little waste as it can
find", not "the least possible". The claim must match your own code comments and your
store listing — and one day it may have to match a review.

### "What changed based on the closed test?" (~300 chars)
```
[SPECIFIC CHANGE], after [WHO] reported [WHAT]. [SECOND CHANGE]. Shipped as [VERSION]
during the test.
```

### "How did you decide it is ready?" (~300 chars)
```
[N] automated tests, [METRIC] on [REAL DEVICE], no crashes or ANRs across [N] testers over
[N] days. [THE HARDEST THING YOU VERIFIED, and how].
```

🔴 **Use real numbers you can defend.** If you can't produce the evidence, don't write it.

---

## 7 · Data safety — the form that gets apps removed

⭐⭐ **Every answer must match your merged manifest and your privacy policy.** A mismatch is
the most common rejection, and it is also a removal risk *after* publication.

```
□ Unzip the AAB and list the ACTUAL permissions — libraries add their own
□ Any ad SDK → you almost certainly collect the ADVERTISING ID. Declare it.
□ Crash reporting → declare diagnostics
□ Does the privacy policy say the same thing? Read it again, line by line
□ Does it mention the platform BACKUP? "Never leaves your device" is false if
  android:allowBackup is true — that is a copy on the user's cloud drive
□ Is there an in-app route to delete data, and does the policy describe it?
```

🔴 **Write the privacy policy last, from the verified manifest** — not first, from intent.
StockCut's claimed "never leaves your device" while Android auto-backup was enabled, and
listed a billing row for a product that was never sold.

---

## 8 · The pre-upload verification block

Paste into the project's release runbook. **Run every time.**

```bash
AAB=app/build/outputs/bundle/release/app-release.aab

# Identity — what actually shipped, not what Gradle claims
unzip -p $AAB base/manifest/AndroidManifest.xml | strings | grep -E 'com\.your\.package'

# Permissions — including ones libraries added for you
unzip -p $AAB base/manifest/AndroidManifest.xml | grep -ao '[a-z.]*permission\.[A-Z_]*' | sort -u

# 🔴 Ad IDs — check EVERY entry, not just the manifest.
#    Banner and interstitial IDs live in classes.dex.
for f in $(unzip -Z1 $AAB); do
  unzip -p $AAB "$f" 2>/dev/null | grep -ao 'ca-app-pub-[0-9]*' | sed "s|^|$f: |"
done | sort -u

# Signature
jarsigner -verify $AAB
```

**Expected:** your package, only permissions you can explain, **your** ad publisher and
**never** `3940256099942544`, and `jar verified`.
