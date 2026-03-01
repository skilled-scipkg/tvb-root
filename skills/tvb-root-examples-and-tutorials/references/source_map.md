# tvb-root source map: Examples and Tutorials

Use this map after doc_map.md when converting docs/examples into runnable checks.

## Fast source navigation
- `rg -n "def run_simulation|def run_analyzer|def read_h5|def search_and_export_ts" tvb_framework/tvb/interfaces/command/demos`
- `rg -n "def configure_simulation|def run_simulation|def main" tvb_documentation/tutorials`

## Function-level entry points
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py` | symbols: `run_simulation` | behavior check: baseline framework simulation demo path.
- `tvb_framework/tvb/interfaces/command/demos/operations/run_analyzer.py` | symbols: `run_analyzer` | behavior check: analyzer execution on produced datatypes.
- `tvb_framework/tvb/interfaces/command/demos/datatypes/read_from_h5.py` | symbols: `read_h5` | behavior check: H5 read path used by example workflows.
- `tvb_framework/tvb/interfaces/command/demos/datatypes/search_and_export.py` | symbols: `_retrieve_entities_by_filters`, `search_and_export_ts` | behavior check: query/export workflow.
- `tvb_framework/tvb/interfaces/command/demos/datatypes/modify_h5_metadata.py` | symbols: `update_local_connectivity_metadata`, `update_written_by` | behavior check: metadata-edit helper flow.
- `tvb_framework/tvb/interfaces/command/demos/project/project_import.py` | symbols: project import script flow | behavior check: tutorial project bootstrap.
- `tvb_framework/tvb/interfaces/command/demos/project/project_export.py` | symbols: project export script flow | behavior check: tutorial export/reproducibility path.
- `tvb_documentation/tutorials/mse_analyse_region.py` | symbols: `configure_simulation`, `run_simulation`, `build_stimulus`, `main` | behavior check: tutorial-level scripted simulation and analysis.
- `tvb_documentation/tutorials/utils.py` | symbols: `run_sim_with_progress_bar`, `multiview` | behavior check: tutorial helper utility behavior.
- `tvb_contrib/tvb/contrib/simulator/demos/region_deterministic_contributed_model.py` | symbols: script entry flow | behavior check: contrib model demo launch.
- `tvb_library/tvb/tests/library/simulator/simulator_test.py` | symbols: `TestSimulator` | behavior check: baseline simulator correctness backing demos.
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py` | symbols: `TestSimulationResource` | behavior check: REST simulation path for tutorial automation.

## Practical validation checkpoints
1. Command demo simulation runs to completion and emits result datatypes.
2. H5 read/export helpers can open exported artifacts and list expected content.
3. Tutorial script changes preserve baseline behavior before parameter expansion.
