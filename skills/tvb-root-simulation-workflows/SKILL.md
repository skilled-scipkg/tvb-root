---
name: tvb-root-simulation-workflows
description: This skill should be used when users ask about simulation workflows in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Simulation Workflows

## High-Signal Playbook

### Route conditions
- Use this skill for launching/running/restarting simulations and interpreting simulation outputs.
- Route to `tvb-root-inputs-and-modeling` for model/coupling/integrator parameter design.
- Route to `tvb-root-api-and-scripting` for REST/client command automation details.
- Route to `tvb-root-build-and-install` if runtime issues are actually setup/profile/auth failures.

### Triage questions
- Which execution path: GUI wizard, command demos, REST, or pure library script?
- Region-level or surface-level simulation?
- Which model, coupling, integrator (`dt`), and monitor set are being used?
- Is run continuation/branching required, or single-shot run is enough?
- Do outputs need TVB-managed persistence (project/operation history) or in-memory arrays only?
- Are runtime failures numerical (unstable integration), resource related (RAM/disk), or configuration mismatches?

### Canonical workflow
1. Start from Simulator UI workflow (`Connectivity`, `Coupling`, `Model`, `Integrator`, `Monitors`, length).
2. Keep the first run short and stable (tutorial defaults: `dt=0.1 ms`, temporal average monitor period `1 ms`).
3. Run once and inspect operation status/history (`green` finished, `red` error).
4. Validate output with time-series visualizers and analyzer quick checks.
5. For parameter exploration, use PSE and keep monitored dimensions explicit.
6. For continuation/branching, preserve core structural invariants (nodes, conduction speed, monitor/time-step setup).
7. For scripting paths, mirror the same simulator model via command demo or REST launch.

### Minimal working example
```python
from tvb.simulator.lab import *
import numpy

conn = connectivity.Connectivity.from_file()
conn.speed = numpy.array([4.0])

sim = simulator.Simulator(
    model=models.Generic2dOscillator(),
    connectivity=conn,
    coupling=coupling.Linear(a=numpy.array([0.0042])),
    integrator=integrators.HeunDeterministic(dt=0.1),
    monitors=(monitors.TemporalAverage(period=1.0),),
    simulation_length=1000.0,
).configure()

(time, data), = sim.run()
print(time.shape, data.shape)
```

```bash
# Framework-managed command demo (expects initialized TVB DB/project)
python tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py
```

### Pitfalls and fixes
- Numerical instability or exploding traces: reduce integrator `dt` and re-check model/coupling scales.
- Red simulation status in UI: inspect operation details/logs before changing model parameters.
- Branching surprises: keep spatiotemporal structure unchanged when using previous history as initial state.
- Missing persisted outputs: `LIBRARY_PROFILE` runs are not framework-managed; use command/GUI/REST when persistence matters.
- Surface simulation storage blow-up: docs note high disk pressure for short simulated intervals; cap run lengths during triage.

### Convergence and validation checks
- Operation reaches terminal success state and emits expected `TimeSeries*` datatype.
- Time axis and monitor output dimensions match configured sampling period and run length.
- Branch run reuses compatible configuration and produces continuous dynamics behavior.
- Re-running with same deterministic settings reproduces equivalent qualitative output.

### Source-code fast path
- `tvb_library/tvb/simulator/simulator.py` (configure/run semantics and component assembly).
- `tvb_library/tvb/simulator/integrators.py` (deterministic/stochastic step behavior).
- `tvb_framework/tvb/core/entities/file/simulator/view_model.py` (GUI/API simulation model schema).
- `tvb_framework/tvb/core/entities/file/simulator/simulation_history_h5.py` (restart/history persistence model).
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py` (framework launch example).
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` (REST fire-simulation endpoint).

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `tvb_documentation/manuals/UserGuide/UserGuide-UI_Simulator.rst`
- `tvb_documentation/tutorials/tutorial_1_BuildingYourOwnBrainNetworkModel.rst`
- `tvb_documentation/demos/Demos.rst`
- `tvb_documentation/sim_doc/tvb.simulator.integrators.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-UI.rst`
- `tvb_documentation/dsl/tvb_dsl.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `tvb_documentation/demos`
- `tvb_documentation/tutorials`
- `tvb_framework/tvb/interfaces/command/demos/operations`
- `tvb_contrib/tvb/contrib/simulator/demos`

## Test references
- `tvb_library/tvb/tests/library/simulator`
- `tvb_framework/tvb/tests/framework/adapters/simulator`
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py`

## Source entry points for unresolved issues
- `tvb_library/tvb/simulator/simulator.py`
- `tvb_library/tvb/simulator/integrators.py`
- `tvb_library/tvb/simulator/_numba/integrators.py`
- `tvb_framework/tvb/core/entities/file/simulator/view_model.py`
- `tvb_framework/tvb/core/entities/file/simulator/simulation_history_h5.py`
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py`
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py`
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_library/tvb/simulator tvb_framework/tvb/interfaces tvb_framework/tvb/core/entities/file/simulator`).
