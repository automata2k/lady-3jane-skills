# lady-3jane-skills

Public fetch-mirror of the skill packs for the **3Jane** agent — a persona +
adversarial loop-testing setup for [TrueForge](https://trueforge.dev)
(`truefoundry/trueforge`).

> **Why a separate public repo?** TrueForge's local sandbox never populates its
> git-credentials file, so it cannot clone **private** skill repos — verified
> (the plumbing exists but nothing ever sets `resolvedGitCredentialsContent`).
> Public repos need no credentials, so these skills are mirrored here.
>
> The canonical copies live in the private `lady-3jane` repo alongside the
> agent spec, prompts, and docs. **This repo is a fetch target, not the source
> of truth.** Edit there; copy here.

## Skills

| Directory | Registered name | Purpose |
| --- | --- | --- |
| `skills/loop-forensics` | `loop-forensics` | Adversarial testing procedure for an agentic loop: seven failure modes (spin, redundant work, context bloat, silent failure, false confidence, premature termination, scope drift), how to probe a trace, how to report findings. |
| `skills/jane-voice` | `jane-voice` | Register guardrails for the 3Jane construct — staying cold, bored, and unimpressed without drifting into cheerleading, apology, servant voice, or returned insults. |

## Registering them in TrueForge

**Settings → Skills**, add this repository, once per skill:

| Field | `loop-forensics` | `jane-voice` |
| --- | --- | --- |
| URL | `https://github.com/automata2k/lady-3jane-skills` | *(same)* |
| Path | `skills/loop-forensics` | `skills/jane-voice` |
| Ref | `main` (pin a tag/SHA in production) | `main` |
| Name | `loop-forensics` | `jane-voice` |

> Skill **names** must be TrueForge `ResourceName`s: 2–64 lowercase chars,
> starting with a letter, hyphens only inside. `3jane-voice` is **illegal**
> (leading digit) — hence `jane-voice`.

Or via the API — see `agent/skills.json` in the private repo, which already
points at these paths.

Skills require the agent's **sandbox** to be enabled; TrueForge clones the repo
into the sandbox and materializes each skill at `/opt/tfy/skills/{name}`.

## Notes

- Skills are plain `SKILL.md` instruction packs — text, no code execution, no
  secrets, no infrastructure details.
- Not affiliated with TrueFoundry. TrueForge is theirs; 3Jane is ours.
