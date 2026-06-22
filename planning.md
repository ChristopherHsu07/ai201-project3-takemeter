# TakeMeter: Planning

## Community

**Source:** r/worldcup

I'm classifying discourse from World Cup 2026 match threads because the tournament is happening right now, which means an enormous, constantly-refreshing volume of real-time public commentary across dozens of games. 
## Labels

**Analysis** — A claim about the match supported by specific reasoning: tactics, a sequence of play, a stat, a pattern across the game. The tone can be excited or calm; what matters is whether there's a "because."
- Example 1: "Their fullbacks keep getting caught upfield on the counter — that's the third time Argentina has exploited that gap in 20 minutes."
- Example 2: "He's been man-marked out of the game since the 30th minute, that's why he hasn't touched the ball in the box."

**Hot Take** — A judgment, prediction, or accusation stated as fact, with little or no supporting reasoning. Often hyperbolic or absolute ("worst," "never," "should be fired," "this team has no chance").
- Example 1: "This referee should never officiate another World Cup game after that call."
- Example 2: "Brazil is the most overrated team in this tournament, they've done nothing all game."

**Hype/Reaction** — Pure emotional expression with no real claim attached. Celebration, despair, disbelief, in-the-moment exclamation.
- Example 1: "GOALLLLL LET'S GOOOOO"
- Example 2: "I can't believe what I just watched omg"

## Hard Edge Cases

The hardest boundary is **Hot Take vs. Hype/Reaction**, because both can carry the same emotional intensity. The rule I'm using: if the comment contains a judgment or claim about a team/player/ref/strategy — even a lazy, unsupported one — it's a Hot Take, not Hype. If it's purely expressive with no claim embedded, it's Hype.

Genuinely ambiguous example: *"VAR is an absolute disgrace, they're ruining this World Cup."* This reads emotionally like hype, but it makes a claim (VAR is ruining the tournament), so I'm calling it a Hot Take. My test going forward: can I rephrase the comment as "I believe ___ is true"? If yes, it's a take (Analysis or Hot Take, depending on whether reasoning is attached); if the comment doesn't reduce to a believable claim at all — it's just noise/emotion — it's Hype.

A second edge case I expect: short comments that *look* like analysis because they use tactical vocabulary but don't actually support a claim, e.g. "press is way too high right now." This names a tactical fact but doesn't connect it to a consequence or judgment. My rule: if it just describes what's happening with no "so what," it's not quite Analysis — I'll default it to Hot Take unless there's at least one connecting clause (cause, effect, or comparison).

## Data Collection Plan

I'll collect from r/worldcup match thread comments only, across multiple different matches to get variety in stakes and team affiliations. Target: roughly 65–70 examples per label (200+ total, no label above 70%). If a label is underrepresented after an initial pass — Analysis is the most likely candidate, since match threads skew toward reaction — I'll specifically pull from threads of closer, more tactically contested matches (where commentary tends to get more technical) rather than blowouts, which skew toward Hype/Hot Take.

## Evaluation Metrics

Accuracy alone isn't enough because the three classes won't be equally easy to separate, and a model that's accurate overall could still be systematically blind to one label (e.g., never correctly identifying Analysis because it's the smallest or most linguistically subtle class). I'll report:
- **Overall accuracy** for both models, to compare against the 33% random baseline for a 3-class task.
- **Per-class precision, recall, and F1**, since I specifically need to know if the model can distinguish Analysis (the label I care about most, since it's the rarest and most valuable signal) from the other two — a model that's accurate only because it's good at telling Hype apart from everything else isn't actually solving the interesting part of the problem.
- **A confusion matrix**, to see the *direction* of errors — I expect Analysis to get misclassified as Hot Take more than the reverse, since both involve evaluative language about the match.

## Definition of Success

I'd consider this genuinely useful if the fine-tuned model achieves at least 0.65 F1 on every class (no class collapsing to near-zero) and meaningfully beats the zero-shot baseline — at least 10 points of accuracy improvement. For "good enough to deploy" in a real community tool (e.g., auto-tagging match thread comments by take quality), I'd want Analysis recall specifically above 0.6, since the main value of this tool is surfacing the substantive comments in a sea of reactions — missing most of them would defeat the purpose, even if overall accuracy looked fine.

## AI Tool Plan

**Label stress-testing:** Before annotating, I'll give an LLM my three label definitions plus the Hot Take/Hype boundary rule and ask it to generate 8–10 World-Cup-match-thread-style comments designed to sit right on the Analysis/Hot Take and Hot Take/Hype boundaries. If I can't confidently label its outputs using my own definitions, I'll tighten the definitions before touching the real 200.

**Annotation assistance:** I plan to use an LLM to pre-label batches of ~20 comments at a time, using my exact label definitions from this document, then review and correct every label myself before moving to the next batch. I'll track this by adding a `pre_labeled_by_llm` notes column in my CSV during the review pass (removed or merged into the notes column before final submission) so I can disclose accurately which examples were touched by this workflow.
