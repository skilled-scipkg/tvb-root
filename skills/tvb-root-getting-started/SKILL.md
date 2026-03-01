---
name: tvb-root-getting-started
description: This skill should be used when users ask about getting started in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Getting Started

## High-Signal Playbook

### Route conditions
- Use this skill for first-run setup, profile selection, default project orientation, and first reproducible workflow.
- Route to `tvb-root-build-and-install` when user is blocked on installation/configuration.
- Route to `tvb-root-simulation-workflows` for detailed simulator/run/restart procedures.
- Route to `tvb-root-api-and-scripting` for command/REST automation after first run is working.

### Triage questions
- Is the user starting from `TVB_Distribution`, PyPI, or monorepo source?
- GUI-first or script-first onboarding?
- Single-user local use or shared/server mode?
- Does user already have `tvb-data` and optional project archives (for example `ModelingEpilepsy.zip`)?
- Does user need only simulator basics, or also data exchange and project sharing?

### Canonical workflow
1. Launch TVB (GUI or scripting profile) and complete initial settings.
2. Confirm admin account and storage location are sane for the machine.
3. Open/select a project (`Default Project` if available, otherwise import one).
4. Import a connectivity datatype (for example `connectivity_96.zip`) into current project.
5. Run one short baseline simulation and inspect time-series output in visualizers.
6. Export one datatype (H5) and inspect basic structure (`data`, `time`, metadata attrs).
7. Optionally test a command-profile script for project import/export or operation launch.

### Minimal working example
```bash
# Distribution shell entry points (Linux naming)
cd TVB_Distribution/bin
./distribution.sh start COMMAND_PROFILE
# alternative: ./distribution.sh start LIBRARY_PROFILE
```

```python
# Example from tutorial_0 style post-export inspection
import h5py
f = h5py.File("LinkAndShare_TimeSeriesRegion.h5", "r")
print(list(f.keys()))
print(list(f.attrs.keys())[:5])
```

### Pitfalls and fixes
- First-run config write fails: set `TVB_USER_HOME` to writable location.
- Default admin credentials left unchanged: update admin email/password for shared environments.
- Data removed unexpectedly: `tvb_clean` (or `tvb_bin/clean.sh`) deletes all user TVB data; use only for reset scenarios.
- Missing demo datasets: install/use `tvb-data` bundle or import project ZIP as documented.
- GUI starts but cannot be reached remotely: verify server/client setup and configured host/port.

### Convergence and validation checks
- GUI accessible and project list visible.
- One project can be created/imported/exported without errors.
- Connectivity import appears in Data Structure and is selectable in Simulator.
- One short simulation produces a `TimeSeries` result with readable H5 keys.
- Command profile can import/export a project using demo scripts when required.

### Source-code fast path
- `tvb_framework/tvb/interfaces/web/run.py` (web profile bootstrap).
- `tvb_framework/tvb/interfaces/command/lab.py` (command profile helpers).
- `tvb_framework/tvb/interfaces/command/demos/project/project_import.py` (project import path).
- `tvb_framework/tvb/interfaces/command/demos/project/project_export.py` (project export path).
- `tvb_contrib/tvb/contrib/rateML/XML2model.py` (early advanced contrib entry point).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `tvb_documentation/tutorials/tutorial_0_GettingStarted.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Overview.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Installation.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Shell.rst`
- `tvb_documentation/manuals/neotraits/neotraits.rst`
- `tvb_documentation/manuals/DeveloperReference/DeveloperReferenceManual.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `tvb_documentation/tutorials`
- `tvb_documentation/demos`
- `tvb_framework/tvb/interfaces/command/demos`
- `tvb_contrib/tvb/contrib/rateML/README.md`

## Source entry points for unresolved issues
- `tvb_framework/tvb/interfaces/web/run.py`
- `tvb_framework/tvb/interfaces/command/lab.py`
- `tvb_framework/tvb/interfaces/command/demos/project/project_import.py`
- `tvb_framework/tvb/interfaces/command/demos/project/project_export.py`
- `tvb_contrib/tvb/contrib/rateML/XML2model.py`
- `tvb_contrib/tvb/contrib/rateML/run/model_driver.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_framework/tvb/interfaces tvb_documentation/tutorials tvb_contrib/tvb/contrib/rateML`).
