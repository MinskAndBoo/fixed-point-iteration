---
name: fixed-point-iteration
description: "Default editing discipline for any text artifact (file, doc, code, config, prompt) or multi-edit task: drive it to a fixed point with cheap, honest, one-at-a-time edits. The loop prevents batching, re-litigation, and over-generation."
---

# Fixed-Point Iteration

Fixed-point iteration: establish a completion criterion, then apply a minimal transformation (an edit), re-observe the state (a re-read), and repeat until no improvement is found, then verify the criterion. The re-read is what makes each pass honest: it refreshes what you attend to, so the next edit is made against the file as it actually is, not as you remember it.

An edit is *honest* when it is a correct edit made correctly: made against the file as it actually is (from the re-read), not from memory or assumption, and genuinely advancing the criterion rather than being a cosmetic change that looks like progress.

The name is the logic; the physics is the explanation. "Fixed-point iteration" names the loop; "LLM attention map" is the physics that explains why it converges.

## Why it converges

Each edit does three things at once:
(a) removes the corrected element from the salient set (progress on the work).
(b) reshapes the terrain around its neighbors (improving clarity): a bump next to a large peak becomes a visible hill once the offending peak is gone. Similarly, an ambiguous large bump may resolve into a set of sharp, focused targets once confounding features are gone.
(c) improves resolution on the remaining potential targets (improving clarity): the proportion of the bounded attention allocated to each target increases, so each remaining issue is seen more sharply and the easy wins come faster.

So there are three reasons why every correct edit, no matter how minor, is a good edit. Every read recomputes the gradient against the new state, making the hill visible on the LLM attention map. The edits drain the salient basin — the inconsistencies with local contrast, where most of the real problems live.

The re-read can be localized to the region around the edit — reasonable context, up to ~25 lines before and after — because attention is global: a localized change updates weights across the whole context, including the original file. You mentally patch the updated piece into the original, the same way a human does. Seeing the whole file in its final form is stronger (you see the result, not a diff), but the localized re-read works. Not re-reading at all also sort of works (weakest), because you still mentally patch.

## When to use

Any time you are making edits to a text artifact — file, doc, code, config, prompt. This is the default editing discipline, not a special polishing mode. Specifically:

- Driving a piece of work to a fixed point by making cheap, honest, one-at-a-time edits.
- Polishing a draft: the gap between 80% and 90% is the ambiguities you leave behind, and each cheap round halves them.
- Tightening a document for its next reader.
- Any multi-edit task where the model is tempted to batch, re-litigate, or over-generate.
- Sidestepping the failure modes that derail improvement (see anti-patterns).

The trigger is a non-trivial edit: as soon as an edit is perceived to be non-trivial, the skill is required. A trivial edit — a one-line fix, a typo, an obvious rename — doesn't need the loop; just make the change.

## Practiced discipline

0. Establish the completion criterion: what does "done" look like? Derive it from the user's ask, or ask if genuinely ambiguous. The criterion must be checkable — if you can't tell whether it's met, it's not a criterion. Name it before starting the loop, and state it out loud so your human partner can override it — the criterion is the contract, and a wrong one wastes the whole loop. If you must ask, ask one sharp question, not a batch.

REPEAT:
1. Read the file to see its current state. On the first pass, read the whole file. After each edit, re-read the region around the edit — reasonable context, up to ~25 lines before and after.
2. Find an improvement you can make towards achieving the criterion.
3. Make that one edit as cheaply as correctness allows. The reasoning must be cheaper than the edit: if the deliberation costs more than the change it produces, stop and make the obvious edit.

UNTIL no improvement is found.

For a multi-file artifact, define the completion criterion at the artifact level, then drive each file to a fixed point in turn. The re-read is the region around the edit in the edited file(s). A change that spans files is still one edit if it's one logical change.

Then switch instruments, because "the criterion is met" is not "it's consistent": run targeted invariant checks against the criterion — always run at least one, even if you suspect nothing; pick a specific global property you suspect might be violated (or the one most central to the criterion) and test it directly — for code, run it (or the relevant test). One such check for a doc: does a full read surface no further ambiguity to reduce? If a check fails, fix it and return to the loop.

If you can't find an improvement, the loop has drained — but drained is not met. The invariant checks are the arbiter: if they pass, the criterion is met (or as close as it gets) — stop and report the criterion and what you did. When you're a subagent, that report is your handoff to the delegator: state the criterion you were given and whether it's met, so the delegator can verify without re-deriving it. If they fail and no fix advances the criterion, you're stuck — the escape hatch is to ask the user or surface the blocker. Don't try to solve the insoluble yourself by sharpening or re-deriving the criterion; misalignment and user mistakes happen, and "what do you mean by X?" is the correct response.

## Anti-patterns

| Urge | Move |
|------|------|
| Compaction-fear | Files are immune to compaction. Keep editing. |
| Drafting | Edit the file. It is the work surface; edits are cheap, reversible, and the tool is reliable. |
| Sequencing | Not entirely moot. Start with the least consequential, most isolated edit — it's cheap to get right, and clearing it sharpens the larger issues that remain. Then work toward the bigger, more entangled ones. |
| Candidate-hunting | Any candidate is good. Pick one and edit; the edit reshapes the attention map so the next choice is clearer. |
| Batching | One edit per pass — one logical change, a single coherent modification that serves one purpose. It may span multiple lines or locations (a rename, a refactor of one function, a fix touching several call sites) as long as it is one change. The rule forbids bundling *unrelated* changes, not a single change that touches several places. Test: can you state the change in one sentence? If yes, it's one edit. The loop does the rest. |
| Reconsidering | Nothing is gained by reconsidering. Any correct edit advances and clarifies the task. |
| Over-thinking | The reasoning must be cheaper than the edit. If it isn't, make the obvious edit. |
| Premature stopping | Don't stop because "it's good enough" — stop only when the criterion is met or no improvement is found. "Good enough" is not the criterion. |
| Re-reading the same issue | If a re-read surfaces a candidate you already weighed, you've stopped re-observing and started re-deliberating. Make the edit or drop the candidate. |
| Oscillation | If an edit undoes a previous edit, the two "improvements" contradict — they can't both be right. Don't ping-pong; stop and surface the conflict (escape hatch). |