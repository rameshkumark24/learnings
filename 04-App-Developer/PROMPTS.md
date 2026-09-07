# 🎯 App Development — The Prompt Library

> ⭐⭐ **This is the working file. Fourteen steps, one prompt each, in order.**
> Copy the prompt, fill the `<angle brackets>`, paste. Every prompt ends in a **gate** —
> a question you must be able to answer before the next step.

**Two rules that make all fourteen work:**

```
⭐⭐ ① PASTE STEP 0 ONCE, AT THE START OF EVERY NEW CHAT.
      Without it the agent does not know how you work and will
      default to generic output.

⭐⭐ ② THE "DO NOT" LINES ARE NOT PADDING. They are the single
      biggest lever on what comes back. Never trim them to save space.
```

| # | Step | Produces | Plan mode |
|---|---|---|---|
| **0** | ⭐⭐ The context block | The agent knows your rules | — |
| **1** | Target customer & reality check | A go / no-go, honestly argued | — |
| **2** | Scope — and the NOT list | The v1 boundary | — |
| **3** | ⭐⭐ The spec document | `SPEC.md` — the reference every future chat reads | — |
| **4** | Architecture & stack | `ARCHITECTURE.md` + the decisions | ⭐⭐ yes |
| **5** | ⭐ Database & data model | Schema + RLS from day one | ⭐⭐ yes |
| **6** | ⭐⭐ Edge cases & failure catalogue | `EDGE-CASES.md` — the file that prevents the rewrite | — |
| **7** | Design system | Tokens + the owned component set | — |
| **8** | Frontend — screen by screen | Screens with all five states | ⭐ per screen |
| **9** | Backend & API | Endpoints, validated and authorized | ⭐⭐ yes |
| **10** | ⭐⭐ Offline, sync & lifecycle | The mobile-only layer | ⭐⭐ yes |
| **11** | Testing | Tests that would have caught real bugs | — |
| **12** | ⭐⭐ Security audit | The adversarial pass | — |
| **13** | ⭐ Quality analysis | An honest score, not a victory lap | — |
| **14** | Store submission & launch | Shipped, with a kill switch | — |

---

# ⭐⭐ STEP 0 · The context block

**Paste this first in every new chat, before any other prompt.** It replaces ten rounds of
correction later.

```
CONTEXT — READ BEFORE ANY WORK.

HOW I WORK
- I vibe code. You write most of the code. Keep diffs small and tell me
  exactly what to look at and what worried you.
- Plan before code on anything non-trivial: more than one file, or any
  auth / payment / user-data / native change. Wait for my approval.
- Never run git push, git reset --hard, git rebase, a migration, or a
  store submission.
- Never add a dependency without naming it and why — and in mobile,
  say whether it needs NATIVE code, because that ends OTA updates.
- Never refactor what I did not ask about. Never delete or weaken a
  test to make something pass.
- If you are unsure, say so. If I ask for something that will leak
  data, drain battery, or get rejected — say so BEFORE building it.

FIVE FACTS THAT DRIVE EVERY MOBILE DECISION
1. I cannot hotfix. Store review is 1-3 days, so a shipped bug is live
   for a week. OTA is the only fast path and it cannot change native code.
2. Old versions never die. Users do not update. Every API change is
   additive or a migration — never breaking.
3. The network is hostile: tunnels, 2G, airplane mode, captive portals
   that resolve DNS and block everything. Offline is a state I DESIGN,
   not an error you catch.
4. The OS kills the app in the background without warning.
5. The bundle is public. Anyone can unzip the APK. There are NO secrets
   in a mobile app.

STACK
Expo (React Native) + TypeScript · Expo Router · TanStack Query with
persistence · expo-secure-store for tokens, MMKV/AsyncStorage for the
rest · Supabase / my API · Sentry · EAS Build + EAS Update.
Money as integers in the smallest unit. Dates stored UTC, formatted at render.

NON-NEGOTIABLE
- Tokens in secure store. NEVER AsyncStorage — it is plain text on disk.
- No secrets in the bundle. If a call needs a secret, the app calls MY
  API and MY API holds it.
- Every query filters by the authenticated user, SERVER-SIDE.
- RLS on and forced. Never return a whole row — use an explicit shape.
- Never log PII. Crash reports leave the device.
- Permissions asked at the moment of use, with a reason. Never on launch.

STOP DOING THESE
- Claiming it works when you only ran the simulator. Say "simulator only".
- Adding a native dependency without flagging that it ends OTA.
- Inventing a package, an API, or a config option.
- Silently changing files outside the task.
- Swallowing an error so the screen goes blank.
- Ignoring Android. "Works on iOS" is half a job.
- Agreeing with me when I am wrong.

Acknowledge in one line, then wait for my actual request.
```

---

# STEP 1 · Target customer & the reality check

**Do this before writing a line.** The most expensive app is the one nobody needed.

```
I am considering building: <one sentence — what it does, for whom>

Act as a skeptical product strategist, not a supporter. I want the
honest version, not encouragement.

Give me:

1. WHO EXACTLY IS THIS FOR
   Not "small businesses". A specific person: their role, their day,
   the moment this need appears. If you cannot name that moment
   concretely, say the idea is too vague and stop.

2. WHAT THEY DO TODAY INSTEAD
   Every product replaces something — a spreadsheet, WhatsApp, paper,
   a competitor, or doing nothing. Name it. "Nothing" is the hardest
   competitor to beat, not the easiest.

3. WHY THEY WOULD SWITCH
   Switching costs are real: learning, migrating, trusting. What is
   the 10x — not 10% — improvement that beats that cost?

4. WHO ALREADY DOES THIS
   Real competitors with names. What they charge. What they are bad at.
   If there are none, that is usually a bad sign, not a good one —
   tell me which it is here.

5. THE THREE REASONS THIS FAILS
   Ranked by likelihood. Be specific and unkind.

6. THE KILL CRITERIA
   What would I have to learn in the next two weeks that should make
   me abandon this? Make it concrete and testable.

7. THE SMALLEST TEST
   What could I do in ONE WEEK, without building the app, that would
   tell me whether this is real?

DO NOT validate the idea to be encouraging. Do not give generic startup
advice. Do not tell me the market is large. If this looks like a bad
idea, your first line should say so plainly.
```

```
⭐⭐ THE GATE — do not go to step 2 until:
   · you can name one real person who has this problem
   · you know what they do today instead
   · you have written down what would make you quit
```

---

# STEP 2 · Scope — and the NOT list

```
Product: <one sentence from step 1>
Target user: <the specific person from step 1>

Define v1. The goal is the SMALLEST thing a real user would use weekly
or pay for — not a demo, not everything.

Give me:

1. THE ONE CORE LOOP
   The single action a user repeats. Everything else supports it.
   If there are two, pick one and tell me why.

2. V1 FEATURES — maximum 5
   For each: what it does, why it cannot be cut, the screens it needs.

3. THE NOT LIST — this is the important half
   Everything a reasonable person would expect that v1 will NOT do,
   each with one line on why it waits. Be aggressive. Include the
   things I will be tempted by: social features, sharing, an analytics
   dashboard, settings nobody changes, an onboarding tour, dark mode.

4. THE SCREEN LIST
   Every screen v1 needs. If it is more than 8, cut a feature and tell
   me which one.

5. WHAT MAKES THIS HARD
   The one or two things that will take 3x longer than they look.
   Usually sync, offline, payments, permissions, or a third-party API
   that lies about its reliability.

6. THE MOBILE QUESTION
   Does this genuinely need to be an app? What would it lose as a
   mobile website? If the honest answer is "nothing", say so — an app
   costs store review, two platforms, and no hotfix.

DO NOT pad the feature list. Do not add "nice to haves". A short v1
that ships beats a complete one that does not.
```

```
⭐⭐ THE GATE — you have a NOT list, and it is LONGER than the yes list.
```

---

# ⭐⭐ STEP 3 · The spec document

> **This is the file every future chat reads.** It is why chat #40 still builds the same app as
> chat #1. Write it once, keep it in the repo, paste or link it whenever you start work.

```
Write SPEC.md for this app. This document is the permanent reference —
every future session reads it before touching code, so it must stand
alone with no memory of this conversation.

Product: <one sentence>
Target user: <from step 1>
Core loop: <from step 2>
V1 features: <from step 2>
NOT in v1: <the not list>

Structure it exactly like this:

1. WHAT THIS IS — three sentences maximum. Someone reading only this
   section knows what the app does and who it is for.

2. THE USER — who they are, the moment of need, what they used before.

3. THE CORE LOOP — the repeated action, step by step, as the user
   experiences it.

4. FEATURES — each with: what it does · what "done" means · what it
   explicitly does NOT do.

5. NOT IN V1 — the full list, with the reason each one waits.

6. SCREENS — every screen, its purpose, and what it shows when there
   is no data yet.

7. THE RULES OF THE DOMAIN — the business logic that is not obvious.
   This is the section that saves the most rework later: what is the
   maximum, what happens on a conflict, who can see what, what is
   irreversible, and what must never happen twice.

8. VOCABULARY — the exact words the app uses for its concepts, and the
   words it must never use. Pick one and never drift: "trip" or
   "journey", never both.

9. OPEN QUESTIONS — what is genuinely undecided. Do not paper over
   these with a guess; list them so I decide deliberately.

DO NOT include implementation, framework choices, or code. This is
what and why, never how. Ask me about anything ambiguous BEFORE
writing rather than inventing an answer — but ask everything in one
batch, not one question at a time.
```

```
⭐⭐ THE GATE — hand SPEC.md to a fresh chat with no other context and
   ask "what does this app do, and who is it for?" If the answer is
   right, the spec works. If not, fix it now — not after 40 files exist.
```

---

# STEP 4 · Architecture & stack · ⭐⭐ plan mode

```
Read SPEC.md. Enter plan mode. Do not write code.

Design the architecture for this app. My defaults are Expo + TypeScript,
Expo Router, TanStack Query, Supabase — argue against any of them if
this app is a bad fit, and say so early rather than politely going along.

Cover:

1. THE SHAPE
   What runs on the device, what runs on the server, and what the
   boundary is. Be explicit about what the device must NEVER decide —
   prices, permissions, limits, anything a user could tamper with.

2. NAVIGATION
   The full tree. Which screens require auth. What a deep link into a
   protected screen does from a COLD START when the token is expired.

3. STATE — draw the four apart and say which is which:
   · server state (TanStack Query)
   · local UI state
   · persisted device state (and where: secure store vs MMKV)
   · derived state — which should NOT be stored at all

4. THE THIRD-PARTY LIST
   Every service and library, each with: why · what it costs at 1k
   users · what happens when it is down · and — for mobile — whether
   it requires NATIVE code, because that ends OTA for that change.

5. THE FIVE DECISIONS THAT ARE EXPENSIVE TO REVERSE
   Name them explicitly and say what each one forecloses. Auth provider,
   the offline model, the payment type, the data model, and the
   navigation shape are the usual suspects.

6. WHAT WOULD BREAK AT 10x
   Not "we'll scale later" — which specific thing breaks first, and
   what the fix would cost then versus now.

7. WHAT I SHOULD BUILD FIRST
   The one vertical slice that proves the architecture works end to end.

DO NOT produce a generic best-practices architecture. Every choice must
trace to something in SPEC.md. Where you are guessing, say you are
guessing. Where two options are genuinely close, say so and give me the
tiebreaker rather than pretending one is obviously right.
```

```
⭐⭐ THE GATE — you can name the five expensive decisions and say why
   each was chosen. If any answer is "it's the default", go back.
```

---

# ⭐ STEP 5 · Database & data model · ⭐⭐ plan mode

```
Read SPEC.md, section 7 especially. Enter plan mode. Do not write code yet.

Design the data model.

1. THE TABLES
   Every table with every column, type, nullability, and default.
   Money as integer minor units. Timestamps as timestamptz, stored UTC.
   No floats for anything a human counts.

2. THE RELATIONSHIPS
   Foreign keys, and what happens on delete for each: cascade, restrict,
   or set null. State the rule for every one — a wrong cascade is how
   data disappears silently.

3. OWNERSHIP — the security-critical part
   For EVERY table, answer: who owns this row, and which column proves
   it? Any table where you cannot answer that is a data leak waiting to
   happen — flag it rather than guessing.

4. RLS POLICIES
   Write them now, not later. Enabled AND forced. For each policy state
   in plain English what it allows and to whom.
   Then answer: if I disabled the client entirely and hit the database
   with a valid user token, what could I read that is not mine?

5. THE INDEXES
   Every query the app will run, and the index that serves it.
   ⭐ Every list that will be paginated needs a STABLE sort — a sort key
   with ties and no tiebreaker duplicates rows across pages. Name the
   tiebreaker column for each list.

6. THE MIGRATION PLAN
   How the first migration runs, and how a later one adds a column
   without breaking a SIX-MONTH-OLD app build that is still installed
   and still calling. Expand, migrate, contract — never rename in place.

7. WHAT I WILL WANT IN V2 THAT THIS MAKES HARD
   Be honest. A model that is right for v1 and impossible for v2 is a
   bad model, and now is the only cheap time to know.

DO NOT over-normalise for imagined future needs, and do not denormalise
for imagined performance. Ask about anything in the domain rules you
are unsure of instead of inventing a rule.
```

```
⭐⭐ THE GATE — for every table you can say who owns the row and which
   column proves it. RLS is written, enabled, and forced.
```

---

# ⭐⭐ STEP 6 · Edge cases & the failure catalogue

> **This is the step people skip, and it is the one that prevents the rewrite.** Do it before
> building, not after the bug reports.

```
Read SPEC.md and the architecture. Do not write code.

Produce EDGE-CASES.md — the catalogue of everything that can go wrong.
For each: what happens, what SHOULD happen, and who handles it (client,
server, or both).

Work through these categories deliberately. Do not skip one because it
seems unlikely — the unlikely ones are the ones that ship.

1. EMPTY AND FIRST-RUN
   First launch, no data, no permissions. Empty list vs a list filtered
   to zero — these are two different screens with two different actions.
   Search with no results. A deleted item still open on another screen.

2. NETWORK — the mobile-specific ones
   Offline before a request · offline DURING a request · a captive
   portal that resolves DNS and blocks everything · a request that
   hangs for 60 seconds instead of failing · a response that arrives
   after the user navigated away · the same write submitted twice
   because the first looked stuck.

3. THE LIFECYCLE
   The OS kills the app mid-form · mid-upload · mid-payment.
   Backgrounded for an hour and returned to with a stale screen and an
   expired token. A deep link from a cold start. A push notification
   tapped while the app is already open on another screen.

4. AUTH
   The token expires mid-session · expires mid-request · the refresh
   itself fails · the user logs out on another device · the account is
   deleted while the app is open · the user is logged in but no longer
   has permission for the screen they are on.

5. DATA AND CONCURRENCY
   Two devices edit the same row · a queued offline write conflicts on
   sync · a row is deleted while someone is editing it · a list changes
   underneath pagination · a number that is zero, negative, enormous,
   or exactly at a boundary.

6. INPUT
   Empty · whitespace only · 10,000 characters · emoji · right-to-left
   text · a name with an apostrophe · a paste of formatted text · a
   file that is not what its extension claims.

7. DEVICE
   Storage full · the largest system font · the smallest supported
   screen · rotation mid-action · a permission denied permanently ·
   low-power mode · a very slow Android.

8. MONEY, if this app takes payments
   A double tap on pay · the network drops after charge but before
   confirmation · a webhook delivered twice · a webhook never delivered ·
   a refund · a subscription that lapses while offline.

For each item give me: the trigger · the current behaviour if unhandled ·
the correct behaviour · and where the fix belongs.

Then rank the whole list by (likelihood × damage) and tell me which
ones must be handled before v1 ships versus which can wait.

DO NOT give me generic error-handling advice. Every item must be
specific to THIS app and its actual screens.
```

```
⭐⭐ THE GATE — EDGE-CASES.md exists and is ranked. The top ten are in
   your build plan, not in a "later" list.
```

---

# STEP 7 · The design system

```
Read SPEC.md. I need the visual foundation before any screen gets built.

Build the design system as tokens plus a small owned component set.

1. TOKENS FIRST — before any component
   · ONE spacing scale (4/8/12/16/24/32) and nothing outside it
   · Five type sizes, not eleven. Two weights.
   · Colours NAMED BY ROLE, never by value: --fg, --fg-muted, --bg,
     --bg-subtle, --border, --primary, --danger. One accent, used only
     on the primary action.
   · One radius. One border weight.
   · Platform-aware: iOS and Android have different conventions for
     navigation, back behaviour, and typography. Do not force one onto
     the other.

2. THE COMPONENT SET — eight, not forty
   Button, Input, Card, Badge, Skeleton, EmptyState, ErrorState, Sheet.
   Plus three layout primitives: Stack, Row, Screen.
   I own this code. Do not wrap a library.

3. EVERY COMPONENT SHIPS ITS STATES
   Default · pressed · disabled · loading · error. A control with no
   pressed state feels broken on a phone even when it works.

4. ACCESSIBILITY, BUILT IN NOT BOLTED ON
   · Tap targets ≥ 44pt iOS / 48dp Android, and not touching each other
   · Every icon-only control has an accessibility label
   · Text scales with the system font setting WITHOUT CLIPPING —
     this is the one that breaks most layouts
   · Colour is never the only signal
   · Respect reduce-motion

DO NOT: purple-to-blue gradients, glassmorphism, three feature cards
with emoji icons, everything centred, or a font-weight-bold everywhere
layout. That set reads as generated, and it reads that way to users too.
Real icons at one size and stroke width. One deliberate motion moment
in the whole app, not motion on everything.

Show me the tokens and ONE component first. I will approve the
direction before you build the other seven.
```

```
⭐ THE GATE — screenshot a screen. If it has the same shape as a stock
   template, nothing was designed yet.
```

---

# STEP 8 · Frontend — one screen at a time · ⭐ plan per screen

> **Never ask for "the screens". Ask for one.** A batch of six screens is six times the diff
> and none of them get read properly.

```
Build the <screen name> screen.

SPEC: <paste that screen's section from SPEC.md>
DATA: <which query/mutation it uses>
NAVIGATION: how it is reached, and what "back" does from here

IT MUST HANDLE ALL FIVE STATES — I will check every one:
 ① LOADING — a skeleton SHAPED LIKE THE CONTENT, not a centred spinner
    that gets replaced by something a different size. Do not show it at
    all under 200ms.
 ② ERROR — what happened, what to do, and a retry. Branch by status:
    401 is not 404 is not 500 is not "you are offline".
    NEVER clear the user's form on an error.
 ③ EMPTY — and this is TWO states: never-had-any (with the action that
    creates the first one) versus filtered-to-zero (with clear filters).
 ④ OFFLINE — a real offline state, not an infinite spinner. This is
    the single most common mobile bug.
 ⑤ SUCCESS — visible confirmation. Silence reads as failure and people
    tap twice.

MOBILE REQUIREMENTS, all of them:
 · Safe areas — notch, status bar, home indicator
 · Nothing hidden behind the keyboard. Test the form with the keyboard
   OPEN on a small device.
 · No horizontal overflow at the smallest supported width
 · Lists virtualised (FlashList/FlatList). Never .map() over 500 rows.
 · Images resized before display — never a full-resolution photo in a list
 · Every listener and subscription cleaned up on unmount: AppState,
   NetInfo, keyboard, navigation, dimensions. They do not unmount with
   the screen.
 · Pull-to-refresh if the list can go stale

Keep the diff to this screen. When you are done, tell me:
 · what to look at first
 · what you were unsure about
 · whether you ran it on a device or only the simulator — say which
```

```
⭐ THE GATE — open the screen in airplane mode. If you get a spinner
   that never resolves, it is not done.
```

---

# STEP 9 · Backend & API · ⭐⭐ plan mode

```
Read SPEC.md and the data model. Enter plan mode first.

Build the API for <feature>.

EVERY ENDPOINT, WITHOUT EXCEPTION:
 · Input validated at the boundary with an explicit schema. Never spread
   a request body into a database call — whitelist the fields.
 · Authorization enforced IN THE QUERY, not after loading the row.
   findByIdAndOwnerId(id, sessionUserId) — not "load, then check".
 · The session user comes from the token. NEVER from anything the
   client sent.
 · An explicit response shape. Never return a whole row — an explicit
   shape is an allow-list, and it is how you stop leaking the column
   someone adds in six months.
 · Pagination with a server-enforced maximum and a STABLE sort.
 · Errors that do not leak internals. No stack traces to the client.

FOR ANYTHING THAT WRITES:
 · Idempotency. Mobile clients retry. The same write WILL arrive twice.
 · A transaction boundary that is explicit and correct.
 · Rate limiting per user and per IP.

FOR ANYTHING THAT CALLS A THIRD PARTY:
 · Connect AND read timeouts, always. The defaults are effectively
   infinite and one slow dependency exhausts everything.
 · Bounded retries with backoff and jitter. Never retry a 4xx except 429.
 · What the user sees when it is down — degrade, do not hang.

MOBILE-SPECIFIC — this is the one people forget:
 · This API must serve an app build from EIGHT MONTHS AGO that is still
   installed and still calling. Every change is additive or a migration.
   Tell me explicitly if anything here would break an older client.
 · Version the API, or have a deliberate reason not to.

Before you write it: list the endpoints with method, path, auth
requirement, and response shape. I will approve that list first.
```

```
⭐⭐ THE GATE — the ID-swap test. Log in as user A, request user B's id.
   Must be 403 or 404. Do it before moving on, not at step 12.
```

---

# ⭐⭐ STEP 10 · Offline, sync & lifecycle · ⭐⭐ plan mode

> **The layer that separates a real app from a website in a wrapper.** Design it; do not let it
> emerge.

```
Read SPEC.md and EDGE-CASES.md sections 2 and 3. Enter plan mode.

Design the offline and lifecycle behaviour.

1. EVERY SCREEN GETS ONE OF THREE ANSWERS — decide per screen, in advance:
   ① WORKS OFFLINE — cached, and writes queue for later
   ② READS OFFLINE — cached data with a "last updated" stamp; writes
      blocked with a clear message
   ③ NEEDS THE NETWORK — an explicit offline state, never a spinner
   Give me the table: screen → answer → why.

2. DETECTION THAT ACTUALLY WORKS
   "Connected" is not "the internet works". A captive portal resolves
   DNS and blocks everything. Test a real request, not the flag.

3. THE WRITE QUEUE, if anything queues
   · Every queued write is IDEMPOTENT. It will be sent twice.
   · What the user sees while it is pending
   · What happens when it fails permanently — not silence
   · What happens when the app is KILLED with a queue still pending
   · Conflict resolution: last-write-wins, or something the user resolves?
     Say which, and why, per entity.

4. THE LIFECYCLE
   · Draft state saved AS THE USER TYPES, not on submit. Android kills
     backgrounded apps freely and an empty form loses everything.
   · Returning to foreground: refresh stale data, but do NOT stampede
     every query at once.
   · A long task that must survive backgrounding — or tell the user to stay.
   · Deep links from a COLD START, not just when already running.

5. THE COST QUESTION
   Every background poll costs the user battery and data, and they can
   see both in Settings. "Drains my battery" is a review you do not
   recover from. Where am I polling, and can it be a push instead?

DO NOT build a full offline-first sync engine unless SPEC.md actually
demands it. Say plainly which of the three answers each screen needs
and build only that.
```

```
⭐⭐ THE GATE — airplane mode, then a throttled lossy connection (worse
   than offline, because requests HANG instead of failing). Then force-kill
   mid-action and reopen. No spinner survives.
```

---

# STEP 11 · Testing

```
Write tests for <feature>.

I do not want coverage. I want the tests that would have caught a real
bug — so start by telling me what could actually break here, then test
that.

MUST EXIST:
 · ⭐⭐ AN AUTHORIZATION TEST PER RESOURCE — user B gets 403/404 on user
   A's row. You will not write this unless I ask, and it is the #1 real
   leak. Write it.
 · The empty case, the one-item case, the many-items case
 · The error path — and that the form is NOT cleared when it fires
 · Boundaries: zero, negative, maximum, one-over-maximum
 · Anything involving money: exact amounts, rounding, double submission
 · Idempotency: the same write twice produces one result

FOR THIS APP SPECIFICALLY:
 · Offline → online transitions
 · A queued write that is sent twice
 · Token expiry mid-request
 · The app killed mid-action

DO NOT write tests that only assert the mock was called. Do not test
implementation details that break on every refactor. If a test needs
more setup than the code it tests, say so — that usually means the code
needs restructuring, not that the test needs more mocks.

Then tell me honestly: what is still untested that worries you?
```

```
⭐ THE GATE — the authorization test exists and passes for every
   user-owned resource.
```

---

# ⭐⭐ STEP 12 · The security audit

> **Run this as a separate pass, adversarially.** The model that wrote the code shares the blind
> spot that made the bug — so run step 12b in a *different* model.

```
Audit this app for security. Be adversarial. Assume the attacker has
the APK, can proxy the traffic, and has a valid account of their own.

1. AUTHORIZATION — the one that actually leaks
   For every endpoint: can a logged-in user reach another user's data
   by changing an ID? Check list and search endpoints too — they are
   the ones people forget to scope.
   Is authorization in the QUERY, or after loading? After loading is
   still a leak if the row is returned on any path.

2. THE BUNDLE
   What secrets are in it? Assume every string ships. Publishable keys
   are fine; anything else is not. Tell me exactly what to grep for.

3. LOCAL STORAGE
   What is written to disk, and where? Anything personal in AsyncStorage
   is plain text on the device. Does the query cache persist user data?
   Does logout clear ALL of it — secure store, cache, files, images?

4. TRANSPORT
   HTTPS everywhere, no cleartext exception in the Android config.
   Anything sensitive in a URL — those land in logs and analytics.

5. INPUT
   Every input validated server-side. No request body spread into a DB
   call. Deep links are untrusted input from ANY app or website — is
   every parameter validated? Can a deep link perform an action without
   confirmation?

6. UPLOADS
   Content validated, size capped, EXIF stripped — photos carry GPS.

7. PRIVACY
   What leaves the device? Crash reports and analytics are a data store
   you never designed. Is PII scrubbed before send? Does the store
   privacy form match what the SDKs actually collect?

For each finding: the concrete attack, the file and line, and the fix.
Rank by real-world exploitability, not theoretical severity.

If you find nothing in a category, say so explicitly — do not invent
findings to seem thorough.
```

**⭐⭐ STEP 12b — the cross-check, in a different model:**

```
Here is a diff. Find bugs. Check specifically:
 ① Can a logged-in user reach another user's data by changing an ID?
 ② Can this loop, retry, or fire more than once? What does it cost?
 ③ What happens if the network drops HALFWAY THROUGH this?
 ④ What happens if the OS kills the app right here?
 ⑤ What input breaks it — empty, null, huge, wrong type, unicode?
 ⑥ Is anything secret being written to storage or logs?
List concrete failures with line numbers. Do not summarise the code.
If you find nothing, say so — do not invent findings.
```

```
⭐⭐ THE GATE — three things done BY HAND, not claimed:
   ① unzip your own build and grep it for secrets
   ② the ID-swap test THROUGH A PROXY, as two real accounts
   ③ log out and confirm nothing personal survives on disk
```

---

# ⭐ STEP 13 · Quality analysis

> **Ask for the honest score.** A model asked "is this good?" will say yes.

```
Assess this app's quality honestly, as if you were deciding whether to
recommend it to someone. I want the real assessment, not encouragement.

1. WOULD A REAL USER FINISH THE CORE LOOP?
   Walk it step by step as a first-time user who has not read anything.
   Where would they hesitate, guess, or give up? Name the exact screen.

2. WHAT LOOKS UNFINISHED
   Placeholder text, inconsistent spacing, a screen that does not match
   the others, an error message written for a developer, a button with
   no pressed state.

3. THE FIVE-STATE AUDIT
   Every screen, every list: loading, error, empty, offline, success.
   Which are missing? Missing states are the most common reason an app
   feels unfinished.

4. PERFORMANCE, HONESTLY
   Cold start time. Scroll on a LOW-END ANDROID, not an iPhone. Any
   list not virtualised. Any full-resolution image in a list. App size.

5. WHAT WOULD GET A ONE-STAR REVIEW
   Rank them. Battery drain, data usage, lost work, a spinner that
   never ends, and "it deleted my thing" are the usual winners.

6. WHAT IS ACTUALLY GOOD
   Say this too — I need to know what to protect during refactors.

7. THE HONEST VERDICT
   Is this ready to ship to strangers? Not "with some polish" —
   yes or no, and if no, the specific list standing in the way.

DO NOT be encouraging. Do not tell me it looks great. If the answer to
7 is no, lead with that.
```

**⭐ Then verify from outside — a grade is a floor, not a certificate:**

| Tool | Point it at | Target |
|---|---|---|
| [SSL Labs](https://www.ssllabs.com/ssltest/) | Your **API domain** — your API is a website | A or A+ |
| [Security Headers](https://securityheaders.com/) | API + your privacy-policy page | A |
| Play Console **pre-launch report** | Your build — free, real devices, usually ignored | No crashes |
| Sentry crash-free rate | Production | > 99.5% |
| `npx license-checker --summary` | Dependencies | No GPL/AGPL |

```
⭐⭐ NONE OF THESE TEST WHETHER USER A CAN READ USER B'S DATA.
   That is step 12, and it needs you and a proxy.
```

---

# STEP 14 · Store submission & launch

```
Prepare this app for store submission. Walk the full checklist and tell
me what is NOT done — do not tell me what is.

CONTENT
 · No placeholder text anywhere. Grep for: lorem, TODO, "Feature One",
   example.com, John Doe, test@test.
 · Real copy, spell-checked. Every image and icon has an a11y label.

LAYOUT — on the SMALLEST supported device
 · No horizontal overflow · safe areas respected · nothing behind the
   keyboard · tap targets ≥ 44pt/48dp · tested at the LARGEST system
   font size without clipping · landscape works or is deliberately locked

STATES — every screen
 · Offline · empty (both kinds) · error with retry · success feedback ·
   skeletons not bare spinners

PAYMENTS — ⭐⭐ this is the #1 rejection and it is binary
 · DIGITAL goods or subscriptions ⇒ MUST use Apple/Google IAP
 · PHYSICAL goods or real-world services ⇒ MUST NOT use IAP
 · Receipts validated SERVER-SIDE. Restore purchases works.
 Tell me which category this app is in and confirm the implementation
 matches. Getting this backwards costs a full review cycle each time.

LEGAL
 · Privacy policy live, reachable, and linked IN-APP — a 404 is a rejection
 · Terms/EULA · in-app account deletion that reaches every system
 · If users can post anything: filter, report, block, and contact info.
   Apple requires all four.
 · Data-safety / privacy-nutrition form matches what you ACTUALLY
   collect — including what your SDKs collect without you asking
 · Permission usage strings explain WHY in plain language.
   "This app needs camera access" is a rejection.
   "To scan the barcode on your receipt" is not.

OPERATIONS — the part that matters after launch
 · Sentry with native symbolication, and a REAL crash triggered once to
   confirm symbolication works. You find out during the incident otherwise.
 · ⭐⭐ A STAGED ROLLOUT. Never 100% on day one.
 · ⭐⭐ A FORCE-UPDATE MECHANISM — the only kill switch you have for the
   day a broken version is live and review takes three days.
 · The OTA path tested once, end to end, BEFORE you need it.
 · Tested on: oldest supported iOS · oldest supported Android · one
   cheap Android · one small screen · one large screen

For each item: done, not done, or not applicable — with a reason.
Do not mark anything done that you have not actually verified.
```

```
⭐⭐ THE GATE — before you submit:
   ① a staged rollout is configured
   ② force-update works and you have tested it
   ③ a real crash appeared in Sentry, symbolicated
   ④ the demo account for review exists and works
```

---

# ⭐ After launch — the four prompts you will actually reuse

| When | Prompt |
|---|---|
| **A crash in Sentry** | `Here is a stack trace and the device/OS breakdown. What is the root cause? How many users are affected, and is this OTA-fixable or does it need a store release?` |
| **A bad review** | `Here is a one-star review. What is the actual underlying problem, which screen is it on, and is it a bug, a missing state, or a misunderstanding?` |
| **"It's slow"** | `Profile the cold start and the <screen> scroll on a low-end Android. Give me the top three costs by measured milliseconds, not by guess.` |
| **A new feature** | `Read SPEC.md and EDGE-CASES.md. I want to add <feature>. What does it break? What does it make impossible in v2? Plan mode first.` |

---

**The full library:** [folder index](README.md) · **Per-project rules:**
[CLAUDE-md-template.md](CLAUDE-md-template.md) · **The audit in depth:**
[10-Ship-Checklist.md](10-Ship-Checklist.md) · **Security detail:** [05-Security.md](05-Security.md)
