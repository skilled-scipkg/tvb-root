---
name: tvb-root-api-and-scripting
description: This skill should be used when users ask about api and scripting in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: API and Scripting

## High-Signal Playbook

### Route conditions
- Use this skill for Python entry points, command-profile automation, REST client/server usage, and script examples.
- Route to `tvb-root-simulation-workflows` for simulation configuration strategy and restart semantics.
- Route to `tvb-root-build-and-install` when setup/auth/port issues block API usage.
- Route to `tvb-root-inputs-and-modeling` for model-parameter and test-depth debugging.

### Triage questions
- Do you need local library scripting (`LIBRARY_PROFILE`), framework command scripting (`COMMAND_PROFILE`), or remote REST?
- Is persistence required (projects/operations/results), or is stateless local simulation enough?
- Is Keycloak configured and reachable for REST login?
- Are you launching simulations, analyzers, importers, or datatype export/import flows?
- Is the caller interactive notebook, one-off script, or CI automation?

### Canonical workflow
1. Pick interface tier:
   - `tvb.simulator.lab` for pure library scripting.
   - `tvb.interfaces.command.lab` for framework-managed projects/operations.
   - `TVBClient` for remote REST interactions.
2. For REST, launch TVB app + Keycloak config first, then start REST server.
3. Authenticate (`browser_login`) and resolve project/data identifiers.
4. Launch operation/simulation via view-model driven API.
5. Poll operation status until terminal state (`FINISHED`, `ERROR`, etc.).
6. Retrieve resulting datatypes and inspect/load H5 data as needed.

### Minimal working example
```bash
# Start local REST server (after Keycloak setup in TVB settings)
python -m tvb.interfaces.rest.server.run
```

```python
from tvb.interfaces.rest.client.tvb_client import TVBClient

tvb_client = TVBClient("http://localhost:9090")
tvb_client.browser_login()
projects = tvb_client.get_project_list()
print(projects)
```

### Pitfalls and fixes
- REST startup does nothing: verify `KEYCLOAK_CONFIG` path and existence (`tvb_framework/tvb/interfaces/rest/server/run.py` checks both).
- Browser login callback fails: callback port (default `8888`) already in use.
- Confusing profile behavior: `LIBRARY_PROFILE` does not persist managed project data; use framework/command/REST flows for DB-backed operations.
- Large datatype load crashes: avoid full in-memory load for expandable datatypes (client docs call out chunking concerns).
- No operation results: poll operation state first, then fetch results only after `FINISHED`.

### Convergence and validation checks
- `TVBClient.get_project_list()` returns user-visible projects.
- Operation status transitions through expected state machine (`PENDING/STARTED/FINISHED` or error states).
- Simulation launch returns a non-empty operation/simulation gid.
- Command demo scripts run end-to-end in initialized environments:
  - `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py`
  - `tvb_framework/tvb/interfaces/command/demos/operations/run_analyzer.py`

### Source-code fast path
- `tvb_framework/tvb/interfaces/command/lab.py` (command-profile helper API).
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py` (framework simulation script path).
- `tvb_framework/tvb/interfaces/rest/client/tvb_client.py` (public Python client surface).
- `tvb_framework/tvb/interfaces/rest/client/simulator/simulation_api.py` (serialization + upload flow).
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` (REST endpoint behavior).
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py` (server-side simulation launch pipeline).

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `tvb_framework/README_rest_client.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Shell.rst`
- `tvb_documentation/manuals/neotraits/index.rst`
- `tvb_documentation/manuals/neotraits/neocom.rst`
- `tvb_framework/tvb/interfaces/rest/server/README.md`
- `tvb_framework/tvb/interfaces/command/demos/importers/demo_array.txt`
- `tvb_library/README.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `tvb_framework/tvb/interfaces/command/demos`
- `tvb_documentation/demos`
- `tvb_documentation/tutorials`

## Test references
- `tvb_framework/tvb/interfaces/rest/client/tests/rest_test.py`
- `tvb_framework/tvb/tests/framework/interfaces/rest`
- `tvb_framework/tvb/tests/framework/interfaces/web`

## Source entry points for unresolved issues
- `tvb_framework/tvb/interfaces/command/lab.py`
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py`
- `tvb_framework/tvb/interfaces/command/demos/operations/run_analyzer.py`
- `tvb_framework/tvb/interfaces/rest/client/tvb_client.py`
- `tvb_framework/tvb/interfaces/rest/client/simulator/simulation_api.py`
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py`
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py`
- `tvb_framework/tvb/interfaces/rest/server/rest_api.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_framework/tvb/interfaces tvb_library/tvb tvb_storage/tvb tvb_contrib/tvb`).
