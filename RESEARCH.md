# Research evidence index

[Profile overview](README.md)

Public evidence checked **2026-10-01**. Links below pin the inspected source snapshots; the profile links to the projects' current entrypoints. These are experiment artifacts, not claims of an accepted paper.

## Ibex: observe the architectural consequence, not just the bus error

**Question and method:** Inject one selected instruction-response error while holding data and latency fixed. Compare bus and accepted cache-to-IF errors with RVFI trap/retirement, handler activity, and exception CSRs. The warm no-error case is a matched negative control; the cold demanded miss exercises the trap path.

The original contribution is the directed fixture, program, runners, and inspectable replay bundle for the mechanism already discussed in [lowRISC/ibex#1451](https://github.com/lowRISC/ibex/issues/1451), not a new Ibex defect or fix. The experiment pins [upstream Ibex](https://github.com/lowRISC/ibex/commit/7cd891ef267e8db36813b29cb8851142ab2636d5).

Snapshot: [`b6047fd`](https://github.com/WLHsu0827/ibex-fetch-error-observability/commit/b6047fd245cd8541b2cb006d65c988df17a51ee3).

| Inspect | Direct evidence |
| --- | --- |
| Implementation and source identity | [Original experiment files][ibex-fixture], [source manifest][ibex-manifest], [opt-in Simple System patch][ibex-patch] |
| Recorded observations | [Standalone cache JSON][ibex-cache], [whole-core RVFI/CSR JSON][ibex-core] |
| Replay comparison | [Cache replay JSON][ibex-cache-replay], [whole-core replay JSON][ibex-core-replay], [verification record][ibex-verification] |
| Run it and retain attribution | [Build/replay commands][ibex-reproduce], [comparison script][ibex-compare], [Apache-2.0 license][ibex-license], [notice][ibex-notice] |

**What the record establishes:** Three directed whole-core cases, one run per case in each of two agent-executed recordings on the **same Ubuntu 24.04 WSL host**. Warm speculative bus error: no accepted IF error or trap; cold demanded miss: accepted IF error and architectural trap.

Cache replay JSON is byte-identical. Whole-core compiled binary hashes and JSON bytes differ for an **unestablished reason**; all other parsed fields, including cycle-tagged events, match. This is neither cross-host nor independent human replication, and it establishes no general error rate. Fresh tool installation and CI execution are not verified by this record.

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
