# tvb-root source map: Developer Guide

Use this map after doc_map.md when architecture or extension behavior must be validated in code.

## Fast source navigation
- `rg -n "class ABCAdapter|class ABCUploader|class OperationService|class ProjectService" tvb_framework/tvb`
- `rg -n "import_adapters|IntrospectionRegistry|has_finished" tvb_framework/tvb`

## Function-level entry points
- `tvb_framework/tvb/core/adapters/abcadapter.py` | symbols: `ABCAdapter`, `nan_allowed`, `nan_not_allowed` | behavior check: adapter launch contract and NaN policy.
- `tvb_framework/tvb/core/adapters/abcuploader.py` | symbols: `ABCUploader` | behavior check: uploader execution contract.
- `tvb_framework/tvb/config/init/introspector_registry.py` | symbols: `import_adapters`, `IntrospectionRegistry` | behavior check: adapter/datatype discovery.
- `tvb_framework/tvb/core/services/operation_service.py` | symbols: `OperationService` | behavior check: operation launch/status transitions.
- `tvb_framework/tvb/core/services/project_service.py` | symbols: `initialize_storage`, `ProjectService` | behavior check: project lifecycle and storage preparation.
- `tvb_framework/tvb/core/entities/model/model_operation.py` | symbols: `Operation`, `has_finished` | behavior check: persisted operation state semantics.
- `tvb_framework/tvb/adapters/uploaders/region_mapping_importer.py` | symbols: `RegionMappingImporter` | behavior check: uploader view-model/form pipeline.
- `tvb_framework/tvb/adapters/datatypes/db/region_mapping.py` | symbols: `RegionMappingIndex` | behavior check: datatype index persistence fields.
- `tvb_framework/tvb/adapters/datatypes/db/removers/remover_region_mapping.py` | symbols: `RegionMappingRemover` | behavior check: cleanup/removal constraints.
- `tvb_framework/tvb/tests/framework/core/services/operation_service_test.py` | symbols: `TestOperationService` | behavior check: expected service behavior.
- `tvb_framework/tvb/tests/framework/core/services/project_service_test.py` | symbols: `TestProjectService` | behavior check: project-storage service behavior.
- `tvb_framework/tvb/tests/framework/adapters/uploaders/region_mapping_importer_test.py` | symbols: `TestRegionMappingImporter` | behavior check: importer regression coverage.

## Practical validation checkpoints
1. Service and adapter tests pass with explicit profile flags.
2. Adapter discovery still finds expected uploaders after changes.
3. Operation status semantics remain consistent for touched code paths.
