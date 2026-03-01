---
name: tvb-root-tvb-documentation
description: This skill should be used when users ask about tvb documentation in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Tvb Documentation

## High-Signal Playbook

### Route conditions
- Use this skill when the user asks "where is it documented?", needs docs routing, or needs a citation-ready file path.
- Route to subsystem skills once intent is clear (`build-and-install`, `simulation-workflows`, `api-and-scripting`, `inputs-and-modeling`).
- Stay docs-first; only escalate to source when docs conflict, are stale, or omit implementation detail.

### Triage questions
- Is the question about installation, GUI workflow, shell/API usage, modeling, or data exchange?
- Is a user-facing manual page enough, or is module API detail required?
- Is the request version-sensitive (benchmark pages, release-era docs)?
- Does the answer need exact commands, conceptual overview, or troubleshooting steps?

### Canonical workflow
1. Start at `tvb_documentation/index.rst` to anchor section hierarchy.
2. Jump to the smallest authoritative doc page for the intent (installation/config/simulator/shell/data exchange).
3. Extract the minimum runnable guidance and cite exact local doc path.
4. If ambiguity remains, consult `references/doc_map.md` to avoid missing sibling docs.
5. Escalate to `references/source_map.md` entry points and inspect concrete implementation files.
6. Return both doc path and source path when behavior details were source-confirmed.

### Minimal working example
```bash
# Docs-first lookup for simulator workflow and constraints
rg -n "Simulation|Stochastic|Branch|PSE|status" tvb_documentation/manuals/UserGuide tvb_documentation/tutorials

# Source escalation only after docs
rg -n "class Simulator|def configure|fire_simulation|SimulationHistory" tvb_library/tvb/simulator tvb_framework/tvb
```

### Pitfalls and fixes
- Sphinx index assumptions: `tvb_documentation/index.rst` requires a real (hidden) toctree for valid site structure.
- Data exchange interpretation errors: docs explicitly warn that TVB import/export does not apply space transforms.
- Connectivity data surprises: UI docs note NaN/Inf and negative values must be cleaned/handled before use.
- Duplicate guidance across README/manual/tutorial files: cite one canonical path, then add cross-links only if needed.
- Visualizer misreads: some visualizers dynamically scale amplitudes; signal amplitudes may not be directly comparable.

### Convergence and validation checks
- Response includes at least one exact docs file path from `tvb_documentation`.
- If source was used, response includes at least one concrete implementation file path.
- Routing decisions are explicit (which subsystem skill should handle follow-up).
- Commands/examples in response map directly to documented workflow steps.

### Source-code fast path
- `tvb_framework/tvb/interfaces/web/controllers/simulator/monitors_wizard_handler.py`
- `tvb_framework/tvb/adapters/visualizers/connectivity.py`
- `tvb_library/tvb/simulator/simulator.py`
- `tvb_library/tvb/simulator/noise.py`
- `tvb_library/tvb/simulator/coupling.py`
- `tvb_library/tvb/simulator/monitors.py`

## Scope
- Handle questions about documentation grouped under the 'tvb-documentation' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `tvb_documentation/index.rst`
- `tvb_documentation/README.md`
- `tvb_documentation/manuals/UserGuide/UserGuide-Installation.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Config.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-UI.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-UI_Simulator.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Shell.rst`
- `tvb_documentation/manuals/UserGuide/DataExchange.rst`
- `tvb_documentation/demos/Demos.rst`
- `tvb_documentation/tutorials`

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
- `tvb_framework/tvb/interfaces/command/demos`

## Test references
- `tvb_framework/tvb/tests`
- `tvb_library/tvb/tests`
- `tvb_storage/tvb/tests`
- `tvb_contrib/tvb/contrib/tests`

## Source entry points for unresolved issues
- `tvb_framework/tvb/interfaces/web/controllers/simulator/monitors_wizard_handler.py`
- `tvb_framework/tvb/adapters/visualizers/connectivity.py`
- `tvb_framework/tvb/adapters/visualizers/connectivity_edge_bundle.py`
- `tvb_framework/tvb/adapters/visualizers/local_connectivity_view.py`
- `tvb_library/tvb/simulator/simulator.py`
- `tvb_library/tvb/simulator/noise.py`
- `tvb_library/tvb/simulator/monitors.py`
- `tvb_library/tvb/simulator/coupling.py`
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_documentation tvb_framework/tvb tvb_library/tvb`).
