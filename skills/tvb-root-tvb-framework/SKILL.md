---
name: tvb-root-tvb-framework
description: This skill should be used when users ask about tvb framework in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Tvb Framework

## High-Signal Playbook

### Route conditions
- Use this skill for framework adapter behavior, importer/visualizer wiring, project/operation services, and framework profile semantics.
- Route to `tvb-root-api-and-scripting` for REST client usage and automation-specific API flows.
- Route to `tvb-root-simulation-workflows` for simulator orchestration semantics.
- Route to `tvb-root-tvb-storage` when failures are primarily H5/encryption/storage-layer issues.

### Triage questions
- Is this issue in adapter importers/visualizers, services, or REST resources?
- Is the failure schema-related (view-model/form/index mismatch) or runtime launch/state related?
- Which framework profile is active (`WEB_PROFILE`, `COMMAND_PROFILE`, test profiles)?
- Which focused framework test reproduces the behavior now?

### Canonical workflow
1. Identify layer first: adapter, service, REST resource, or persistence model.
2. Reproduce with one focused framework test under explicit profile.
3. Trace call path from view-model/form to service and DAO boundaries.
4. Apply minimal change at the owning layer.
5. Re-run focused test plus one neighboring integration test.

### Minimal working example
```bash
cd tvb_framework
python -m pytest tvb/tests/framework/adapters/uploaders/region_mapping_importer_test.py --profile=TEST_SQLITE_PROFILE
python -m pytest tvb/tests/framework/interfaces/rest/simulation_resource_test.py --profile=TEST_SQLITE_PROFILE
```

```bash
# Optional service-level checks
cd tvb_framework
python -m pytest tvb/tests/framework/core/services/operation_service_test.py --profile=TEST_SQLITE_PROFILE
```

### Pitfalls and fixes
- Importer appears correct but fails at runtime: verify matching view-model/form/index classes.
- REST endpoint fix breaks launch: inspect `SimulationFacade` and `OperationService` before resource decorators.
- Test-only regressions: confirm profile and DB backend assumptions in test invocation.
- Mapping import issues: validate both H5 index and remover behavior, not only importer class.

### Convergence and validation checks
- Reproducer test passes under explicit profile.
- Adjacent framework integration test still passes.
- Operation status transitions remain consistent for launch paths touched.
- Data import/removal behavior remains consistent for affected datatypes.

### Source-code fast path
- `tvb_framework/tvb/adapters/uploaders/region_mapping_importer.py` (`RegionMappingImporter`).
- `tvb_framework/tvb/adapters/uploaders/zip_connectivity_importer.py` (`ZIPConnectivityImporter`).
- `tvb_framework/tvb/adapters/datatypes/h5/region_mapping_h5.py` (`RegionMappingH5`).
- `tvb_framework/tvb/adapters/datatypes/db/region_mapping.py` (`RegionMappingIndex`).
- `tvb_framework/tvb/core/services/operation_service.py` (`OperationService`).
- `tvb_framework/tvb/core/services/project_service.py` (`ProjectService`).
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` (`FireSimulationResource`).
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py` (`SimulationFacade`).

## Scope
- Handle questions about tvb-framework architecture, adapters, services, and framework-run workflows.
- Keep responses practical and test-backed for behavior checks.

## Primary documentation references
- `tvb_framework/README.rst`
- `tvb_framework/README_rest_client.rst`
- `tvb_framework/README_bids_monitor.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-UI.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-UI_Project.rst`

## Workflow
- Start with primary docs for intended framework layer.
- If details are missing, inspect `references/doc_map.md`.
- For implementation behavior, inspect `references/source_map.md` function-level anchors.
- Validate with focused framework tests before and after edits.
- Cite exact docs/source paths in responses.

## Test references
- `tvb_framework/tvb/tests/framework/adapters/uploaders/region_mapping_importer_test.py`
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py`
- `tvb_framework/tvb/tests/framework/core/services/operation_service_test.py`
- `tvb_framework/tvb/tests/framework/core/services/project_service_test.py`

## Source entry points for unresolved issues
- `tvb_framework/tvb/adapters/uploaders/region_mapping_importer.py`
- `tvb_framework/tvb/adapters/uploaders/zip_connectivity_importer.py`
- `tvb_framework/tvb/adapters/datatypes/h5/region_mapping_h5.py`
- `tvb_framework/tvb/adapters/datatypes/db/region_mapping.py`
- `tvb_framework/tvb/adapters/datatypes/db/removers/remover_region_mapping.py`
- `tvb_framework/tvb/adapters/visualizers/region_volume_mapping.py`
- `tvb_framework/tvb/core/services/operation_service.py`
- `tvb_framework/tvb/core/services/project_service.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_framework/tvb/adapters tvb_framework/tvb/core/services tvb_framework/tvb/interfaces/rest/server`).
