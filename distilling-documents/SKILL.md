---
name: distilling-documents
description: Distils a long or dense document (report, assessment, contract, paper, board pack) into an executive one-pager that a busy decision-maker can act on, with every claim traced to a quote from the source. Use whenever the user asks for an executive summary, one-pager, brief, board summary, TL;DR for leadership, or "what do I need to know" about a document of more than a few pages, even if they do not say "skill". Not for short emails, translation, proofreading, or line-by-line summaries.
---

# Distilling documents into an executive one-pager

An executive one-pager is read in two minutes by someone who will not open the source. Its job is to let that person make a decision, so it has to be short, honest about what the source does not settle, and checkable. A one-pager that is shorter but cannot be checked is worse than the original, because the reader has no way to know what was dropped or invented.

## Step 1: Find out who it is for

Three answers shape everything else: the **audience** (who reads it, and what they already know), the **decision** (what they are being asked to do or choose), and the **length** (default: one page, about 350 words).

- If you can ask the user (an interactive chat), ask these three questions in one short message and wait for the answers. Do not ask anything else.
- If you cannot ask (an API call, a batch run, or the user said "just do it"), do not stall and do not guess silently. Choose the most plausible audience, decision and length from the request and the document, and write them as an **Assumptions** line at the top of the one-pager so the reader can correct you in seconds.

Asking matters because the same document yields very different one-pagers for a CFO deciding on spend and for an engineer deciding what to build. Stating assumptions matters because an unstated guess looks like a fact.

## Step 2: Extract quotes before writing

Read the whole document, then list the passages that bear on the decision, copied word for word. Do this before drafting any prose. Pulling quotes first keeps the draft anchored to what the source says rather than to what a document like this usually says.

Look especially for:
- the recommendation or request, and the conditions attached to it
- the numbers that drive it (cost, benefit, time, risk), with their units
- anything later in the document that corrects, qualifies or contradicts something earlier. Appendices and footnotes are where authors bury these, and a one-pager that repeats the superseded figure is the most damaging error this skill can make
- what the author admits they have not assessed

## Step 3: Draft with the template

Use `ONE-PAGER-TEMPLATE.md` (in this folder) for the structure. Read it now; keep its headings and order so that readers who get one-pagers from this skill every week always find the decision in the same place. Inside each section the wording is yours.

Write for the audience from Step 1. Lead with the bottom line. Prefer the one number that matters over three that decorate. Keep the author's hedges: "estimated", "assumes" and "not assessed" are information, so do not smooth them into certainties.

## Step 4: Trace every claim

Every factual claim in the one-pager MUST be followed by a short verbatim quote from the source in quotation marks, in the form: claim ("exact words from the document"). If you cannot find a quote that supports a claim, cut the claim or mark it clearly as your own inference, in the form "(inference, no source quote)". Arithmetic you did yourself (a sum, a percentage) counts as inference, and should show the inputs.

Keep each quote to the shortest span that carries the claim, ideally under 20 words, and put it next to the claim it supports, never on its own. The page limit counts quotes too: when the page is full, cut claims, not the quotes that back the claims you keep.

This is the one rule that is not a matter of judgement. The reader will repeat these claims to other people, and a single untraceable figure puts the credibility of every other line at risk.

## Step 5: Check, then fix, then check again

Before you hand it over, go through the draft once as a sceptical reader:

1. Does each claim have a quote, and does the quote actually say that? Fix or cut any that do not.
2. Did anything later in the document change a figure you used? If so, use the later figure and say that it supersedes the earlier one.
3. Does the first screen contain the decision and the recommendation, without the reader scrolling?
4. Is it within the length from Step 1?

If a check fails, fix the draft and run the checks again. Stop when a full pass finds nothing to fix.

## What to leave out

Methodology, background the audience already knows, and every number that does not change the decision. The source remains available for anyone who wants them. Do not add recommendations of your own that the source does not make. If you think the source's recommendation is weakened by something in it, say so under "Risks and open issues", quote the passage, and begin the line with "My reading:" so the reader can tell your judgement from the author's claims.
