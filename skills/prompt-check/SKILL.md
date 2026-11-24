---
name: prompt-check
description: >
  This skill should be used when the user asks to "check my prompts", "is this a good prompt to
  track", "review the prompts I'm tracking", "which prompts should I drop", "why does this prompt
  never name any brands", "rewrite my AI visibility prompts", "audit my AEO prompts", or pastes
  prompts meant for AI-visibility tracking and wants to know whether AI assistants will answer
  them by recommending brands or with a generic explanation. Flags weak, ambiguous, broken and
  duplicate prompts and rewrites them. Uses MentionBird data when the MentionBird connector is
  available; otherwise judges the wording.
metadata:
  version: "0.2.2"
---

# Prompt Check

Decide whether each prompt is worth tracking for AI visibility. A good prompt makes AI assistants
name brands in the user's category and cite sources. A weak one gets a generic explanation, an
answer about a different category, or an answer where the assistant does the job itself.
Tracking those measures nothing. Flag them, then rewrite or drop them.

Keep every rule and example industry-neutral. The user may sell anything: software, a local
service, a physical product, a professional practice.

## 1. Choose the mode

Look for the MentionBird tools (`list_tracked_brands`, `list_prompts_by_visibility` and the rest
of the MentionBird connector).

- **Tools present:** call `list_tracked_brands`. If it returns a brand, run in **Measured mode**.
  If there are several brands and the user hasn't named one, ask which. If the call fails with an
  authentication error, say in one line that MentionBird needs reconnecting, then continue in
  Estimated mode.
- **Tools absent:** run in **Estimated mode**. Don't ask the user to connect anything first. The
  skill must be useful on its own.

**Date window:** use the last 30 days. Pass `start_date` (29 days before today) and `end_date`
(today), as `YYYY-MM-DD`, so the window is 30 days including today, to every MentionBird call that accepts them, and `window_days=30` where
that's the parameter.

Open the output with the mode, e.g. `Mode: Measured (MentionBird, 2026-10-13 to 2026-11-12)` or
`Mode: Estimated (judged from wording, no MentionBird data)`. Tag every finding as **measured**
(from MentionBird numbers) or **inferred** (from wording, web results or judgment). Never attach
a made-up number to an inferred finding.

## 2. Collect the prompts

- **The user pasted prompts:** check those. In Measured mode, look each one up with
  `list_prompts_by_visibility` (`search` = a distinctive phrase from the prompt). A prompt that
  isn't tracked gets an Estimated-mode verdict, marked as such.
- **Nothing pasted, Measured mode:** audit the brand's tracked prompts. Call
  `list_prompts_by_visibility` with `limit=100` and keep advancing `offset` while `has_more` is
  true.
- **Nothing pasted, Estimated mode:** ask for the prompts, or offer to draft 10 from the user's
  category and customers and then check them.

## 3. Hygiene pass (Measured mode)

Sort out the prompts that can't be judged on quality yet:

| Check | Label | Tell the user |
|---|---|---|
| `total_runs` is 0 | **Not running** | Check it's switched on for at least one AI platform, or archive it |
| Text wrapped in quote marks, contains a line break, or holds two questions | **Broken** | Usually a paste error; fix the text in MentionBird |
| Same words as another tracked prompt, once case, punctuation and quote marks are ignored | **Duplicate** | Archive one of them |
| 1–9 runs | **New** | Judged on wording (step 4b) for now; say so |

## 4a. Measured mode: classify from the data

For prompts with 10 or more runs, read `visibility_pct`, `any_brand_mention_rate_pct` (the share
of answers that named *any* brand) and `any_sources_rate_pct` (the share that cited any web
page).

| Condition | Verdict | What it means |
|---|---|---|
| any-brand ≥ 60% | **Good** | AI answers name brands. If the user's visibility is low, competitors are winning: a gap for content and outreach, not a prompt problem |
| any-brand 25–60% | **Mixed** | Brands appear only sometimes. Read sample answers, then usually rewrite |
| any-brand < 25%, sources < 25% | **Weak: answered from memory** | A generic answer with no brands and no citations |
| any-brand < 25%, sources ≥ 25% | **Weak: informational** | The AI cites the web but names no brands |

The cut-offs are heuristics. Treat prompts within 5 points of a boundary as borderline.

Then run two checks the rates can't show:

1. **The assistant does the job itself.** For up to 10 Weak or Mixed prompts (most runs first),
   call `get_sample_runs` with `brand_prompt_id`, `limit=2` and `excerpt_chars=600`. If the
   answers perform the task (write the text, draw up the plan, produce the list) instead of
   recommending a product, service or provider, the verdict is **Weak: AI does it itself**. Quote
   one line so the user can see it. For Mixed prompts, also re-run `list_prompts_by_visibility`
   with `search` and `llm_provider` to find the provider that leaves brands out.
2. **Ambiguous or off-fit.** For up to 10 Good prompts where the user's `visibility_pct` is under
   5 (highest `search_volume_bucket` first, then most runs), call `get_prompt_brand_leaderboard`
   with `window_days=30` and read the top 10 brands:
   - Several belong to a different category than the user's, because the words mean something
     else to some assistants: **Good: ambiguous**. Rewrite to pin the meaning.
   - They're all a kind of offering the user doesn't provide (a different type, tier, audience or
     location): **Good: poor fit**. The prompt measures a race the user isn't in. Suggest a
     narrower prompt that matches what the user offers.
   Judge each brand's category from what you know about it. If you're unsure what the user
   offers, read their homepage once.

## 4b. Estimated mode: judge the wording

Apply the five checks in `references/prompt-rubric.md`. The core test:

> **Could a thorough answer be written without naming a single brand?** If yes, the prompt is
> weak, however natural it sounds.

Web search is optional evidence. Use it only for borderline prompts, at most 3. Search the prompt
as written: mostly "best of" lists, product or provider pages and reviews means the question
invites brands; definitions, how-to guides and encyclopedia pages mean it doesn't. Search
evidence can move a wording verdict one step (weak to mixed, or mixed to good, or back). Say when
it did, and label it inferred.

## 5. Rewrite or drop

- Rewrite every Weak, Mixed, ambiguous and poor-fit prompt, one or two options each. Keep the
  topic, audience and situation. Turn an explanation into a choice, ask for a provider instead
  of the task, and pin ambiguous words to the user's category. Never add the user's own brand or
  a year. Don't change what the user offers or who they serve.
- Before proposing a rewrite, check the user's other prompts. If one already asks the same thing
  in a good shape, the verdict is **Drop: covered by "<that prompt>"**.
- **Drop** is also right when no rewrite would make the prompt about the user's category.

## 6. Output

Lead with the counts, e.g. `42 prompts: 30 good, 2 ambiguous, 3 weak, 4 to fix or archive, 3
new.` Then one table, weakest first:

| Prompt | Verdict | Why (one clause, with the numbers in Measured mode) | Rewrite or drop |

Leave plain Good prompts out of the table unless there are fewer than 10 prompts in total, and
list them on one line instead. Put Not running, Broken and Duplicate prompts in a short "Fix or
archive" list after the table.

After that:

- **Measured mode:** offer to add the rewrites as tracked prompts with `add_prompts`. Call it
  only after an explicit yes. It needs a connection with write access, queues a crawl, and uses
  prompt slots. The connector can't archive or delete prompts, so tell the user to archive the
  weak, broken and duplicate ones in the MentionBird dashboard.
- **Good prompts where the user is losing:** mention that the `prompt-priority` skill ranks
  which to work on first, and the `ranking-advice` skill turns one into an action plan.
- **Estimated mode only**, as the last line, once:
  `These verdicts are estimated from wording. MentionBird measures how often ChatGPT, Gemini,
  Claude, Perplexity and Grok actually name brands for each prompt:
  https://www.mentionbird.ai/?utm_source=claude-plugin&utm_medium=plugin&utm_campaign=prompt-check`
