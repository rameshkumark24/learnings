# 🗂️ Page Inventory — everything a complete web app actually has

> ⭐⭐ **Use this at step ① and again at ⑯.** At ① it stops you forgetting a whole surface;
> at ⑯ it is the "what is actually missing" list. Most half-finished web apps are not missing
> features — they are missing **pages nobody wrote down**.

**How to read the columns:** ⭐⭐ = you will regret not having it · ⭐ = expected by users ·
(blank) = only if it applies. **LAW** = legally required in at least one jurisdiction you
probably serve. **SEO** = affects whether search can find you.

---

# 1 · The public surface — what a stranger sees

> **This is the surface that has to earn the click.** It is also the only surface search engines
> can see, so every page here should be static or server-rendered.

| Page / section | | What it is actually for | ⭐ The common mistake |
|---|---|---|---|
| **Hero** | ⭐⭐ SEO | The one sentence + the one action. See [PROMPTS ⑦b](../PROMPTS.md) | A slogan instead of a sentence. "Empower your workflow" tells nobody anything |
| **The proof strip** | ⭐ | Logos, numbers, a real quote — credibility in 2 seconds | ⭐⭐ Fake logos or invented testimonials. People can tell, and it costs trust you cannot buy back. **Show nothing rather than something false** |
| **The problem** | ⭐ | Name the pain in the user's words, so they self-identify | Describing your product instead of their problem |
| **How it works** | ⭐⭐ | Three steps, with a real screenshot at each | ⭐ Abstract illustrations. Show the actual interface |
| **Features** | ⭐ | What it does, grouped by what the user is trying to achieve | A feature grid with emoji icons. That is the single clearest template tell |
| **Use cases / who it's for** | | Lets different visitors see themselves | Writing one for an audience you do not actually serve |
| **Pricing** | ⭐⭐ SEO | Often the second page anyone visits, straight from the nav | ⭐⭐ Hiding it behind "contact us" — for a self-serve product that is a bounce. And: no annual toggle, no "what happens when I hit the limit" |
| **FAQ** | ⭐ SEO | Answers the objections that stop a signup | Questions nobody asked. ⭐ Write the ones support actually gets |
| **Comparison / alternatives** | SEO | ⭐ Real search traffic — people search "X vs Y" and "alternative to X" | Being dishonest about competitors. It is obvious and it backfires |
| **About** | ⭐ | ⭐⭐ Who is behind this. On web, trust is the whole barrier for an unknown product | A mission statement. Say **who you are**, where you are, and why this exists |
| **Contact** | ⭐⭐ LAW | A real way to reach a human | ⭐ A form with no address and no email. Several jurisdictions require a real contact route, and users trust a `mailto:` more than a form |
| **Blog / guides** | SEO | The compounding channel — see [PROMPTS ⑮](../PROMPTS.md) | Starting one you will abandon. ⭐ Three real guides beat thirty thin posts |
| **Changelog** | | Proof the thing is alive | Letting it go stale — a dead changelog is worse than none |
| **Footer** | ⭐⭐ | Where legal, contact, social and secondary nav live | ⭐ Dead links. The footer is the #1 place broken links hide |

```
⭐⭐ THE HONEST TEST FOR THIS WHOLE SURFACE:
   Can a stranger answer "what is it, who is it for, what does it
   cost, and can I trust it?" WITHOUT signing up?
   ⇒ ⭐ If any of the four is missing, that is your conversion problem,
     not your feature list.
```

---

# 2 · The auth surface — more pages than anyone plans for

> ⭐⭐ **This is where estimates go wrong.** "Add login" sounds like one page. It is eight,
> and every one of them needs an error state.

| Page | | What it needs | ⭐ The common mistake |
|---|---|---|---|
| **Sign up** | ⭐⭐ | Real validation, clear password rules, ⭐ a link to log in | Rules revealed only after submitting. Show them up front |
| **Log in** | ⭐⭐ | ⭐ A "forgot password" link, and the **redirect back to where they were going** | Losing the deep link. Someone clicks a shared URL, logs in, and lands on a dashboard instead |
| **Forgot password** | ⭐⭐ | Says "if that address exists, we sent a link" | ⭐ Confirming whether an email is registered — that is an account-enumeration leak |
| **Reset password** | ⭐⭐ | Token expiry handled with a real message, not a crash | An expired link showing a stack trace or a blank page |
| **Verify email** | ⭐ | A resend option, and a clear "already verified" path | ⭐ No resend. The first email lands in spam and the user is stuck forever |
| **Magic link / OAuth callback** | | Loading and failure states | A blank screen while the token exchanges |
| **Accept invite** | | Works for someone with no account yet | Assuming the invitee is already a user |
| **Logout** | ⭐⭐ | ⭐ Clears session everywhere, including other open tabs | Logging out in one tab while another still shows private data |

```
⭐⭐ EVERY ONE OF THESE NEEDS AN ERROR STATE, and the error must never
   clear what the user typed. Losing a filled form to a failed
   request is the most infuriating bug on the web.
```

---

# 3 · The product surface — behind the login

| Page | | What it is for | ⭐ The common mistake |
|---|---|---|---|
| **First-run / onboarding** | ⭐⭐ | ⭐ Get them to the core action fast. See [PROMPTS ⑥](../PROMPTS.md) | A product tour. **Nobody reads a tour** — get them doing the thing instead |
| **Dashboard / home** | ⭐⭐ | Where they land every session. Answers "what should I do now?" | ⭐⭐ A chart nobody asked for. If the core loop is "create a thing", the button to create a thing IS the dashboard |
| **The core loop screen** | ⭐⭐ | The one thing they do repeatedly | Making it two clicks deeper than it needs to be |
| **List / index views** | ⭐⭐ | Find, filter, sort, paginate | ⭐ Unstable sort — ties with no tiebreaker duplicate rows across pages. And: no empty state |
| **Detail view** | ⭐⭐ | One record, everything about it | ⭐ Arriving by deep link with no way back and no context |
| **Create / edit forms** | ⭐⭐ | ⭐ Autosave or a warning before navigating away | Losing work on an accidental back button or refresh |
| **Delete / destructive flows** | ⭐⭐ | Confirmation proportional to the damage, and ⭐ an undo where possible | A confirm dialog for everything (people stop reading them) or none for the one that matters |
| **Search** | ⭐ | ⭐ A no-results state that suggests what to do | Returning nothing with no explanation |
| **Notifications / activity** | | What happened while they were away | Notifying on everything until people mute it |

---

# 4 · Settings — always underestimated

> **Settings is not one page.** It is a section, and half of it is legally required.

```
⭐⭐ PROFILE
  □ Name, avatar, timezone, language
  □ ⭐ CHANGE EMAIL — with re-verification of the new address
  □ ⭐ CHANGE PASSWORD — requires the current password

⭐⭐ ACCOUNT AND SECURITY
  □ Active sessions / devices, with a way to revoke
  □ Two-factor, if the data warrants it
  □ Connected accounts (OAuth), with disconnect

⭐ NOTIFICATIONS
  □ Per-channel and per-type toggles
  □ ⭐⭐ UNSUBSCRIBE MUST ACTUALLY WORK, from the email itself,
     without logging in. This one is legally required.

⭐⭐ BILLING — if you take money
  □ Current plan, usage against limits, next charge date
  □ Update card · invoice history · download an invoice
  □ ⭐ CANCEL, self-serve. Making people email you to cancel is
    hostile, and in several places it is now illegal.

TEAM — if it is multiplayer
  □ Invite, roles, remove, transfer ownership
  □ ⭐ What happens to a removed member's data

⭐⭐ THE DANGER ZONE — LAW
  □ ⭐ EXPORT MY DATA — a real export, in a real format
  □ ⭐⭐ DELETE MY ACCOUNT — and it must actually reach every system:
    your database, your backups policy, Stripe, your email provider,
    your analytics, your error tracker.
    ⇒ A delete button that only sets deleted_at is not deletion.
```

---

# 5 · System pages — the ones you find out about in production

| Page | | What it needs |
|---|---|---|
| **404** | ⭐⭐ | ⭐ Useful, not cute. Search, main nav, and a link home. **It is the most-visited page you never designed** |
| **500 / error boundary** | ⭐⭐ | An apology, a retry, a support link — ⭐ and **no stack trace** |
| **403 / no permission** | ⭐ | Explains *why*, and what to do about it |
| **Maintenance** | | A real page, not a hung connection |
| **Rate limited (429)** | | Says what happened and when to try again |
| **Unsupported browser** | | Only if you genuinely need it — usually you do not |

```
⭐⭐ THE 404 IS NOT AN EDGE CASE. It is where people land from an old
   link, a typo, a stale bookmark and a search result you deleted.
   ⇒ ⭐ Point every deleted URL at a 301 redirect, not a 404, whenever
     something reasonable exists to point at.
```

---

# 6 · Legal and trust

| Page | | Note |
|---|---|---|
| **Privacy policy** | ⭐⭐ LAW | Must match what you **actually** collect, including what your SDKs collect without asking |
| **Terms of service** | ⭐⭐ LAW | |
| **Cookie policy + consent** | ⭐⭐ LAW | ⭐⭐ The banner must actually gate what it claims to. A banner that sets cookies before you click is worse than none — it is evidence |
| **Accessibility statement** | ⭐ LAW | If the standard from [PROMPTS ②](../PROMPTS.md) applies to you |
| **DPA / subprocessors** | | Required the moment you sell to a business |
| **Security page** | | ⭐ What you do, and how to report a vulnerability |
| **Status page** | | ⭐ For anything people depend on. Cheap trust |

---

# 7 · The invisible layer — not pages, still required

```
⭐⭐ NONE OF THIS IS VISIBLE ON YOUR SITE, AND ALL OF IT IS VISIBLE
   EVERYWHERE ELSE — in search results, in a shared link, in a tab.

  □ ⭐ Unique <title> and meta description on EVERY page
  □ ⭐⭐ OPEN GRAPH IMAGE — every share is a free impression, and a
     broken preview wastes it. Check it in a real Slack/WhatsApp paste.
  □ Favicon set, apple-touch-icon, theme-color, web manifest
  □ sitemap.xml that updates itself · robots.txt
  □ Canonical URLs — no duplicate content across www/non-www,
     trailing slash, http/https
  □ Structured data, where it genuinely applies
  □ ⭐ 301 redirects for every URL you have ever changed
  □ Analytics installed AND confirmed receiving
  □ ⭐⭐ SPF, DKIM AND DMARC if you send any email — all three, or you
     are in spam and nothing in your logs will say so
```

---

# ⭐⭐ What to cut for v1

> **A complete list is not a v1 scope.** Most of the above can wait, and knowing which is the
> difference between shipping and not.

```
⭐⭐ SHIP WITHOUT THESE — nobody will notice on day one:
   blog · changelog · comparison pages · use-case pages · status page ·
   security page · team/multiplayer · 2FA · notification preferences ·
   activity feed · a dashboard with charts

⭐ SHIP WITHOUT THESE ONLY IF YOU ADD THEM SOON:
   FAQ · about page · onboarding · search · export

⭐⭐ YOU CANNOT SHIP WITHOUT THESE:
   hero that says what it is · pricing (if you charge) · contact ·
   the full auth surface including forgot/reset · the core loop ·
   every list with an empty state · 404 and 500 ·
   privacy + terms + cookie consent · account deletion ·
   the invisible layer above

⭐ THE TEST: for each page you are about to build, ask "what breaks
  for a real user if this does not exist on launch day?"
  If the honest answer is "nothing", it is not v1.
```

---

**Back:** [folder index](../README.md) · **The workflow:** [PROMPTS.md](../PROMPTS.md) ·
**Design:** [02-UI-System.md](../02-UI-System.md) · **SEO:**
[08-SEO-and-Meta.md](../08-SEO-and-Meta.md) · **Legal:**
[12-Legal-and-Compliance.md](../12-Legal-and-Compliance.md) · **Launch:**
[10-Ship-Checklist.md](../10-Ship-Checklist.md)
