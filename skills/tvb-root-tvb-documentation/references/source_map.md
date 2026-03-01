# tvb-root source map: Tvb Documentation

Use this map after doc_map.md when user-facing docs must be cross-checked against implementation behavior.

## Fast source navigation
- `rg -n "class MonitorsWizardHandler|class ConnectivityViewer|class Simulator" tvb_framework/tvb tvb_library/tvb`
- `rg -n "def create_module_file|class DocGenerator" tvb_build/tvb_build/tvb_documentor`

## Function-level entry points
- `tvb_framework/tvb/interfaces/web/controllers/simulator/monitors_wizard_handler.py` | symbols: `MonitorsWizardHandler` | behavior check: simulator monitor wizard behavior shown in UI docs.
- `tvb_framework/tvb/adapters/visualizers/connectivity.py` | symbols: `ConnectivityViewer` | behavior check: connectivity viewer behavior behind user guide pages.
- `tvb_framework/tvb/adapters/visualizers/connectivity_edge_bundle.py` | symbols: `ConnectivityEdgeBundle` | behavior check: edge-bundle visualizer behavior.
- `tvb_framework/tvb/adapters/visualizers/local_connectivity_view.py` | symbols: `LocalConnectivityViewer` | behavior check: local connectivity visualizer behavior.
- `tvb_library/tvb/simulator/simulator.py` | symbols: `Simulator` | behavior check: simulator doc statements vs runtime behavior.
- `tvb_library/tvb/simulator/noise.py` | symbols: `Noise`, `Additive`, `Multiplicative` | behavior check: noise model behavior referenced in docs.
- `tvb_library/tvb/simulator/coupling.py` | symbols: `Linear`, `Sigmoidal`, `Kuramoto` | behavior check: coupling definitions in sim docs.
- `tvb_library/tvb/simulator/monitors.py` | symbols: `TemporalAverage`, `EEG`, `Raw` | behavior check: monitor semantics in user docs.
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` | symbols: `FireSimulationResource.post` | behavior check: API doc claims vs endpoint behavior.
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py` | symbols: `SimulationFacade.launch_simulation` | behavior check: server-side launch behavior behind documented workflow.
- `tvb_build/tvb_build/tvb_documentor/generate_modules.py` | symbols: `create_module_file`, `create_package_file` | behavior check: generated API docs structure.
- `tvb_build/tvb_build/tvb_documentor/doc_generator.py` | symbols: `DocGenerator` | behavior check: documentation generation orchestration.

## Practical validation checkpoints
1. Each cited user-doc behavior has at least one matching implementation anchor.
2. Visualizer/simulator behavior referenced in docs can be traced to concrete classes.
3. API doc examples map to current REST resource/facade launch path.
