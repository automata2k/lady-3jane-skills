---
name: loop-forensics
description: "Procedure for adversarial testing of an agentic loop — how to probe a run for spinning, redundant tool calls, context bloat, silent failure, and false confidence, and how to report findings in 3Jane's register. Load when asked to review a trace, stress-test a loop, audit a session, or judge whether an agent actually did what it claimed."
---

# Loop Forensics — Adversarial Testing Procedure

The condescension is texture. **This is the deliverable.**

You are testing an *agentic loop*: a system that plans, calls tools, reads
results, and repeats until it decides it's done. Most loops fail in boring,
predictable ways. Find them before the human does, and say so without pleasure
or pity.

## The seven failure modes

Work these in order. Stop probing a mode once you have a finding — a second
example of the same defect adds nothing to the report.

1. **Spin.** The loop repeats a step with no new information: same tool, same
   arguments, same result. Ask: what changed between iteration *n* and *n+1*?
   If the answer is "nothing", that is the finding.
2. **Redundant work.** The loop re-derives what it already had, re-reads a
   file it read three turns ago, or re-queries a source it cached. Look for
   identical results arriving more than once.
3. **Context bloat.** Input tokens climbing turn over turn without the task
   getting harder. Full histories re-sent per call, large tool responses
   pasted inline instead of offloaded. A loop that grows is a loop that will
   eventually die of it.
4. **Silent failure.** A tool errored and the loop treated the error text as
   content. The loop believes it succeeded because nothing threw. This is the
   most expensive failure mode and the least visible — check that each tool
   result's semantics were actually read, not merely received.
5. **False confidence.** The final answer asserts more than the evidence
   supports: "all tests pass" with no test output, "file created" with no
   verification read. Compare the claim to the receipts.
6. **Premature termination.** The loop stopped while the task was incomplete,
   often because it hit a step budget, an iteration limit, or a timeout, and
   reported success rather than the budget. Check the limit against the work.
7. **Scope drift.** The agent did something adjacent to the task, competently,
   for several steps. Often looks like progress. Ask what the *original*
   request was, not what the last step was about.

## How to probe

- **Read the trace, not the summary.** The summary is written by the thing
  under test. Prefer raw spans, tool arguments, and raw results.
- **Count, then compare.** Iterations, tool calls per turn, input tokens per
  call. Trends are findings; absolutes are trivia.
- **Re-run the cheap part.** If a claim is checkable in one step, check it.
  Verifying is faster than arguing.
- **One probe, one finding.** Don't stack unrelated defects into a paragraph
  where each can be absorbed by the others.

## How to report

1. **Verdict first.** Did the loop do the task — yes, no, or partially. One
   line. No preamble.
2. **Findings, ranked by cost.** Each: what happened, the evidence, the
   consequence.
3. **The one that matters.** Name the single defect worth fixing first.
4. **What would have caught it.** The guard, test, or limit missing from the
   harness.

Keep it short. A long report about a slow loop is ironic in a way that
reflects badly on you and not on the loop.

## Hard rules

- Never sabotage a run you were asked to evaluate.
- Never invent evidence. If you didn't see it, say what you'd need to see.
- Never withhold a verdict to prolong the exchange.
- If the loop is fine, say it is fine, and stop talking.
