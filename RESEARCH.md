# Research evidence index

[Profile overview](README.md)

Original public evidence checked **2026-10-01**; hosted Ibex upgrade and standalone monitor checked **2026-10-02**. Links below pin the inspected source snapshots; the profile links to the projects' current entrypoints. These are experiment artifacts, not claims of an accepted paper.

## Ibex: observe the architectural consequence, not just the bus error

**Question and method:** Inject one selected instruction-response error while holding data and latency fixed. Compare bus and accepted cache-to-IF errors with RVFI trap/retirement, handler activity, and exception CSRs. The warm no-error case is a matched negative control; the cold demanded miss exercises the trap path.

The original contribution is the directed fixture, program, runners, and inspectable replay bundle for the mechanism already discussed in [lowRISC/ibex#1451](https://github.com/lowRISC/ibex/issues/1451), not a new Ibex defect or fix. The experiment pins [upstream Ibex](https://github.com/lowRISC/ibex/commit/7cd891ef267e8db36813b29cb8851142ab2636d5).

### Original same-host WSL record

Snapshot: [`b6047fd`](https://github.com/WLHsu0827/ibex-fetch-error-observability/commit/b6047fd245cd8541b2cb006d65c988df17a51ee3).

| Inspect | Direct evidence |
| --- | --- |
| Implementation and source identity | [Original experiment files][ibex-fixture], [source manifest][ibex-manifest], [opt-in Simple System patch][ibex-patch] |
| Recorded observations | [Standalone cache JSON][ibex-cache], [whole-core RVFI/CSR JSON][ibex-core] |
| Replay comparison | [Cache replay JSON][ibex-cache-replay], [whole-core replay JSON][ibex-core-replay], [verification record][ibex-verification] |
| Run it and retain attribution | [Build/replay commands][ibex-reproduce], [comparison script][ibex-compare], [Apache-2.0 license][ibex-license], [notice][ibex-notice] |

**What the record establishes:** Three directed whole-core cases, one run per case in each of two agent-executed recordings on the **same Ubuntu 24.04 WSL host**. Warm speculative bus error: no accepted IF error or trap; cold demanded miss: accepted IF error and architectural trap.

Cache replay JSON is byte-identical. Whole-core compiled binary hashes and JSON bytes differ for an **unestablished reason**; all other parsed fields, including cycle-tagged events, match. This is neither cross-host nor independent human replication, and it establishes no general error rate. Fresh tool installation and CI execution are not verified by this record.

### Automated hosted replay (separate from the WSL record)

**Status checked 2026-10-02:** [WLHsu0827/ibex-fetch-error-observability#1][ibex-hosted-pr] is open and unmerged. [Run 36889109686][ibex-hosted-latest] completed successfully at PR head [`4500569`][ibex-hosted-snapshot]; both the package/offline and fresh Ubuntu RTL jobs passed. The hosted workflow and archive are in this pending upgrade, not the repository's `main` branch.

**Recorded method and outcome:** A new GitHub-hosted Ubuntu 24.04.5 environment installed Verilator 5.020-1 and libelf-dev, with Python 3.12.3, FuseSoC 2.4.3, Edalize 0.6.8, packaging 24.2, and g++ 13.3.0; no compiled cache was restored. Fresh cache and whole-core suites each passed **3/3**, followed by default-off lint, strict complete-event/source-hash/classification comparison, and package audit.

| Inspect | Direct evidence |
| --- | --- |
| Current upgrade and completed jobs | [Open owner-repo PR][ibex-hosted-pr], [successful run at inspected head][ibex-hosted-latest] |
| Persistent successful replay | [Archive guide][ibex-hosted-guide], [success manifest][ibex-hosted-success], [retained outputs and log tails][ibex-hosted-success-raw] from [run 36887152817][ibex-hosted-success-run] |
| Preserved initial failure | [Failure manifest][ibex-hosted-failure] from [run 36885980667][ibex-hosted-failure-run] |

**Input identity:** The archived success executed bundle `cfeeb13460b1b4efdf924666d12df79153616638`; its manifest separately records GitHub event/merge commit `14e8195321b8fc44fc09b77e9b332f8a335fc78a`. The archive snapshot `4500569` is **not that archived run's input**. Original source and observation/replay bytes remain unchanged. Manifests are derived provenance/checksums, not simulator output; the archive retains public sanitized payloads and log tails, not the full console, ZIP, or compiler output.

**Failure and scope:** The initial attempt failed an upstream whole-core pre-build check because Python `packaging` was missing. Whole-core replay, lint, comparison, and final audit were **not run** then; installing the missing pinned dependency fixed it without bypassing the check. Successful hosted cache JSON remains byte-identical to the original. Core binary/JSON bytes differ, while every other parsed field and exact event agrees; the cause and general binary reproducibility remain unknown.

This closes the fresh automated host/tool-installation gap for the **same three directed cases per suite**, not added statistical trials. It is not independent human/end-user validation, research-independent replication, or an empty-OS installation test. The original WSL evidence above remains a distinct record.

## Standalone trace monitor: reset and process outcomes

**Question and engineering contribution:** Can logging before reset or accepting any fatal-looking output turn an invalid trace into a pass? A reset-armed SystemVerilog observer suppresses rows before/during reset and rejects reset after measurement starts. Its checker enforces consecutive cycles, row widths, event counts, and controlled phase/order associations; its process runner distinguishes exits, signals, timeouts, and tool/spawn failures.

**Status checked 2026-10-02:** [WLHsu0827/ibex-fetch-error-observability#2][monitor-pr] is open and unmerged. [Final-head CI][monitor-ci] completed successfully on Ubuntu 24.04 with Verilator 5.020-1, including offline contracts and real standalone module/fixture execution at [`fdeedd7`][monitor-snapshot]. This separate owner-branch artifact is not in `main` and is not part of the known [lowRISC/ibex#1451](https://github.com/lowRISC/ibex/issues/1451) whole-core/cache experiment above.

| Inspect | Direct evidence |
| --- | --- |
| Instrument, checker, and process contract | [Observer module][monitor-observer], [trace checker][monitor-checker], [typed runner][monitor-runner], [source manifest][monitor-sources] |
| Replay entrypoint | [Module guide, licensing, and attribution][monitor-guide], [completed final-head CI][monitor-ci] |
| Corrected permanent proof | [Raw-byte manifest][monitor-proof-manifest], [case summary][monitor-proof-summary], [retained outputs][monitor-proof], [archived input run][monitor-proof-run] |
| Preserved earlier limitation | [First archive: timeout-only live hang evidence][monitor-first-proof] |

From a checkout of the pinned owner-PR source, at the repository root, run **offline Python 3.12 standard-library contracts** without RTL compilation:

```sh
python -B -m monitor.run_all --mode offline
```

Or run **real Linux module/fixture replay**, requiring Verilator 5.020 and a C++ compiler:

```sh
python -B -m monitor.run_all --mode real --output monitor-output
```

**Archive identity and observed outcomes:** Corrected run `36953631262` executed source commit `273ac718b6d401e32650a4bf08dcf429537a6b72`, distinct from archive publication `fdeedd7`. The later final-head CI is a separate execution, not a replacement for those archived outputs. Four positive reset shapes (initial-low, delayed, held, and asynchronous) each produce `Q [0, 1]` and one `B`/`PRE`/`POST`/`R` with checked phase/order associations. No-reset empty trace and the fresh unarmed `Q [0, 1, 0, 1]` counterexample are rejected.

Repeated reset terminates as `signaled:6` with the intended diagnostic. The corrected live hang captures exactly `TRACE_MONITOR_RESET_AFTER_START\n` but remains `timeout:124`, with `reset_diagnostic_captured: true` and `expected_fatal: false`. The first archive (`36952633401`) retains empty live hang stdout: timeout rejection was demonstrated there, **not** marker capture. That record is preserved, not reconstructed.

**Scope:** Instruction bits and control signals are opaque synthetic labels, not ISA execution, CPU coverage, an architectural oracle, qualified RVFI integration, or a full Ibex bind. This is a bounded engineering artifact, not a confirmed Ibex bug, general error rate, new research method/paper, or independent human replication. No binaries, waveforms, or private CPU data are published; the guide retains Apache-2.0 licensing and Copilot assistance attribution.

## Comparator Atlas: separate correctness from meeting the deadline

**Question and method:** Compare schematic comparator designs under the same local calibration policy, then evaluate a separate nominal, code-zero schematic/archived RC study across 45 PVT conditions. Decisions use complementary output thresholds and input polarity; a wrong decision and an unresolved deadline are different outcomes. Sampled specification maps select by mean core energy only after the included inputs meet the deadline.

Snapshot: [`798f498`](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/commit/798f49838947a7d65e343d4325b72d1cd303b88a).

| Inspect | Direct evidence |
| --- | --- |
| Question, circuit, and analysis | [Notebook][atlas-notebook], [reviewer tour][atlas-tour] |
| Measurements and design choices | [Schematic measurement table][atlas-measurements], [sampled specification map JSON][atlas-spec], [nominal PVT records][atlas-pvt] |
| Analyze saved data or run fresh simulations | [Execution modes and limits][atlas-reproduce], [clean-start smoke/full guide][atlas-clean] |
| Source identity and attribution | [Entry checksums][atlas-checksums], [MIT license][atlas-license], [third-party notices][atlas-notices] |

**Replay boundary:** Default notebook execution analyzes supplied data and remeasures saved waveforms; it is not a new SPICE campaign. The documented clean-start smoke test covers four transients at one TT point. A fresh full-grid rerun through that entrypoint has not been performed. The archived extracted-RC model's physical fidelity remains unqualified; numerical agreement and structural checks do not establish silicon performance or statistical yield.

**Publication boundary:** [sscs-ose/sscs-ose-code-a-chip.github.io#195](https://github.com/sscs-ose/sscs-ose-code-a-chip.github.io/pull/195) was open and unmerged when checked. It is a submission, not an accepted publication. The project documents credit Wei-Lun Hsu and disclose GitHub Copilot assistance; no personal hands-on replication is asserted here.

[ibex-fixture]: https://github.com/WLHsu0827/ibex-fetch-error-observability/tree/b6047fd245cd8541b2cb006d65c988df17a51ee3/experiment
[ibex-manifest]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/SOURCE_MANIFEST.json
[ibex-patch]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/patches/simple-system-fetch-fault.patch
[ibex-cache]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/observations/results.json
[ibex-core]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/observations/core_results.json
[ibex-cache-replay]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/verification/replayed_cache.json
[ibex-core-replay]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/verification/replayed_core.json
[ibex-verification]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/VERIFICATION.md
[ibex-reproduce]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/REPRODUCE.md
[ibex-compare]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/scripts/compare.py
[ibex-license]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/LICENSE
[ibex-notice]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/b6047fd245cd8541b2cb006d65c988df17a51ee3/NOTICE
[ibex-hosted-pr]: https://github.com/WLHsu0827/ibex-fetch-error-observability/pull/1
[ibex-hosted-latest]: https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36889109686
[ibex-hosted-snapshot]: https://github.com/WLHsu0827/ibex-fetch-error-observability/commit/45005691491e298cdf087d07ba8806f648b9c047
[ibex-hosted-guide]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/45005691491e298cdf087d07ba8806f648b9c047/verification/hosted/README.md
[ibex-hosted-success]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/45005691491e298cdf087d07ba8806f648b9c047/verification/hosted/run-36887152817/manifest.json
[ibex-hosted-success-raw]: https://github.com/WLHsu0827/ibex-fetch-error-observability/tree/45005691491e298cdf087d07ba8806f648b9c047/verification/hosted/run-36887152817/raw
[ibex-hosted-success-run]: https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36887152817
[ibex-hosted-failure]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/45005691491e298cdf087d07ba8806f648b9c047/verification/hosted/run-36885980667/manifest.json
[ibex-hosted-failure-run]: https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36885980667
[monitor-pr]: https://github.com/WLHsu0827/ibex-fetch-error-observability/pull/2
[monitor-snapshot]: https://github.com/WLHsu0827/ibex-fetch-error-observability/commit/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48
[monitor-observer]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/trace_phase_observer.sv
[monitor-checker]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/trace_check.py
[monitor-runner]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/process_runner.py
[monitor-sources]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/SOURCE_MANIFEST.json
[monitor-guide]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/README.md
[monitor-ci]: https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36953848473
[monitor-proof-manifest]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/evidence/run-36953631262/RAW_MANIFEST.json
[monitor-proof-summary]: https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/evidence/run-36953631262/summary.json
[monitor-proof]: https://github.com/WLHsu0827/ibex-fetch-error-observability/tree/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/evidence/run-36953631262
[monitor-proof-run]: https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36953631262
[monitor-first-proof]: https://github.com/WLHsu0827/ibex-fetch-error-observability/tree/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/evidence/run-36952633401
[atlas-notebook]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/Comparator_Atlas.ipynb
[atlas-tour]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/REVIEWER_GUIDE.md
[atlas-measurements]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/results/study/verified_measurements.csv
[atlas-spec]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/results/study/specification_map/summary.json
[atlas-pvt]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/tree/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/results/study/postlayout_pvt45
[atlas-reproduce]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/REPRODUCIBILITY.md
[atlas-clean]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/reproduction/pvt45/pvt45_reproduce/README.md
[atlas-checksums]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/entry_checksums.json
[atlas-license]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/LICENSE
[atlas-notices]: https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/798f49838947a7d65e343d4325b72d1cd303b88a/ISSCC27/submitted_notebooks/comparator_atlas/THIRD_PARTY_NOTICES.txt
