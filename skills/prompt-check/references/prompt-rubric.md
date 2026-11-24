# Prompt rubric

Use this to judge a prompt from its wording (Estimated mode, or prompts with too few runs), and to
write rewrites in either mode. Every rule is industry-neutral. Square brackets mark the parts that
change per business: [category] is what the user offers, [situation] and [audience] are who it's
for and when.

## The core test

**Could a thorough answer be written without naming a single brand?** If yes, the prompt won't
measure visibility. A second test separates useful prompts from idle ones: **if an assistant
answered this well, would a real customer be closer to choosing?**

## The five checks

### 1. It asks for a choice, not an explanation

Wording that asks *which*, *best*, *who should I use* or *alternatives to* makes assistants look
things up and name providers. Definitions and how-to questions are usually answered from memory,
with no sources and no brands.

**The trap: comparing concepts instead of choices.** "[Approach A] vs [approach B]" compares
*ideas*, and a thorough answer names no providers. "Should I choose [approach A] or [approach B]
for [situation], and who does it best?" compares *choices*. Concept-vs-concept prompts look like
comparisons and are the most common weak prompt.

Shapes that reliably bring up brands:

- **Best [category] for [situation]**
- **Which [category] should a [audience] choose?**
- **Who should I use for [need] in [place]?** (for local and service businesses)
- **Most recommended [category] for [need]**
- **Is [type of offering] worth it, and which are best?**
- **Alternatives to [competitor] for [situation].** The one case where naming a competitor is
  fine, because customers really ask it this way.

### 2. It doesn't name the user's own brand

"Is [your brand] good for [need]?" guarantees the brand appears because the prompt put it there,
so it measures nothing. Track the category question instead. Brand-name prompts are only useful
for checking how the brand is *described*, not whether it gets recommended.

### 3. It has no year

"Best [category] [year]" drifts out of date when the calendar turns, and a trend line built on it
ends up comparing different questions. Use "right now" if recency matters.

### 4. It's in customer language

Write the prompt the way a customer would say it (their problem, their situation, plain words),
not in marketing or industry jargon. A good sign: the phrase turns up in customer emails, calls,
reviews or forum posts.

### 5. It asks for a provider, not the task

"Write me a [document]", "[thing] generator for [audience]", "template for [task]" and "plan my
[task]" get the assistant *doing the job* instead of recommending who should. Ask which tool,
service or company to use: "Which [category] helps [audience] [do the task]?"

## Ambiguous words

Some category names mean different things to different assistants, and the answers then name
brands from unrelated categories. Pin the meaning by adding the job the user's category does, or
who it's for: "[ambiguous term] for [the specific job]".

## Verdicts from wording

- **Good:** passes checks 1, 2 and 5. Checks 3 and 4 are fix-ups, not failures.
- **Mixed:** a choice is implied but not asked for ("Tips for choosing a [category]"), or it's a
  choice between concepts with a provider choice one step away.
- **Weak:** fails check 1 (definition, how-to, why, concept vs concept), check 2, or check 5.

## Rewrite patterns

| Pattern | Before | After |
|---|---|---|
| Add the choice | How do I [task]? | How do I [task], and which [category] should I use? |
| Concept to choice | [Approach A] vs [approach B] | Which [category] should a [audience] use for [approach A]? |
| Remove own brand | Is [your brand] good for [need]? | What's the best [category] for [need]? |
| Remove the year | Best [category] [year] | Best [category] for [situation] right now |
| Customer language | [Category] solutions for [jargon segment] | What [category] should a [plain-language customer] use for [need]? |
| Provider, not task | [Task] generator for [audience] | Which [category] helps [audience] [task]? |
| Pin the meaning | [Ambiguous term] | [Ambiguous term] for [the specific job] |
