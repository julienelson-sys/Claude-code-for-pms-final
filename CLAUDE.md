# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

_Built from `00-rook/company/` on 8 September 2026._

### My role

I'm the Product Manager for **Rook Dispatch**, started Monday 31 August 2026. I
replaced Priya Raghunathan, who left on 21 August with no overlap — her handover
is a document, not a conversation. I am the only PM on Dispatch.

### The company

Rook Industries (founded 2014, 241 staff, HQ at Site Aleph) sells coordination
and provisioning software to independently-operating masked responders and to
the handlers and quartermasters who support them. Rook does not employ
responders. Revenue is subscription, priced per active responder. Most staff are
remote. Monthly release train; point releases carry a 4.x number.

### The two product surfaces — both on 4.2

**Rook Dispatch** (mine) — gets the right responder to the right incident.
Incident arrives in the handler's web console → Dispatch ranks available
responders into a routing priority order → a callout offer goes to the
top-ranked responder's mobile → they accept or decline → acceptance marks them
engaged and assigns the incident. Declines and timeouts roll to the next
responder. Handlers use the web console; responders use native mobile.
Metrics: **acceptance rate** (headline), **time-to-accept**, **coverage gap**.

**Rook Supply** — gear provisioning: requisitions → quartermaster approval →
fulfillment → maintenance schedules → field failure reports.

**The coupling that matters to me:** Supply's maintenance scheduling *reads* the
Responder Availability Record, which **Dispatch writes**. Supply never writes to
it. Any change to how Dispatch calculates availability lands in Supply's
maintenance scheduling with no change on their side. Availability Confidence
(4.2 roadmap) touches this.

### The team

| Person | Role | Notes |
|---|---|---|
| Helen Achebe | Director of Product, both surfaces | My director. Owns roadmap and commitments. Priya: "She's good. She'll give you room." |
| Marcus Oyelaran | Engineering Manager, Dispatch | First stop for anything uncertain. Straight; will say when something's a bad idea. |
| Wen Li | Staff Engineer, routing (Berlin) | **Built the ranking logic.** There is no good document — understanding routing is a conversation with Wen. Was on PTO 14–24 Aug. |
| Nadia Hoffmann | Support Lead, both surfaces (Berlin) | Sees complaint volume first. Worth a standing 15 minutes. |
| Sofia Marino | Product Designer, Dispatch | Console and phone app. |
| Ravi Menon | Data Analyst, both surfaces (Singapore) | Owns the **official weekly** acceptance numbers. Requests go through `#data`. |

### Vocabulary that means something specific here

- **Responder** — accepts callouts, attends incidents. Not a Rook employee.
- **Handler** — responsible for a responder's availability, gear and readiness.
  Usually the person actually clicking around in the product.
- **Quartermaster** — owns equipment stock and approvals. Supply, rarely Dispatch.
- **Cover identity** — a responder's public persona. Rook holds **no** mapping to
  a legal identity, by contract.
- **Callout** — a request to attend an incident. The unit of work in Dispatch.
- **Callout offer** — one callout presented to one responder, awaiting response.
- **Callout timeout** — how long an offer stays live. Set in the release, same
  for everyone; not a handler-adjustable runtime setting.
- **Decline vs. timeout** — distinct in the data, though both pass the callout on.
- **Coverage gap** — nobody *could* have gone (no capability match). Counted
  separately from low acceptance, which is nobody *would*.
- **Routing priority** — the ranking score. Inputs: proximity (travel-time
  estimate), current availability, capability match, and recent acceptance
  history. **Declining or timing out lowers the recent-acceptance component,
  which lowers routing priority on subsequent callouts until it recovers.**
- **Capability tag** — flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.
- **Mutual aid** — cross-region cover. Not supported; Q4 exploration ("Shared
  cover between responders").

### Where things stand

**4.2 shipped 12 August 2026** and is the live issue. It carried two routing
changes at once:

1. **Routing weight rebalance** — proximity weighted up relative to recent
   acceptance history. A three-quarter-old ask from responders working wide
   geographies, who objected to someone nearby sitting unoffered while the
   system reached 40 minutes away for a better acceptance record.
2. **Callout offer timeout cut from 90s to 60s.**

Plus console filter persistence and three defect fixes. The release itself went
clean — no rollback, no pages.

**Since then:** acceptance rate is down, and callout tickets have run ~3x normal
since roughly 19 August, still elevated as of 26 August and not improving. Nadia
splits them **two thirds "my phone never goes off anymore"** and **one third
"it buzzed but was gone before I could answer."** She can explain the second
(shorter timeout). She cannot explain the first. A handler emailed her directly,
which never happens.

**The prevailing read, which is not yet tested against data:** Priya and Marcus
both said August is seasonally soft every year, two things moved in one release,
and nobody should spiral before seeing numbers. Priya's parting advice was to
look at seasonality first and to *not* let this become a conversation about
reverting 4.2 — the change was asked for, and reverting trades one angry group
of responders for another.

**My timing:** Marcus deliberately held the team off giving me a conclusion —
"give them a week to get up to speed and then regroup properly on the 4.2
picture." That week is up. Nadia has a ticket breakdown ready.

**Q3 roadmap** (owner: Helen, revised 30 June). Committed to 4.2: change to who
gets pinged ✓ shipped, ping timeout tuning ✓ shipped, and **Availability
Confidence** — surface a confidence score alongside stated availability, the one
item driven by *support escalations* rather than internal priorities. It does
not appear in the 4.2 release notes. Committed to 4.3: requisition approval
chains (Supply). Exploring for Q4: handler phone app (Supply), shared cover
(Dispatch). Committed items against a numbered release are locked; changes go
through Product.

### What we established on 10 September

- **It is not seasonal.** Weekly offer volume is flat (172 → 165). A
  seasonal lull means fewer incidents; incidents didn't fall. Kip's
  interview settles it: Meteor Mite near zero and The Gale at a record
  in the *same week, same city*.
- **Aggregate acceptance:** 75–78% baseline → **54.2%** the week of
  release → 72.7% on 31 Aug. Recovering, not recovered.
- **Four responders are frozen out:** Farlight, Meteor Mite, The
  Undertow, Vesper. From ~12 offers/week to 0–1, and **zero accepted in
  the last two weeks.** Revenue-bearing — Rook bills per active responder.
- **The defect chain** (in `00-rook/code/dispatch-routing/`): timeout cut
  90s → 60s; `record_declined()` scores a timeout identically to a
  refusal; penalty 0.12 vs credit 0.08; **no decay** (Wen's TODO, open
  since 2019); floor 0.0. Recovery needs offers you no longer receive —
  an absorbing state. The fix is small and **does not require touching
  the proximity rebalance**, so it isn't a revert conversation.
- **Routing config ships in the release**, not as a runtime setting — so
  there is no hotfix path for the timeout. Affects how early P0 must start.
- **The ticket queue points at the wrong people.** Meteor Mite and Vesper
  have **zero tickets** between them. Kip, Aunt Dot and Halloran filed
  nothing all month. 1 High in 25 tickets. Severity is filer-assigned.
- **Tickets and the CSV disagree, and both may be right.** They agree at
  the extremes and diverge only in the middle band (Nightwell, Ironvale,
  Stormwrack, Cindermark, The Drift report drought while offers rose).
  Candidates: weekly buckets hiding straddling gaps; offers dispatched but
  never delivered to the phone; salience.
- **Treat `callout-history.csv` as unverified.** It cannot split declines
  from timeouts, which the glossary says the real data does — so it is not
  the official reporting. Marcus, 18 Aug: *"it won't be the real weekly
  numbers."* Confirm provenance before relying on it again.
- **Three asks for Ravi, none yet made:** day-level offers per responder;
  delivery confirmations vs offers dispatched; decline/timeout split. Plus
  August 2025 to retire seasonality by evidence.
- **Halloran's requisition evidence belongs to Supply** — one queue, the
  priority field does nothing, 11 days on a cracked vest plate. Unrelated
  to 4.2; it's unsolicited support for the Committed 4.3 item. Hand it over.

### What we established on 15 September

- **Found the exact fix location.** `offer.py`'s `offer_to()` already returns
  three distinct outcomes — `ACCEPTED`, `DECLINED`, `NO_ANSWER`. The
  distinction is thrown away one line later: `dispatch()` calls
  `history.record_declined(responder)` for both `DECLINED` and
  `NO_ANSWER`. This is not new instrumentation — it's a two-line fix to
  stop discarding a value the system already has.
- **Priya's seasonal read does not hold up.** Total offer volume is flat
  (~172/wk → ~165/wk) but redistributed, not reduced: four responders
  collapsed to near-zero while two (Nightwell, The Gale) rose ~24%.
  15 of 16 responders' acceptance rates fell, including ones getting
  *more* work — not consistent with fewer incidents. Kip's two
  responders, same city same week, went opposite directions. Her
  confound-spotting (two changes in one release) and her advice not to
  revert both still stand — just not for the seasonal reason she gave.
- **Manual routing override exists, but only in the console** — nothing
  in `dispatch-routing/` decides *whether* someone's asked, only the
  order (confirmed across all five files: config, routing, offer,
  history, availability). Audit-logged since 4.0. No evidence Aunt Dot
  ever used it for Vesper, and it wouldn't have mattered if she had — an
  override is per-incident and doesn't touch the recent-acceptance
  score, so it can't undo the frozen state.
- **Vesper, traced week by week, confirms the mechanism end-to-end:**
  steady ~82% acceptance for six weeks → drops to 50% the week 4.2 ships
  while still offered a normal volume (the timeout bite) → offers
  themselves collapse to near-zero the next two weeks (the scoring
  lock-out). Good reference case if this needs to be explained to
  someone with one concrete example instead of an aggregate.

### Open threads I inherited

- **Availability Confidence appears to have silently slipped 4.2.** Priya flagged
  that items were squeezed out when the timeline compressed and that the
  conversation with Helen about what's still a Q3 commitment "hasn't happened
  and it needs to." This is the first thing to settle.
- **Marcus's unanswered question (14 Aug):** was the ping change meant to apply
  to responders who have been declining, or only to everyone else? The config
  doesn't distinguish. Wen was on PTO; the thread was never resolved. Given that
  declining lowers routing priority, this is worth understanding before drawing
  conclusions about the "phone never goes off" complaints.
- **Nobody has pulled the real weekly numbers.** Nadia asked Marcus for something
  rough on 18 Aug. Ravi owns the official weekly reporting via `#data` and does
  not appear in that thread at all.
- **There is no written description of how routing decides who gets pinged.**
  Priya asked me directly to write it.
- Console filter persistence will generate cosmetic tickets. Noise — don't let
  it eat the first month.

### Working rules

- Never design anything that assumes Rook can map a cover identity to a legal
  identity. Production holds capability tags, availability windows and callout
  history only. Read Security Policy 4.1 before touching responder records.
- Acceptance rate is reported **weekly, in aggregate** — be precise about which
  number is being quoted and where it came from.
- "Committed" against a numbered release means locked. Route changes through
  Helen, not directly.
- Distinguish declines from timeouts. They are different in the data and they
  mean different things.
