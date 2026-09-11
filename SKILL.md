---
name: fixed-point-iteration
description: "Drive a text artifact (file, doc, code) to a fixed point: make one cheap, honest edit, re-read the whole thing, repeat until a full read surfaces no further ambiguity to reduce. Use when polishing a draft toward 90%, tightening a document for its next reader, or when something 'looks done' but small ambiguities remain."
---

# Fixed-Point Iteration

Fixed-point iteration: apply a minimal transformation (an edit), re-observe the state (a re-read), and repeat until the state is at a fixed point — a full read surfaces no further ambiguity to reduce. The re-read is what makes each pass honest: it refreshes what you attend to, so the next edit is made against the file as it actually is, not as you remember it.

The name is the logic; the physics is the explanation. "Fixed-point iteration" names the loop; "LLM attention map" is the physics that explains why it converges.

## Why it converges

Each edit does three things at once:
(a) removes the corrected element from the salient set (progress on the work).
(b) reshapes the terrain around its neighbors (improving clarity): a bump next to a large peak becomes a visible hill once the offending peak is gone. Similarly, an ambiguous large bump may resolve into a set of sharp, focused targets once confounding features are gone.
(c) improves resolution on the remaining potential targets (improving clarity): the proportion of the bounded attention allocated to each target increases, so each remaining issue is seen more sharply and the easy wins come faster.

So there are three reasons why every correct edit, no matter how minor, is a good edit. Every read recomputes the gradient against the new state, making the hill visible on the LLM attention map. The edits drain the salient basin — the inconsistencies with local contrast, where most of the real problems live.

## When to use

- Driving a piece of work to a fixed point by making cheap, honest, one-at-a-time edits.
- Polishing a draft toward 90%: the gap between 80% and 90% is the ambiguities you leave behind, and each cheap round halves them.
- Tightening a document for its next reader.
- Sidestepping the failure modes that derail improvement (see anti-patterns).

## Practiced discipline

REPEAT:
1. Read the whole file to see its current state.
2. Find an improvement you can make towards achieving the completion criterion.
3. Make that one edit as cheaply as correctness allows. The reasoning must be cheaper than the edit: if the deliberation costs more than the change it produces, stop and make the obvious edit.

UNTIL a full read surfaces no further ambiguity to reduce. Polishing rounds are cheap and each one halves what the next reader has to puzzle around — the gap between 80% and 90% is the ambiguities you leave behind.

Then switch instruments, because "nothing pops" is not "it's consistent": run targeted invariant checks — pick a specific global property you suspect might be violated and test it directly. If a check fails, fix it and return to the loop.

## Anti-patterns

| Urge | Move |
|------|------|
| Compaction-fear | Files are immune to compaction. Keep editing. |
| Drafting | Edit the file. It is the work surface; edits are cheap, reversible, and the tool is reliable. |
| Sequencing | Not entirely moot. Start with the least consequential, most isolated edit — it's cheap to get right, and clearing it sharpens the larger issues that remain. Then work toward the bigger, more entangled ones. |
| Candidate-hunting | Any candidate is good. Pick one and edit; the edit reshapes the attention map so the next choice is clearer. |
| Batching | One edit per pass. The loop does the rest. |
| Reconsidering | Nothing is gained by reconsidering. Any correct edit advances and clarifies the task. |
| Over-thinking | The reasoning must be cheaper than the edit. If it isn't, make the obvious edit. |
| Re-reading the same issue | If a re-read surfaces a candidate you already weighed, you've stopped re-observing and started re-deliberating. Make the edit or drop the candidate. |