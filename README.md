# Wei-Lun Hsu

I focus on reproducible RTL verification and circuit simulation. I aim to make source versions, controls, evidence, and limitations as easy to inspect as the results.

## Selected public work

### [Ibex instruction-fetch error observability](https://github.com/WLHsu0827/ibex-fetch-error-observability)

An Apache-2.0 directed RTL pilot pinned to [an Ibex commit](https://github.com/lowRISC/ibex/commit/7cd891ef267e8db36813b29cb8851142ab2636d5). Three full-core cases (one run per case in each recording) use RVFI trap/retirement and CSR checks as architectural truth: a warm speculative bus error does not trap, while a cold demanded miss does. [Reproduction guide](https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/main/REPRODUCE.md) · [Verification record](https://github.com/WLHsu0827/ibex-fetch-error-observability/blob/main/VERIFICATION.md).

This examines a [previously discussed mechanism](https://github.com/lowRISC/ibex/issues/1451), not a new bug. The recorded executions used two isolated checkouts on the same WSL host, not separate-host or human replication; compared events match, but whole-core binary hashes and JSON bytes differ.

### [Comparator Atlas](https://github.com/sscs-ose/sscs-ose-code-a-chip.github.io/pull/195) (open submission PR)

A SKY130 comparator notebook with nominal schematic and extracted-RC SPICE simulations across a documented 45-condition PVT grid. [Notebook](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/wlhsu0827-comparator-atlas-isscc27/ISSCC27/submitted_notebooks/comparator_atlas/Comparator_Atlas.ipynb) · [Entry README](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/wlhsu0827-comparator-atlas-isscc27/ISSCC27/submitted_notebooks/comparator_atlas/README.md) · [Reproduction instructions](https://github.com/WLHsu0827/sscs-ose-code-a-chip.github.io/blob/wlhsu0827-comparator-atlas-isscc27/ISSCC27/submitted_notebooks/comparator_atlas/REPRODUCIBILITY.md).

The PR is open and unmerged, not an accepted publication or a silicon result. Extracted-model physical fidelity remains unqualified, and a fresh full-grid replay through the clean reproduction entrypoint has not been performed.

My working principle: pin sources and tools, document controls, share inspectable evidence, and state what the results do not establish.

**Contact:** [GitHub profile](https://github.com/WLHsu0827).
