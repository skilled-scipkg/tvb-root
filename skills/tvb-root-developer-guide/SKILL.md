---
name: tvb-root-developer-guide
description: This skill should be used when users ask about developer guide in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Developer Guide

## High-Signal Playbook

### Route conditions
- Use this skill for extension points, adapter lifecycle, operation orchestration, and contribution workflow.
- Route to `tvb-root-build-and-install` if environment/bootstrap issues block any dev task.
- Route to `tvb-root-simulation-workflows` for simulator execution semantics.
- Route to `tvb-root-tvb-storage` for H5/encryption/storage-layer behavior.

### Triage questions
- Is the change in adapter execution, project/operation services, or datatype persistence?
- Is the target behavior in framework only, or cross-package (`tvb_library`, `tvb_storage`, `tvb_contrib`)?
- Which tests can prove behavior (framework core-services tests, adapter tests, REST resource tests)?
- Is this a new adapter/importer, or a bug in an existing execution path?

### Canonical workflow
1. Start from contributor docs and identify the owning layer (adapter, service, storage, REST).
2. Find the narrowest class/function in `references/source_map.md` and inspect its call chain.
3. Run one focused test that reproduces current behavior before making edits.
4. Implement the smallest change that preserves profile and storage expectations.
5. Re-run the same focused test, then one neighboring integration test.

### Minimal working example
```bash
# Service-level tests for operation/project behavior
cd tvb_framework
python -m pytest tvb/tests/framework/core/services/operation_service_test.py --profile=TEST_SQLITE_PROFILE
python -m pytest tvb/tests/framework/core/services/project_service_test.py --profile=TEST_SQLITE_PROFILE
```

```bash
# Adapter-level behavior check for uploaders
cd tvb_framework
python -m pytest tvb/tests/framework/adapters/uploaders/region_mapping_importer_test.py --profile=TEST_SQLITE_PROFILE
```

### Pitfalls and fixes
- Adapter changes pass unit tests but fail launch: verify view-model/form alignment in adapter classes.
- Profile-dependent failures: run framework tests with explicit profile flags.
- Service behavior drift: inspect `OperationService` + DAO interactions before changing REST resource code.
- Hidden schema assumptions: check datatype index classes and remover paths, not only importers.

### Convergence and validation checks
- Target framework test reproduces, then passes after change.
- A neighboring integration test still passes (adapter or REST resource path).
- No profile-specific regressions for `TEST_SQLITE_PROFILE` baseline.
- If touching storage boundaries, storage tests still pass.

### Source-code fast path
- `tvb_framework/tvb/core/adapters/abcadapter.py` (`ABCAdapter`, launch contract).
- `tvb_framework/tvb/core/adapters/abcuploader.py` (`ABCUploader`, importer contract).
- `tvb_framework/tvb/config/init/introspector_registry.py` (`IntrospectionRegistry`, adapter discovery).
- `tvb_framework/tvb/core/services/operation_service.py` (`OperationService`, launch/state transitions).
- `tvb_framework/tvb/core/services/project_service.py` (`ProjectService`, project/storage lifecycle).
- `tvb_framework/tvb/core/entities/model/model_operation.py` (`Operation`, status model).

## Scope
- Handle questions about developer architecture, extension points, and contribution workflow.
- Keep responses practical: map docs to concrete source entry points and validation tests.

## Primary documentation references
- `tvb_documentation/manuals/ContributorsManual/ContributorsManual.rst`
- `tvb_documentation/doc_site/top_developers.rst`
- `tvb_documentation/sim_doc/tvb.simulator.devguide.rst`
- `tvb_documentation/manuals/DeveloperReference/DeveloperReferenceManual.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If behavior details are still ambiguous, inspect `references/source_map.md` and start with function-level anchors.
- Use tests as behavior contracts before and after source edits.
- Cite exact documentation and source file paths in responses.

## Test references
- `tvb_framework/tvb/tests/framework/core/services/operation_service_test.py`
- `tvb_framework/tvb/tests/framework/core/services/project_service_test.py`
- `tvb_framework/tvb/tests/framework/adapters/uploaders/region_mapping_importer_test.py`
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py`
- `tvb_storage/tvb/tests/storage/storage_test.py`

## Source entry points for unresolved issues
- `tvb_framework/tvb/core/adapters/abcadapter.py`
- `tvb_framework/tvb/core/adapters/abcuploader.py`
- `tvb_framework/tvb/config/init/introspector_registry.py`
- `tvb_framework/tvb/core/services/operation_service.py`
- `tvb_framework/tvb/core/services/project_service.py`
- `tvb_framework/tvb/core/entities/model/model_operation.py`
- `tvb_framework/tvb/adapters/uploaders/region_mapping_importer.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_framework/tvb/core tvb_framework/tvb/adapters tvb_framework/tvb/config`).
