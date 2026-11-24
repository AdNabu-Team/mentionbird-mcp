# Action playbook

Rules here are industry-neutral. Square brackets mark the parts that change per business.

## What drives citations, in order of impact

1. **Mentions on sites the user doesn't own**, above all the listicles and comparisons the
   assistants already cite.
2. **Named attribution:** claims from a named person with a stated role.
3. **Specific numbers:** "[number] [unit] in [timeframe]" gets cited; adjectives don't. Original
   data is strongest.
4. **Answer-first structure:** the answer in the first two sentences of a section, headings
   phrased as the real question, tables, short FAQ blocks.
5. **Freshness**, where the category changes: a visible updated date and current details.
6. **Consistent naming** of the brand and what it offers.

Hygiene, never the top action: structured markup, crawler access, page speed. Counterproductive:
keyword stuffing, thin pages published for volume.

## Action types

| Action type | When | What a good recommendation says |
|---|---|---|
| **Improve existing page** | The user has a page close to the question (cited or not) | Which page; what it lacks against the cited pages (answer-first opening, question headings, comparison table, FAQ, numbers, named expert, updated date); the exact sections to add |
| **Create page** (comparison, listicle, guide, alternatives page) | Nothing the user owns fits, and the cited pages are mostly that type | The page type and why; title (with the year if the AI's searches use one); answer-first opening; sections from the AI's decision criteria; proof; link it from the closest existing page |
| **Get on a listicle** | A cited, non-competitor list leaves the brand out | The exact URL; competitors it names; the pitch angle; the contact route |
| **Move up a listicle** | The brand is in the bottom half of a cited list | New evidence the writer can use; never a paid upgrade |
| **Fix own content elsewhere** | The user's own video, post or profile on another site is cited but doesn't name or recommend their offering | What to change in the title, description or content |
| **Get listed or reviewed** | Review platforms, marketplaces or directories are cited | Claim and complete the profile; ask real customers for reviews; never incentivize ratings |
| **Join a forum thread** | Forum or Q&A threads are cited | Which thread; a useful answer with affiliation disclosed |
| **Pitch a review** | Single-item reviews on major outlets are cited | The outlet and the reviewer, with something verifiable to offer |
| **Video** | Video pages are cited | Pitch the creator, or create a video titled with the question |
| **Hygiene** | Something blocks access or freshness | Only when it's a real blocker; always last |

## Listicles

### What counts

A ranked or curated list page: "best [category] for [situation]", "top N [category]",
"[competitor] alternatives", "best [category] in [place]". In MentionBird these carry
`content_type_tags` containing `listicle`; comparison pages ("[A] vs [B]") are close cousins.

Two look-alikes need a different action:
- **Marketplace and directory category pages** are tagged as listicles, but getting there means
  ranking inside that marketplace or directory (complete profile, reviews, category choice).
- **Lists published by a competitor that ranks itself first** are low-feasibility. Never make a
  competitor-owned site an outreach target.

### Pitch angles

| Situation on the list | Angle |
|---|---|
| It doesn't cover a segment, use case, location, budget or audience the user serves | "Readers who need [segment] have no option here; we serve exactly that." |
| Details are out of date (prices, availability, features, providers that closed) | Send the corrections, plus the user's entry as a ready-to-paste block |
| It has "best for [X]" picks | Pitch for the "best for [X]" slot the user is strongest in, not for #1 |
| The writer says they test or try things | Offer something verifiable: a trial, sample, demo, visit or reference customer |
| It quotes experts or cites data | Offer a named quote with a credential, or a data point only the user has |
| The brand is in the bottom half | Send new evidence that gives the writer a reason to move it |

### Feasibility

Raise priority when the page was updated in the last 6 months or carries a year in its title, has
a named author with a contact route or a submission form, already lists smaller players, or comes
from a publisher with many similar lists.

Lower priority when the publisher ranks its own product first, every slot is visibly paid (note
it; the decision is the user's), the page is cited by one assistant only a few times, or the URL
is dead or redirects (`url_status` of `dead` or `redirect`).

### Brand status on a list

- **Absent:** the strongest target.
- **#N of M, bottom half:** a move-up target.
- **Top half:** reinforce. Keep the details current and send updates when something changes.

## Not pitchable

A platform's own help or developer docs (the assistant is answering from documentation, so a
how-to page on the user's own site is the play), encyclopedias (notability and conflict-of-
interest rules), and government or institutional sites (only through real partnerships).
