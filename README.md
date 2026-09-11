# fixed-point-iteration

Drive a text artifact (file, doc, code) to a fixed point: make one cheap, honest edit, re-read the whole thing, repeat until a full read surfaces no further ambiguity to reduce.

## Install

```
npx skills add MinskAndBoo/fixed-point-iteration
```

## Why it exists

LLMs over-generate and re-litigate. They keep thinking after they have the conclusion, second-guess prior drafts and produce more tokens than the task requires. This skill drives the model to a fixed point so it stops. The process of iterative editing also causes more nuanced problems to become apparent after more clear-cut ones are fixed. It's hard to assess the flow and logical details in a typo-ridden mess. LLMs have fixed attention budgets. The fewer problems remain, the more attention is focused on the remaining issues.

## Built for

As of September 2026, Qwen 3.8 is the best local coding model on a 4090 (100K+ context), but it has a known catch: absurd token generation and endless re-litigation. This skill is the fix. It works on any model, but the problem is most acute here.

## Why it works

Each edit does three things at once: fixes the problem in the file, reshapes the terrain around the problem's neighbors, and improves resolution on the remaining targets. Every read recomputes the gradient against the new state.

Or, in more words, take it from the LLM's thinking: "Wait, is salience-first actually sound? Could there be an inconsistency that's NOT salient — one that doesn't 'pop'? Yes, in principle. A subtle inconsistency that's locally coherent but globally contradictory might not pop out on a single scan. But here's the thing: the loop re-reads each pass, and each pass is a fresh attention pass. Over multiple passes, different elements can become salient as the context shifts (you've fixed other things, so the relative contrast changes). So the loop has a kind of 'annealing' property — each fix changes the landscape, making previously-subtle inconsistencies more salient. This is actually a nice property: fixing the loud inconsistencies first reduces the 'noise,' making the quieter ones pop out on later passes.

So the user's insight is not just correct but deeper than they might realize: the re-read-each-pass + first-to-mind combination gives you a convergent search where each iteration both (a) grounds you in the true current state and (b) the act of fixing reduces the salience of what you fixed, letting the next-most-illogical element rise to the top. It's like gradient descent where each step reduces the 'illogic energy' and the re-read recomputes the gradient.

Let me also connect to the 'attention map' framing more precisely. The user is using 'attention map' in the transformer sense — the model attends to all tokens simultaneously, and the 'map' is the distribution of attention. When you re-read the file, you're re-injecting the full current file into context, which resets the attention map to reflect the true current state. If you only had source+diff, the attention map would be over a representation that doesn't match reality (the diff is sparse, the source is stale). So the re-read is what keeps the attention map honest."

## Limitations

I have not yet been successful at taming xhigh thinking. It still takes too long before it will make the first edit, but on medium thinking this skill works well in my experience. Prompts are still king. What do you want, how do you want it, subject to what constraints. Qwen 3.8 is a logic engine, that's why it codes well. If the ask is ambiguous or internally contradictory then no skill is going to save you (except for the one I'm working on that hopefully will).

## Counter-intuitive insights

1. **Read-after-edit is required, not optional.** The model has to re-read the file after each edit to actually *see* the change and pick the next edit. Many harnesses discourage the re-read as inefficient — the skill works better when you do it.
2. **Knocking edits off improves downstream edits.** Each edit reshapes the attention map, so the next edit is clearer.
3. **Stopping conditions are required to be able to start.** You need a defined "done" (a fixed point) to know when to begin and when to stop.
4. **You do not quote back the behaviors that led to excessive token generation.** Naming the model's over-generation to it draws attention to the elephant in the room and amplifies it — the fix is applied silently, not called out.

## Notes

- **The skill edited itself.** It was used extensively to edit its own file — read the file, apply its discipline, re-read, repeat. The dogfooding is the proof that the loop works.
- **Qwen 3.8 (non-thinking mode) as an unblocker.** The hard-to-write sections were unblocked by running Qwen 3.8 in non-thinking mode against the draft — its raw, un-reflected output surfaced phrasings the thinking mode kept over-polishing.

## License

CC0 Public Domain (Attribution appreciated)
