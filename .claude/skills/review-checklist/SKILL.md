---
name: review-checklist
description: Review a brief, proposal, or one-pager against a fixed four-point checklist before it moves forward — ownership, success criteria, scope consistency, and problem-before-fix ordering. Use when asked to review, check, or vet a brief, or before a brief gets sent, presented, or approved.
---

# Review checklist

A fixed check, run the same way every time, so a brief gets vetted
against the same bar regardless of who's reviewing or what mood
they're in. Exactly four checks. No more, no fewer — resist the urge
to add a fifth one just because it's tempting in the moment.

## What to review

If `args` names a file or points at specific content, review that.
Otherwise, ask which brief to check, or offer to review the most
recent brief-like document already in the conversation, if one
exists.

## The four checks — in this order, every time

For each one, quote the exact line(s) that satisfy it, or state
plainly what's missing. Don't soften a miss into a maybe, and don't
give partial credit — a check either has what it needs or it doesn't.

1. **Names who owns it.** A specific person or named role — not "the
   team," not implied, not left for the reader to infer. If the
   document lists several people, the one accountable for the outcome
   needs to be unambiguous, not just whoever's mentioned first or
   most.

2. **Says how we'll know it worked.** A concrete way to check the
   outcome afterward — a metric, a behavior, a test, an observable
   change. A description of what will be built does not satisfy
   this. Neither does a vague intention ("this should help"). Ask:
   if this shipped, how would anyone actually know it did what it
   was supposed to?

3. **Scope at the end matches scope at the start.** Reread the
   opening framing — what's in, what problem is being solved, for
   whom — against wherever the document lands by its close. Flag any
   drift: a bigger promise creeping in, a quiet narrowing, a
   different audience by the end than the one named at the start.

4. **Explains the problem before it proposes the fix.** The document
   needs to establish what's wrong, and for whom, before it describes
   what to build. A fix introduced before the problem it solves is a
   fail on this point even if the fix itself is good — check the
   actual order of the document, not just whether a problem statement
   exists somewhere in it.

## Output

Exactly four lines, one per check, each starting with ✓ or ✗, then one
line naming the single most important thing to fix if anything failed
— or that it's ready to move forward if all four passed.

This is a check, not an edit — don't rewrite the brief unless asked
to separately, and don't turn a ✗ into a suggestion or a soft maybe.
