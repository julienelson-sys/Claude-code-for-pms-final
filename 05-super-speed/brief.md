# Response Standing — brief for Helen

**Written for: Helen Achebe, Director of Product**

## The ask

Approve moving Response Standing from proposal to build. The design
below answers what your note asked for directly: it's not a setting,
it's visible to both the responder and the handler, and it closes
Wen's decay question outright instead of deferring it again.

## Background, thirty seconds

4.2's shorter offer window turned honest misses into permanent
penalties. Four responders, Vesper among them, have effectively
dropped out of rotation with no way back on their own. Engineering's
fix is small and ready to ship. You held it back on purpose, asking
to see what we'd build around it first — from the point of view of
the people it happens to, not as a setting.

## What we're proposing

**Response Standing** — a plain-language status, shown in the same
form on the responder's phone and the handler's coverage card, telling
both sides "Steady" or "Recovering" instead of leaving either to
guess. No raw scores exposed on either side.

- **Who it's for:** responders who fall out of rotation through no
  decision of their own — Vesper is the case in hand — and the
  handlers standing next to them with nothing to explain it. Aunt
  Dot, in Vesper's case.
- **What changes:** each side gets an honest, self-explaining signal
  instead of silence. And because the fix underneath closes Wen's
  2019 decay question, the signal tells the truth — standing actually
  recovers on its own, it isn't a label sitting on top of a system
  still stuck at zero.
- **What it deliberately doesn't do:** doesn't get anyone more offers
  on its own, doesn't retroactively fix the four already frozen,
  doesn't expose the underlying number, and doesn't explain full
  routing logic.

## What's attached

- **`prototype.html`** — a clickable mockup of both screens. One
  button toggles both between Steady and Recovering at once, so it's
  visible that it's one status surfaced in two places, not two
  separate features. Open it directly in a browser — no install, no
  server. It's static markup, not connected software; built to make
  the idea concrete enough to react to, not to demonstrate working
  code.

## What this requires

Two things ship together, not separately: the code fix — separating
a missed offer from a real decline, and adding the decay Wen asked
about years ago — and the two labels built on top of it. Shipping the
label without the fix underneath would just be an honest description
of a system that's still broken.

## Decision needed

1. **Approve this direction**, so Engineering scopes Response
   Standing alongside the code fix rather than shipping the fix alone
   and calling it handled.
2. **Separately, regardless of the above:** authorize an immediate,
   one-time reset for the four responders already at the floor —
   Farlight, Meteor Mite, The Undertow, Vesper. They can't recover on
   their own even after the fix ships, and Response Standing doesn't
   reach backward to fix that.

## If anything here needs to change

Say what's off and I'll revise the one-pager and the prototype
together, so they stay consistent with each other and with whatever
you decide.
