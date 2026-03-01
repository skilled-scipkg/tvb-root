---
name: tvb-root-tvb-storage
description: This skill should be used when users ask about tvb storage in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Tvb Storage

## High-Signal Playbook

### Route conditions
- Use this skill for H5 persistence, storage layout, encryption handlers, ZIP export/import packaging, and DAO/storage boundaries.
- Route to `tvb-root-build-and-install` for environment/bootstrap failures.
- Route to `tvb-root-tvb-framework` when failures are adapter/service orchestration rather than storage implementation.
- Route to `tvb-root-simulation-workflows` for simulation launch/monitor semantics.

### Triage questions
- Is the issue read/write correctness, metadata serialization, encryption flow, or project archive handling?
- Is failure local filesystem, remote notification, or DB-backed entity linkage?
- Are you debugging a one-off file issue or regression across storage tests?
- Which operation/datatype path produces the problematic H5 files?

### Canonical workflow
1. Reproduce with one focused storage test.
2. Identify layer: storage interface, HDF5 manager, metadata XML handler, or encryption path.
3. Inspect owning class from `references/source_map.md` and trace call path.
4. Apply minimal fix in storage layer before changing framework services.
5. Re-run focused storage test plus one framework integration test that persists data.

### Minimal working example
```bash
# Storage baseline regression tests
cd tvb_storage
python -m pytest tvb/tests/storage/storage_test.py
```

```bash
# Framework integration sanity check touching storage
cd tvb_framework
python -m pytest tvb/tests/framework/core/services/project_service_test.py --profile=TEST_SQLITE_PROFILE
```

### Pitfalls and fixes
- H5 file exists but metadata missing: inspect XML handlers and metadata writer/reader pair.
- Encryption behavior inconsistent: follow `DataEncryptionHandler` queue/lifecycle paths.
- ZIP exports incomplete: verify file helper + storage interface path handling.
- DAO-level symptoms with no H5 errors: inspect framework storage DAOs before changing HDF5 manager logic.

### Convergence and validation checks
- Focused storage test passes with unchanged fixtures.
- H5 read/write round-trip preserves expected metadata keys.
- Encryption toggles preserve readable/decryptable output where expected.
- Framework service test still persists and retrieves data correctly.

### Source-code fast path
- `tvb_storage/tvb/storage/storage_interface.py` (`StorageInterface`).
- `tvb_storage/tvb/storage/h5/file/hdf5_storage_manager.py` (`HDF5StorageManager`).
- `tvb_storage/tvb/storage/h5/file/files_helper.py` (`FilesHelper`, `TvbZip`).
- `tvb_storage/tvb/storage/h5/file/xml_metadata_handlers.py` (`XMLReader`, `XMLWriter`).
- `tvb_storage/tvb/storage/h5/encryption/encryption_handler.py` (`EncryptionHandler`).
- `tvb_storage/tvb/storage/h5/encryption/data_encryption_handler.py` (`DataEncryptionHandler`).
- `tvb_framework/tvb/core/entities/storage/project_dao.py` (`CaseDAO`).
- `tvb_framework/tvb/core/entities/storage/operation_dao.py` (`OperationDAO`).

## Scope
- Handle questions about tvb-storage behavior and persistence internals.
- Keep responses implementation-grounded with concrete tests/checkpoints.

## Primary documentation references
- `tvb_storage/README.rst`
- `tvb_documentation/manuals/UserGuide/DataExchange.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Config.rst`
- `tvb_documentation/manuals/neotraits/h5.rst`

## Workflow
- Start with storage docs and config/data-exchange docs.
- If details are missing, inspect `references/doc_map.md`.
- Escalate to `references/source_map.md` for class/function behavior checks.
- Validate on storage tests before framework-wide tests.
- Cite exact docs/source paths in responses.

## Test references
- `tvb_storage/tvb/tests/storage/storage_test.py`
- `tvb_framework/tvb/tests/framework/core/services/project_service_test.py`
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py`

## Source entry points for unresolved issues
- `tvb_storage/tvb/storage/storage_interface.py`
- `tvb_storage/tvb/storage/h5/file/hdf5_storage_manager.py`
- `tvb_storage/tvb/storage/h5/file/files_helper.py`
- `tvb_storage/tvb/storage/h5/file/xml_metadata_handlers.py`
- `tvb_storage/tvb/storage/h5/encryption/encryption_handler.py`
- `tvb_storage/tvb/storage/h5/encryption/data_encryption_handler.py`
- `tvb_framework/tvb/core/entities/storage/project_dao.py`
- `tvb_framework/tvb/core/entities/storage/operation_dao.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_storage/tvb/storage tvb_framework/tvb/core/entities/storage`).
