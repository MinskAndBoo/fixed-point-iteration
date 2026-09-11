# fixed-point-iteration

Drive a text artifact (file, doc, code) to a fixed point: make one cheap, honest edit, re-read the whole thing, repeat until a full read surfaces no further ambiguity to reduce.

## Install

```
npx skills add MinskAndBoo/fixed-point-iteration
```

## What it does

The skill applies a fixed-point iteration loop to text editing: read the whole file, make one cheap edit, re-read, repeat. The re-read is what makes each pass honest — it refreshes what you attend to, so the next edit is made against the file as it actually is, not as you remember it.

## Why it works

Each edit does three things at once: removes the corrected element from the salient set, reshapes the terrain around its neighbors, and improves resolution on the remaining targets. Every read recomputes the gradient against the new state.

## License

MIT