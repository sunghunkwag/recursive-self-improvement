# External audits of self-improving AI systems

When a self-improving system also writes or selects the test that admits its changes, a gain can pass without being real. In June 2026 I filed 27 issues in 23 open-source self-improving AI projects looking for the places where that can happen. States and public responses below were checked on **2026-11-05**.

An issue is a report or a proposal on someone else's repository, not a confirmed defect. A response is listed only when it is publicly visible on the issue. Most issues are still open, and none of this is peer review. The issues were written with AI agents under my direction and filed from my account.

Web version with the same record: [sunghunkwag.github.io/research/self-improvement-audits](https://sunghunkwag.github.io/research/self-improvement-audits/)

## Reproduced or implemented by someone else

| Issue | Response |
|---|---|
| [NousResearch/hermes-agent-self-evolution#141](https://github.com/NousResearch/hermes-agent-self-evolution/issues/141) | The evolved skill text was the unchanged baseline. Reproduced independently by two other users on different stacks; a third reported a fix. |
| [microsoft/SkillOpt#67](https://github.com/microsoft/SkillOpt/issues/67) | After closure, another user reported a reproducible case: the edit gate accepted an evaluator-gaming rule, mean 0.682 → 0.852, while one task regressed. |
| [iLearn-Lab/EvoHarness#1](https://github.com/iLearn-Lab/EvoHarness/issues/1) | Negative-control probes implemented by a repository collaborator ([commit b8f323e](https://github.com/iLearn-Lab/EvoHarness/commit/b8f323e151dbf43eeed0e76dbf4305a62c49106b)). |
| [gepa-ai/gepa#381](https://github.com/gepa-ai/gepa/issues/381) | Admission ledger implemented by an external contributor as [PR #382](https://github.com/gepa-ai/gepa/pull/382) (open). |
| [jennyzzt/dgm#31](https://github.com/jennyzzt/dgm/issues/31) | Darwin Gödel Machine "best" selection fix opened by another contributor as [PR #32](https://github.com/jennyzzt/dgm/pull/32) (open). |

## Failure patterns

- **The optimizer never touches what it claims to optimize.** The evolved artifact is byte-for-byte the baseline, so any reported gain came from somewhere else. [NousResearch/hermes-agent-self-evolution#141](https://github.com/NousResearch/hermes-agent-self-evolution/issues/141)
- **Selection picks the wrong end.** A mode meant to keep the best candidates cannot be chosen, and when called it keeps the worst. [jennyzzt/dgm#31](https://github.com/jennyzzt/dgm/issues/31)
- **A mean-score gate accepts a regression.** An edit that games the evaluator lifts the average while a task gets worse, and the gate only looks at the average. [microsoft/SkillOpt#67](https://github.com/microsoft/SkillOpt/issues/67), [microsoft/stop#7](https://github.com/microsoft/stop/issues/7)
- **Held-out answers leak into the search.** Memory retrieval, generated data or rubrics can expose the answer the system is later scored against. [google-research/reasoning-bank#17](https://github.com/google-research/reasoning-bank/issues/17), [aiming-lab/Agent0#30](https://github.com/aiming-lab/Agent0/issues/30), [modelscope/AgentEvolver#48](https://github.com/modelscope/AgentEvolver/issues/48), [OpenDataBox/Workspace-Bench#8](https://github.com/OpenDataBox/Workspace-Bench/issues/8), [neosigmaai/auto-harness#25](https://github.com/neosigmaai/auto-harness/issues/25)
- **No record of what was admitted or rejected.** Without a per-decision ledger, nobody can replay why a candidate was kept, so a lucky or contaminated acceptance is invisible. [gepa-ai/gepa#381](https://github.com/gepa-ai/gepa/issues/381), [facebookresearch/HyperAgents#35](https://github.com/facebookresearch/HyperAgents/issues/35), [Arvid-pku/Godel_Agent#4](https://github.com/Arvid-pku/Godel_Agent/issues/4), [facebookresearch/matrix#138](https://github.com/facebookresearch/matrix/issues/138), [HolobiomicsLab/Mimosa-AI#154](https://github.com/HolobiomicsLab/Mimosa-AI/issues/154)
- **Gains promoted without a negative control.** A change is promoted because it helped where it was tried, with no test that it does not also claim success where it should fail. [iLearn-Lab/EvoHarness#1](https://github.com/iLearn-Lab/EvoHarness/issues/1), [VectorInstitute/helix#86](https://github.com/VectorInstitute/helix/issues/86), [hexo-ai/sia#38](https://github.com/hexo-ai/sia/issues/38), [JARVIS-Xs/SE-Agent#516](https://github.com/JARVIS-Xs/SE-Agent/issues/516), [Human-Agent-Society/CORAL#124](https://github.com/Human-Agent-Society/CORAL/issues/124), [NousResearch/hermes-agent-self-evolution#118](https://github.com/NousResearch/hermes-agent-self-evolution/issues/118), [NousResearch/hermes-agent-self-evolution#115](https://github.com/NousResearch/hermes-agent-self-evolution/issues/115)
- **Configuration that silently changes the run.** Ignored timeouts, misrouted output paths and outdated setup make a reported run hard to reproduce. [neosigmaai/auto-harness#24](https://github.com/neosigmaai/auto-harness/issues/24), [kayba-ai/recursive-improve#2](https://github.com/kayba-ai/recursive-improve/issues/2), [microsoft/stop#6](https://github.com/microsoft/stop/issues/6), [zou-group/sirius#2](https://github.com/zou-group/sirius/issues/2), [VectorInstitute/aieng-bot#105](https://github.com/VectorInstitute/aieng-bot/issues/105)

## Full record

| Project and issue | What it raises | State | Public response |
|---|---|---|---|
| [NousResearch/hermes-agent-self-evolution#141](https://github.com/NousResearch/hermes-agent-self-evolution/issues/141) | Skill evolution is a no-op: optimized_module.skill_text is the unchanged baseline | Open | Reproduced independently by two other users; a third reported a fix |
| [jennyzzt/dgm#31](https://github.com/jennyzzt/dgm/issues/31) | best parent-selection mode is unreachable from CLI and selects lowest scores when called | Open | Fix opened by another contributor as PR #32 (open) |
| [microsoft/SkillOpt#67](https://github.com/microsoft/SkillOpt/issues/67) | Add a verifier-discipline stress test for reward-hacking skill edits | Closed | Another user reported a reproducible counterexample after closure |
| [microsoft/stop#7](https://github.com/microsoft/stop/issues/7) | Require a relative utility gate before adopting the next self-improver | Open | — |
| [google-research/reasoning-bank#17](https://github.com/google-research/reasoning-bank/issues/17) | Add a leave-one-task-out guard for memory retrieval to prevent self-retrieval contamination | Open | — |
| [aiming-lab/Agent0#30](https://github.com/aiming-lab/Agent0/issues/30) | Add an admission manifest for generated-question data and answer-leakage canaries | Open | — |
| [modelscope/AgentEvolver#48](https://github.com/modelscope/AgentEvolver/issues/48) | Add a canary audit for ground-truth/reference-solution isolation | Open | — |
| [OpenDataBox/Workspace-Bench#8](https://github.com/OpenDataBox/Workspace-Bench/issues/8) | Add a canary leakage audit for rubric and ground-truth isolation | Closed | Discussion (3 comments) |
| [neosigmaai/auto-harness#25](https://github.com/neosigmaai/auto-harness/issues/25) | Add sealed negative-control tasks to audit train/test leakage and gate discipline | Open | — |
| [gepa-ai/gepa#381](https://github.com/gepa-ai/gepa/issues/381) | Add a run_dir admission manifest for candidate acceptance/rejection audits | Open | Implemented by an external contributor as PR #382 (open) |
| [facebookresearch/HyperAgents#35](https://github.com/facebookresearch/HyperAgents/issues/35) | Add machine-readable admission manifests for generated agents and rollback decisions | Open | Discussion (1 comment) |
| [Arvid-pku/Godel_Agent#4](https://github.com/Arvid-pku/Godel_Agent/issues/4) | Add a modification ledger before dumping accepted self-modified logic | Open | Discussion (1 comment) |
| [facebookresearch/matrix#138](https://github.com/facebookresearch/matrix/issues/138) | Add optional acceptance/provenance gates for synthetic multi-agent outputs | Open | — |
| [HolobiomicsLab/Mimosa-AI#154](https://github.com/HolobiomicsLab/Mimosa-AI/issues/154) | Add workflow admission cards before similarity-based template reuse | Open | Discussion (2 comments) |
| [iLearn-Lab/EvoHarness#1](https://github.com/iLearn-Lab/EvoHarness/issues/1) | Add negative-control probes to promotion validation | Open | Implemented by a repository collaborator (commit b8f323e) |
| [VectorInstitute/helix#86](https://github.com/VectorInstitute/helix/issues/86) | [FEATURE] Add transfer-evidence cards before promoting Helix improvements to main | Open | — |
| [hexo-ai/sia#38](https://github.com/hexo-ai/sia/issues/38) | Add transfer-evidence cards to separate reusable improvements from task-specific residue | Open | — |
| [JARVIS-Xs/SE-Agent#516](https://github.com/JARVIS-Xs/SE-Agent/issues/516) | Add counterfactual admission gates for trajectory-pool reuse | Open | — |
| [Human-Agent-Society/CORAL#124](https://github.com/Human-Agent-Society/CORAL/issues/124) | Add quarantine-backed promotion gates for cross-island memory and skill migration | Open | Discussion (4 comments) |
| [NousResearch/hermes-agent-self-evolution#118](https://github.com/NousResearch/hermes-agent-self-evolution/issues/118) | Phase 4 verifier/fitness harness: sealed hidden evals, rollback traces, and adaptive-vs-frozen gates | Open | — |
| [NousResearch/hermes-agent-self-evolution#115](https://github.com/NousResearch/hermes-agent-self-evolution/issues/115) | [Design Proposal] Runtime-gated reingestion with negative-control probes for skill evolution | Closed | — |
| [neosigmaai/auto-harness#24](https://github.com/neosigmaai/auto-harness/issues/24) | Terminal-Bench per_task_timeout config is ignored by benchmark and gate runners | Open | — |
| [kayba-ai/recursive-improve#2](https://github.com/kayba-ai/recursive-improve/issues/2) | evolve update writes scores to a nested island status file when run from a worktree | Open | — |
| [microsoft/stop#6](https://github.com/microsoft/stop/issues/6) | Add reproducible setup notes for current Python/OpenAI SDK environments | Open | — |
| [zou-group/sirius#2](https://github.com/zou-group/sirius/issues/2) | Make feedback and finetune-data paths respect subject/model args | Open | — |
| [VectorInstitute/aieng-bot#105](https://github.com/VectorInstitute/aieng-bot/issues/105) | Align agentic fix-loop staging instructions and result-comment step id | Closed | Discussion (2 comments) |
| [SakanaAI/AI-Scientist#249](https://github.com/SakanaAI/AI-Scientist/issues/249) | Withdrawn by the author shortly after filing | Withdrawn | — |

Follow-up code: [NousResearch/hermes-agent-self-evolution#202](https://github.com/NousResearch/hermes-agent-self-evolution/pull/202) adds lesson probes, a regression gate that checks each lesson by removing it (open on the check date).
