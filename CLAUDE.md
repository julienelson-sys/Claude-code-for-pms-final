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
