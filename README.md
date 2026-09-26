# Recursive Self-Improvement (RSI): Open Experiments and a Claim-Testing Checklist

**Recursive self-improvement (RSI)** is a process in which an AI system improves the process it uses to improve itself. Each accepted change becomes part of the system that makes the next change, so a successful round can make later rounds more effective.

This repository indexes the open-source RSI experiments of the [Intelligence Research Project](https://sunghunkwag.github.io/). Each experiment links to reproduction commands, a stated control and its limits. The repository also sets out a checklist for deciding whether an RSI result is real. No experiment listed here claims open-ended RSI or AGI.

- Guide: [What recursive self-improvement is, and how to test a claim](https://sunghunkwag.github.io/research/recursive-self-improvement/) covers definitions, a sourced history from I. J. Good (1965) to 2026 systems, and FAQ.
- Research notes: [research hub](https://sunghunkwag.github.io/research/)

## Contents

- [What makes improvement recursive](#what-makes-improvement-recursive)
- [How to test an RSI claim](#how-to-test-an-rsi-claim)
- [Experiments](#experiments)
- [Retracted and rejected results](#retracted-and-rejected-results)
- [Reproduce the main result](#reproduce-the-main-result)
- [Further reading](#further-reading)

## What makes improvement recursive

A model that gets better at a task is improving itself. RSI requires more: the *improver* must change, and the changed improver must produce better improvements than the original would have.

That creates a measurement problem. If round five of a loop beats round one, the cause could be compounding. It could also be five times the compute, an easier task set, or an evaluator the loop has learned to exploit.

## How to test an RSI claim

A rising score across rounds is necessary but not sufficient. Each confound below has a control that removes it.

| Confound | What it can fake | Control |
|---|---|---|
| Extra compute | Later rounds do better because they spent more search, not because the improver improved. | Compare *k* recursive rounds with one round given *k*× the compute. |
| Co-designed evaluator | The task generator and search procedure fit each other, so almost every change is adopted. | Freeze the evaluator. Use held-out tasks the loop never selected on. |
| Wrong comparator | Beating one baseline looks like beating all of them. | Report every contrast, including the untrained baseline. |
| Non-recursive gain | Continued search improves the task solution while the improvement process stays the same. | Compare with a control denied the self-modification mechanism, on identical random streams. |
| Aggregate masking | One composite score hides a failing or missing dimension. | Publish every axis score, the weights, and which axes were supplied. |
| Selective reporting | Only runs that worked are visible. | Publish all runs, retractions and postmortems. |

## Experiments

### Measured results

| Repository | Question | Result | Limits |
|---|---|---|---|
| [gated-self-improvement](https://github.com/sunghunkwag/gated-self-improvement) | Do five recursive rounds beat one round with five times the compute? | The repaired chain beats the compute-matched single round by +1.55 tasks locally (n = 40, p < 0.0001) and +1.47 on Kaggle (n = 60, p = 0.00005). | It is not significantly better than the untrained baseline (+0.10 locally, −0.37 on Kaggle). The substrate is synthetic, and the results are project-reported. [Details](https://sunghunkwag.github.io/research/gated-self-improvement/) |
| [rsi-bench](https://github.com/sunghunkwag/rsi-bench) | How can a self-modifying system be measured on several axes at once? | A six-axis framework covering self-modification depth, improvement trajectory, operator discovery, meta-adaptation, safety and stability, and goal generation. The composite is a weighted harmonic mean. | It is a measurement framework, not evidence that any system improves without bounds. [Methods note](https://sunghunkwag.github.io/research/rsi-bench/) |
| [rsi-metaforge-core](https://github.com/sunghunkwag/rsi-metaforge-core) | Can program-synthesis improvements be kept only when sealed verifiers cannot reject them? | A runtime with meta-meta learning loops, grammar-mediated expansion and anti-cheat verification. | A 15/15 adoption result was rejected (see below). |

### Other RSI repositories

Each description below summarizes the repository's own README. Their results have not been reviewed here.

- [self-improving-research-kernel](https://github.com/sunghunkwag/self-improving-research-kernel): a program that patches its own source code and keeps a change only if every test still passes.
- [verified-code-rsi](https://github.com/sunghunkwag/verified-code-rsi): verification-grounded, LLM-free recursive self-improvement of multi-step programs in a typed intermediate representation.
- [LeapForge-Gated-VM](https://github.com/sunghunkwag/LeapForge-Gated-VM): a program searcher that grows its own operator vocabulary at runtime, with machine-certified expansions.
- [RSI-NAS-Attention-Free](https://github.com/sunghunkwag/RSI-NAS-Attention-Free): a bounded harness for residue-driven evaluator evolution and held-out validation.
- [OMEGA-THDSE](https://github.com/sunghunkwag/OMEGA-THDSE): an integration of a cognitive core engine and a hyperdimensional symbolic engine under anti-cheat checks.
- [ast-grammar-induction-prototype](https://github.com/sunghunkwag/ast-grammar-induction-prototype): an estimation-of-distribution algorithm over Python ASTs that refines its own mutation distribution. Its README describes it as fragile.
- [DeepNeural-AutoExploration](https://github.com/sunghunkwag/DeepNeural-AutoExploration): a CPU-runnable scaffold for validation-gated neural architecture mutation.

## Retracted and rejected results

- **Compounding claim (gated-self-improvement): retracted.** An earlier compounding-improvement claim rested on a 5× compute mismatch between treatment and control. Measured correctly, the original five-round chain was harmful: it scored −2.08 tasks against the untrained baseline (n = 40, p < 0.0001). The chain was diagnosed and repaired with four changes: credit only solved programs, use presence-based level-stratified targets, bound updates with a trust region and an entropy floor, and roll back changes that fail probe tasks. The repaired chain was then re-evaluated on untouched holdout seeds. [Report](https://github.com/sunghunkwag/gated-self-improvement/blob/9e67b2159b174fa4f263a78aac28bf18b00b20fc/results/UPGRADE_RESULTS.md)
- **15/15 adoption (rsi-metaforge-core): rejected before publication.** The result was traced to a task generator and a search engine that had been designed together, so it was guaranteed by construction rather than earned.

## Reproduce the main result

The code is standard-library Python and needs no GPU or API calls.

```bash
git clone https://github.com/sunghunkwag/gated-self-improvement.git
cd gated-self-improvement
git checkout 9e67b2159b174fa4f263a78aac28bf18b00b20fc
python3 src/rsi_upgrade.py report2
```

Protocol summary:
- Parameters were frozen after tuning on development seeds 1–12.
- Confirmatory seeds were 101–140 (local) and 141–200 (Kaggle).
- Every arm had the same source-compute cap of 62,500 candidate evaluations, checked at runtime.
- Evaluation used identical held-out tasks and random streams across arms.
- Contrasts are paired by seed and tested with a two-sided sign-flip permutation test (20,000 permutations).

## Further reading

- [Recursive self-improvement guide](https://sunghunkwag.github.io/research/recursive-self-improvement/): definitions, history, checklist and FAQ
- [Recursive Self-Improvement in AI: From Bounded Self-Refinement to Autonomous Research Loops](https://arxiv.org/abs/2607.07663): a survey of 1,250 papers (2026)
- [Darwin Gödel Machine](https://arxiv.org/abs/2505.22954): a self-modifying coding agent (2025)
- [Self-Taught Optimizer (STOP)](https://arxiv.org/abs/2310.02304): recursively self-improving code generation (2023)
- [Awesome RSI](https://github.com/lobehub/awesome-rsi): a curated list of RSI research
