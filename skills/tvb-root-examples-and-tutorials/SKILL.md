---
name: tvb-root-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Examples and Tutorials

## High-Signal Playbook

### Route conditions
- Use this skill for runnable demos/tutorials, script-first walkthroughs, and reproducible example outputs.
- Route to `tvb-root-getting-started` for first install/profile onboarding.
- Route to `tvb-root-simulation-workflows` for deep simulator execution tuning.
- Route to `tvb-root-inputs-and-modeling` for model-parameter validity and scientific interpretation.

### Triage questions
- GUI tutorial walkthrough, command demo script, or notebook-driven workflow?
- Do you need a minimal deterministic run, or richer analyzers/visualizers?
- Which output needs validation (`TimeSeries`, exported H5, metadata updates)?
- Are you validating tutorial behavior or adapting it into new code?

### Canonical workflow
1. Choose one runnable path (command demo or tutorial script) and keep first run short.
2. Confirm input requirements (project, connectivity, profile) before launch.
3. Run example script with no code changes and capture baseline outputs.
4. Inspect generated datatype/H5 structure and operation status.
5. Only then adapt parameters or replace model/monitor selections.

### Minimal working example
```bash
# Framework-managed simulation demo
python tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py

# Datatype read/export demo
python tvb_framework/tvb/interfaces/command/demos/datatypes/read_from_h5.py
python tvb_framework/tvb/interfaces/command/demos/datatypes/search_and_export.py
```

```bash
# Tutorial analysis helper script
python tvb_documentation/tutorials/mse_analyse_region.py
```

### Pitfalls and fixes
- Demo script fails on missing project/context: initialize framework profile and confirm project selection.
- Exported H5 looks incomplete: verify operation finished before running export/read helpers.
- Tutorial script runtime mismatch: keep tutorial defaults before adding custom monitors/models.
- Adapted script diverges from docs: compare against the original demo function entry points first.

### Convergence and validation checks
- Demo command exits cleanly and yields expected operation/datatype output.
- Exported/read H5 files contain expected groups/attributes.
- Modified script keeps baseline behavior before parameter expansion.
- One related framework or library test still passes after edits.

### Source-code fast path
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py` (`run_simulation`).
- `tvb_framework/tvb/interfaces/command/demos/operations/run_analyzer.py` (`run_analyzer`).
- `tvb_framework/tvb/interfaces/command/demos/datatypes/read_from_h5.py` (`read_h5`).
- `tvb_framework/tvb/interfaces/command/demos/datatypes/search_and_export.py` (`search_and_export_ts`).
- `tvb_framework/tvb/interfaces/command/demos/datatypes/modify_h5_metadata.py` (`update_local_connectivity_metadata`).
- `tvb_documentation/tutorials/mse_analyse_region.py` (`configure_simulation`, `run_simulation`, `main`).
- `tvb_documentation/tutorials/utils.py` (`run_sim_with_progress_bar`).

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Keep responses concrete and runnable: command, inputs, and expected validation checkpoints.

## Primary documentation references
- `tvb_documentation/tutorials/Tutorials.rst`
- `tvb_documentation/tutorials/tutorial_0_GettingStarted.rst`
- `tvb_documentation/tutorials/tutorial_1_BuildingYourOwnBrainNetworkModel.rst`
- `tvb_documentation/tutorials/tutorial_3_ModelingEpilepsy.rst`
- `tvb_documentation/manuals/neotraits/h5.rst`
- `dev_resources/docs/HowToStaticHelp.txt`
- `dev_resources/docs/HowToProfileTVB.txt`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for additional topic docs.
- If behavior details are still ambiguous, inspect `references/source_map.md` and use the function-level anchors.
- Use demos/tests as executable contracts for expected behavior.
- Cite exact file paths in responses.

## Tutorials and examples
- `tvb_documentation/tutorials`
- `tvb_documentation/demos`
- `tvb_framework/tvb/interfaces/command/demos`
- `tvb_contrib/tvb/contrib/simulator/demos`

## Test references
- `tvb_library/tvb/tests/library/simulator/simulator_test.py`
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py`
- `tvb_framework/tvb/interfaces/rest/client/tests/rest_test.py`

## Source entry points for unresolved issues
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py`
- `tvb_framework/tvb/interfaces/command/demos/operations/run_analyzer.py`
- `tvb_framework/tvb/interfaces/command/demos/datatypes/read_from_h5.py`
- `tvb_framework/tvb/interfaces/command/demos/datatypes/search_and_export.py`
- `tvb_framework/tvb/interfaces/command/demos/datatypes/modify_h5_metadata.py`
- `tvb_documentation/tutorials/mse_analyse_region.py`
- `tvb_documentation/tutorials/utils.py`
- `tvb_contrib/tvb/contrib/simulator/demos/region_deterministic_contributed_model.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_framework/tvb/interfaces/command/demos tvb_documentation/tutorials tvb_contrib/tvb/contrib/simulator/demos`).
