# tvb-root source map: Tvb Framework

Use this map after doc_map.md when framework behavior must be validated at class/function level.

## Fast source navigation
- `rg -n "class RegionMappingImporter|class ZIPConnectivityImporter|class OperationService" tvb_framework/tvb`
- `rg -n "RegionMappingIndex|RegionMappingRemover|FireSimulationResource|launch_simulation" tvb_framework/tvb`

## Function-level entry points
- `tvb_framework/tvb/adapters/uploaders/region_mapping_importer.py` | symbols: `RegionMappingImporter` | behavior check: region mapping upload pipeline.
- `tvb_framework/tvb/adapters/uploaders/zip_connectivity_importer.py` | symbols: `ZIPConnectivityImporter` | behavior check: connectivity ZIP import contract.
- `tvb_framework/tvb/adapters/uploaders/tvb_importer.py` | symbols: `TVBImporter` | behavior check: packaged datatype import path.
- `tvb_framework/tvb/adapters/datatypes/h5/region_mapping_h5.py` | symbols: `RegionMappingH5`, `RegionVolumeMappingH5` | behavior check: H5 serialization format.
- `tvb_framework/tvb/adapters/datatypes/db/region_mapping.py` | symbols: `RegionMappingIndex`, `RegionVolumeMappingIndex` | behavior check: DB index representation.
- `tvb_framework/tvb/adapters/datatypes/db/removers/remover_region_mapping.py` | symbols: `RegionMappingRemover` | behavior check: cleanup/removal behavior.
- `tvb_framework/tvb/adapters/visualizers/region_volume_mapping.py` | symbols: `RegionVolumeMappingVisualiser`, `MappedArrayVolumeVisualizer` | behavior check: volume visualization route.
- `tvb_framework/tvb/core/services/operation_service.py` | symbols: `OperationService` | behavior check: launch and status update behavior.
- `tvb_framework/tvb/core/services/project_service.py` | symbols: `ProjectService` | behavior check: project lifecycle and storage hookup.
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` | symbols: `FireSimulationResource` | behavior check: REST simulation entry point.
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py` | symbols: `SimulationFacade` | behavior check: resource-to-service execution bridge.
- `tvb_framework/tvb/tests/framework/adapters/uploaders/region_mapping_importer_test.py` | symbols: `TestRegionMappingImporter` | behavior check: importer regression contract.

## Practical validation checkpoints
1. Importer and REST simulation tests pass with explicit profile.
2. Region-mapping upload/removal round-trip preserves expected datatype state.
3. Service-level launch path remains stable for touched framework code.
