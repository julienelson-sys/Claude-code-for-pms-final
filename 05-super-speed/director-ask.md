# What Helen is actually asking for

*A breakdown of director-request.txt, so the scope is pinned down
before anything else gets built against it.*

## What's already settled — not what she's asking about

- The team traced the quiet-responder problem to the routing code.
  She knows this already, via Marcus. Not in question.
- The code fix itself is ready: *"small enough to ship this
  afternoon if I asked for it."* Not what she's asking for — she's
  explicitly holding it back.

## The actual ask

> "Before anyone touches the code, I want to see what we'd actually
> build instead — from the point of view of the person it happens
> to."

She wants a **design for what accompanies the fix**, seen through the
eyes of the people affected — not a validation of the fix, not a
technical explanation, and not something scoped from the system's
side.

Two named touchpoints, both required, not optional extras:
- **"Something a handler like Kip would notice."**
- **"Something a responder who's gone quiet would feel differently
  about."**

Anything proposed needs to answer both of those specifically, not
generally.

## What she's explicitly ruling out

> "I don't want to just quietly change a number and call it handled."

> "Not a setting."

Two different fixes are excluded by this: an invisible tuning change
with no visible effect on either screen, and a config/admin toggle
that only engineers would ever see. Both technically "solve" the bug.
Neither is what she asked for.

## The thing she's pointing at without spelling out

> "Wen left a note next to that code years ago wondering whether it
> should work the way it does, and she wasn't wrong to wonder —
> nobody ever came back to it, and I'd rather we come back to it
> properly this time."

She's read (or been told about) the 2019 decay TODO specifically, and
she's naming it as unfinished business, not a side detail. Whatever
gets proposed should resolve that question outright, not leave it
open again under a new label.

## Deliverables, and their actual priority

1. **Required:** a one-pager. *"It doesn't need to be polished — I'd
   rather see something real than a tidy slide."* Substance over
   finish, explicitly.
2. **Bonus, not required:** *"if you can get me something I can
   actually click through, even better."* Worth doing, but the
   one-pager is the actual ask — a prototype without it doesn't
   satisfy the request.

## Sequencing

This is a gate, not parallel work: *"Before anyone touches the
code..."* The one-pager needs to land, and presumably get a decision
from her, before the fix ships — even though the fix is sitting ready
to go right now.

## What "answering this" looks like

A proposal only counts if it can point to:
- a specific thing on the handler's screen that's different and
  noticeable,
- a specific thing on the responder's side that changes how it
  *feels*, not just what it does,
- an explicit answer to Wen's decay question, not a deferral of it,
- and it isn't a setting or a silent number change.

`brief.md` in this folder is a first pass at meeting these — worth
checking each point above against it before treating it as done.
