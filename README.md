# Wei-Lun Hsu

**Computer architecture / RTL verification / Reproducible hardware experiments**

I focus on how hardware behavior becomes observable evidence: from instruction-fetch signals to architectural traps, and from comparator waveforms to deadline-qualified decisions. My public projects connect a precise question to source-pinned experiments, explicit controls, raw observations, and replay instructions.

[Research evidence index](RESEARCH.md): inspect the implementation, follow the recorded results, or choose a replay entrypoint.

## Selected research artifacts

### [Ibex instruction-fetch error observability](https://github.com/WLHsu0827/ibex-fetch-error-observability)

**Question:** When does an instruction-bus error become an architectural instruction-fetch fault?

**Contribution:** A directed RTL fixture and runner bundle comparing bus and cache-to-IF observations against independent RVFI trap/retirement and exception-CSR checks. Three whole-core cases include a matched no-error control: the warm speculative error does not trap; the cold demanded miss does.

[Source and raw results](https://github.com/WLHsu0827/ibex-fetch-error-observability) · [Replay guide](https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/main/REPRODUCE.md) · [Verification record](https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/main/VERIFICATION.md)

This is an engineering reproduction of [known Ibex behavior](https://github.com/lowRISC/ibex/issues/1451), not a novel bug or paper. Two agent-executed recordings used isolated checkouts on the same Ubuntu 24.04 WSL host; byte-level replay differences are documented in the evidence index.

**Automated hosted replay:** A [successful GitHub Actions run](https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36889109686) passed fresh Ubuntu RTL replay (cache 3/3, whole-core 3/3), default-off lint, strict comparison, and package audit after tool installation. The upgrade is in [WLHsu0827/ibex-fetch-error-observability#1](https://github.com/WLHsu0827/ibex-fetch-error-observability/pull/1), **open and unmerged as checked 2026-10-02**. [Persistent proof and replay limits](RESEARCH.md#automated-hosted-replay-separate-from-the-wsl-record) distinguish this new automated host from the original WSL records and human validation.

### [Reset-armed trace phase monitor](https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/README.md)

**Question:** Can reset and process failures make a trace misleading?

**Contribution:** A standalone SystemVerilog observer, strict phase/order checker, and typed process runner that distinguish valid traces, intended fatal termination, and timeouts. Synthetic fixtures only, not CPU/ISA verification or qualified RVFI integration.

[Source and replay guide](https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/fdeedd7b0c0a7b0108d6b1fdbaecdc7056164b48/monitor/README.md) · [Successful hosted check](https://github.com/WLHsu0827/ibex-fetch-error-observability/actions/runs/36953848473) · [Permanent proof and limits](RESEARCH.md#standalone-trace-monitor-reset-and-process-outcomes)

**Status checked 2026-10-02:** [WLHsu0827/ibex-fetch-error-observability#2](https://github.com/WLHsu0827/ibex-fetch-error-observability/pull/2) is open and unmerged; the monitor is in the owner PR branch, not the default branch.

### [Comparator Atlas: When Calibration Is Not Enough](https://github.com/sscs-ose/sscs-ose-code-a-chip.github.io/pull/195)

**Question:** Does offset calibration produce a correct decision before the deadline?

**Contribution:** A SKY130 StrongARM comparator notebook comparing schematic design/calibration choices, plus a separate nominal schematic/archived extracted-RC SPICE study across 45 process-voltage-temperature conditions. Saved waveforms distinguish wrong decisions from unresolved ones; sampled specification maps expose timing/energy trade-offs.

[Notebook](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/wlhsu0827-comparator-atlas-isscc27/ISSCC27/submitted_notebooks/comparator_atlas/Comparator_Atlas.ipynb) · [Project overview](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/wlhsu0827-comparator-atlas-isscc27/ISSCC27/submitted_notebooks/comparator_atlas/README.md) · [Replay modes](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/wlhsu0827-comparator-atlas-isscc27/ISSCC27/submitted_notebooks/comparator_atlas/REPRODUCIBILITY.md)

**Status checked 2026-10-01:** Code-a-Chip submission PR open and unmerged, not an accepted publication or silicon result. Extracted-model physical fidelity remains unqualified; a fresh full-grid replay through the clean entrypoint has not been performed.

## How I present evidence

Pin sources and tools, separate controls from faulted cases, and distinguish recorded-data analysis from fresh execution. GitHub Copilot assisted implementation, experiment automation, and documentation; project licenses and third-party attribution are linked in the [evidence index](RESEARCH.md).

**Contact:** [GitHub profile](https://github.com/WLHsu0827).
