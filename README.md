# fixed-point-iteration

Drive a text artifact (file, doc, code) to a fixed point: make one cheap, honest edit, re-read the whole thing, repeat until a full read surfaces no further ambiguity to reduce.

## Install

```
npx skills add MinskAndBoo/fixed-point-iteration
```

## Why it exists

LLMs over-generate and re-litigate. They keep editing after the work is done, second-guessing prior edits, and producing more tokens than the task requires. This skill drives the model to a fixed point so it stops.

## Why it works

Each edit does three things at once: removes the corrected element from the salient set, reshapes the terrain around its neighbors, and improves resolution on the remaining targets. Every read recomputes the gradient against the new state.

## Counter-intuitive insights

1. **Read-after-edit is required, not optional.** The model has to re-read the file after each edit to actually *see* the change and pick the next edit. Many harnesses discourage the re-read as inefficient — the skill works better when you do it.
2. **Knocking edits off improves downstream edits.** Each edit reshapes the attention map, so the next edit is clearer.
3. **Stopping conditions are required to be able to start.** You need a defined "done" (a fixed point) to know when to begin and when to stop.
4. **You do not quote back the behaviors that led to excessive token generation.** Naming the model's over-generation to it draws attention to the elephant in the room and amplifies it — the fix is applied silently, not called out.

## License

MIT