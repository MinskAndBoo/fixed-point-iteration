---
name: fixed-point-iteration
description: "Default editing discipline for any text artifact (file, doc, code, config, prompt): establish a completion criterion, then make one cheap, honest edit, re-read the whole thing, repeat until the criterion is met. Use for any multi-edit task — the loop prevents batching, re-litigation, and over-generation."
---

# Fixed-Point Iteration

Fixed-point iteration: establish a completion criterion, then apply a minimal transformation (an edit), re-observe the state (a re-read), and repeat until the criterion is met. The re-read is what makes each pass honest: it refreshes what you attend to, so the next edit is made against the file as it actually is, not as you remember it.

The name is the logic; the physics is the explanation. "Fixed-point iteration" names the loop; "LLM attention map" is the physics that explains why it converges.

## Why it converges

Each edit does three things at once:
(a) removes the corrected element from the salient set (progress on the work).
(b) reshapes the terrain around its neighbors (improving clarity): a bump next to a large peak becomes a visible hill once the offending peak is gone. Similarly, an ambiguous large bump may resolve into a set of sharp, focused targets once confounding features are gone.
(c) improves resolution on the remaining potential targets (improving clarity): the proportion of the bounded attention allocated to each target increases, so each remaining issue is seen more sharply and the easy wins come faster.

So there are three reasons why every correct edit, no matter how minor, is a good edit. Every read recomputes the gradient against the new state, making the hill visible on the LLM attention map. The edits drain the salient basin — the inconsistencies with local contrast, where most of the real problems live.

## When to use

Any time you are making edits to a text artifact — file, doc, code, config, prompt. This is the default editing discipline, not a special polishing mode. Specifically:

- Driving a piece of work to a fixed point by making cheap, honest, one-at-a-time edits.
- Polishing a draft: the gap between 80% and 90% is the ambiguities you leave behind, and each cheap round halves them.
- Tightening a document for its next reader.
- Any multi-edit task where the model is tempted to batch, re-litigate, or over-generate.
- Sidestepping the failure modes that derail improvement (see anti-patterns).

## Practiced discipline

0. Establish the completion criterion: what does "done" look like? Derive it from the user's ask, or ask if genuinely ambiguous. Name it before starting the loop.

REPEAT:
1. Read the whole file to see its current state.
2. Find an improvement you can make towards achieving the criterion.
3. Make that one edit as cheaply as correctness allows. The reasoning must be cheaper than the edit: if the deliberation costs more than the change it produces, stop and make the obvious edit.

UNTIL the criterion is met.

Then switch instruments, because "the criterion is met" is not "it's consistent": run targeted invariant checks against the criterion — pick a specific global property you suspect might be violated and test it directly. One such check: does a full read surface no further ambiguity to reduce? If a check fails, fix it and return to the loop.

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