# tvb-root source map: Tvb Storage

Use this map after doc_map.md when storage behavior must be verified in implementation classes.

## Fast source navigation
- `rg -n "class StorageInterface|class HDF5StorageManager|class FilesHelper" tvb_storage/tvb/storage`
- `rg -n "class DataEncryptionHandler|class XMLReader|class XMLWriter|class OperationDAO" tvb_storage/tvb/storage tvb_framework/tvb/core/entities/storage`

## Function-level entry points
- `tvb_storage/tvb/storage/storage_interface.py` | symbols: `StorageInterface` | behavior check: public storage API surface.
- `tvb_storage/tvb/storage/h5/file/hdf5_storage_manager.py` | symbols: `HDF5StorageManager` | behavior check: core H5 read/write manager.
- `tvb_storage/tvb/storage/h5/file/files_helper.py` | symbols: `FilesHelper`, `TvbZip` | behavior check: file/ZIP helper behavior.
- `tvb_storage/tvb/storage/h5/file/xml_metadata_handlers.py` | symbols: `XMLReader`, `XMLWriter` | behavior check: metadata serialization round-trip.
- `tvb_storage/tvb/storage/h5/encryption/encryption_handler.py` | symbols: `EncryptionHandler` | behavior check: encryption lifecycle hooks.
- `tvb_storage/tvb/storage/h5/encryption/data_encryption_handler.py` | symbols: `DataEncryptionHandler`, `DataEncryptionRemoteHandler` | behavior check: data-encryption queue and remote behavior.
- `tvb_storage/tvb/storage/kube/kube_notifier.py` | symbols: `KubeNotifier` | behavior check: notification path for distributed deployments.
- `tvb_framework/tvb/core/entities/storage/project_dao.py` | symbols: `CaseDAO` | behavior check: project persistence interactions.
- `tvb_framework/tvb/core/entities/storage/operation_dao.py` | symbols: `OperationDAO` | behavior check: operation persistence interactions.
- `tvb_framework/tvb/core/entities/storage/workflow_dao.py` | symbols: `WorkflowDAO` | behavior check: workflow persistence queries.
- `tvb_storage/tvb/tests/storage/storage_test.py` | symbols: `StorageTestCase` | behavior check: storage regression baseline.
- `tvb_framework/tvb/tests/framework/core/services/project_service_test.py` | symbols: `TestProjectService` | behavior check: framework integration with storage.

## Practical validation checkpoints
1. Storage tests pass for H5, metadata, and zip helper paths.
2. Encryption-enabled paths preserve read/write expectations.
3. Framework project/operation persistence still passes integration tests.
