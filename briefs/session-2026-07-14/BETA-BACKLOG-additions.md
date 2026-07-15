# BACKLOG ADDITIONS — from Walk 2 (14 July)

Parked deliberately. Not Alpha. Captured so they are not lost.

---

## BETA SPIKE — PIPELINE LATENCY  *(Simeon: "still too slow, needs a proper spike")*

**The problem, measured** (project 725384bf, from `ai_usage_events`):
- A full roadmap generation = **~31 model calls, ~50s wall-clock** on nano; **~35 calls, 43s** on
  gpt-4.1.
- Confirmation save = **~7s server-side**, 15 serial inserts, each re-authenticating
  (`GET /auth/v1/user` before every POST).
- Perceived total from sign-in to a usable roadmap: **well over a minute.**

**Why it's a Beta spike, not an Alpha fix:** some of this is quick and belongs in Alpha (P2 in the
Walk-2 fix plan — parallelise the independent batches, stop the unwinnable DoD re-judging, build a
narrating loader, batch the inserts). But driving waiting time toward "as low as possible" is a
deeper optimisation problem — model routing per stage, caching, streaming partial results, possibly
restructuring the pipeline so the user sees themes appear as they're certified rather than waiting
for the whole batch. That is a dedicated investigation with its own before/after targets, and it
belongs to the monetisable-Beta quality bar, not the Alpha "does it work" bar.

**Scope when it runs:**
- Target: a hard wall-clock budget (e.g. themes visible in <10s, full roadmap in <20s) — set the
  number as part of the spike.
- Investigate: per-stage model routing (cheap where safe, strong where it matters — the routing
  framework applied INSIDE the pipeline); parallelisation beyond the obvious batches; streaming/
  progressive reveal so perceived wait ≠ total compute; insert batching + single auth.
- Prerequisite: the F33 redesign lands first — no point optimising a pipeline whose shape is about
  to change.

**Do NOT confuse with the Alpha P2 work.** Alpha P2 = "stop it being embarrassingly slow and stop
showing a fake loader." This spike = "make the wait genuinely short." Different bars.

---

## ALPHA-ADJACENT — MODEL ROUTING INSIDE THE PIPELINE

The whole pipeline defaults to `gpt-4.1-nano` (`responses.ts` OPENAI_MODEL default). Proven to
break the classifier. The immediate Alpha fix is to move the judgement-critical calls (classifier,
referee) onto a capable model. But the GENERAL principle — the routing framework applied per
pipeline stage, cheap model where it's safe, strong model where judgement lives — is a small design
job worth doing properly rather than a blanket model swap. Cost data: nano $0.002/run vs gpt-4.1
$0.055/run (~25×), but strong was FASTER wall-clock. So this is about correctness-per-stage, not
just cost.

---

## SMALL, REAL, NOT YET TICKETED

- **Fail-open judge (P5):** a task-judge call that FAILS currently ships the task unjudged. Fail
  closed / retry / flag — never silently ship. (Walk-2 fix plan P5.)
- **The 92% eval is missing.** The criterion validation everyone cites cannot be found in any repo.
  Either it resurfaces or the criteria are treated as unproven. (Feeds the F33 new-validation-set
  requirement.)
- **Placeholder-DoD inconsistency:** the client backfills a synthetic DoD only when a Next theme is
  corrected/renamed (`form.tsx:86-88`), so corrected Next themes get a DoD and untouched ones stay
  null. Resolve alongside the P4 "do Next themes get a DoD" decision.
