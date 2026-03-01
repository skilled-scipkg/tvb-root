# tvb-root source map: Inputs and Modeling

Use this map after doc_map.md for model dynamics, parameter assumptions, and test-backed behavior checks.

## Fast source navigation
- `rg -n "class Model|class ModelNumbaDfun|class .*Wong|class Epileptor|class WilsonCowan" tvb_library/tvb/simulator/models`
- `rg -n "class Coupling|class Monitor|class .*ViewModel" tvb_library/tvb/simulator tvb_framework/tvb/core/entities/file/simulator`

## Function-level entry points
- `tvb_library/tvb/simulator/models/base.py` | symbols: `Model`, `ModelNumbaDfun` | behavior check: base model contract and state handling.
- `tvb_library/tvb/simulator/models/oscillator.py` | symbols: `Generic2dOscillator` | behavior check: baseline tutorial model dynamics.
- `tvb_library/tvb/simulator/models/epileptor.py` | symbols: `Epileptor`, `Epileptor2D`, `_numba_dfun` | behavior check: epilepsy model differential equations.
- `tvb_library/tvb/simulator/models/wong_wang.py` | symbols: `ReducedWongWang`, `_numba_dfun` | behavior check: resting-state style model behavior.
- `tvb_library/tvb/simulator/models/wilson_cowan.py` | symbols: `WilsonCowan`, `_numba_dfun` | behavior check: excitatory/inhibitory dynamics.
- `tvb_library/tvb/simulator/coupling.py` | symbols: `Linear`, `Sigmoidal`, `Difference`, `Kuramoto` | behavior check: coupling transformations.
- `tvb_library/tvb/simulator/monitors.py` | symbols: `TemporalAverage`, `EEG`, `Raw` | behavior check: output sampling/monitoring semantics.
- `tvb_framework/tvb/core/entities/file/simulator/view_model.py` | symbols: integrator/monitor view-model classes | behavior check: framework simulation input schema.
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` | symbols: `FireSimulationResource.post` | behavior check: API launch schema acceptance.
- `tvb_library/tvb/tests/library/simulator/models_test.py` | symbols: `TestModels` | behavior check: model-level regression expectations.
- `tvb_library/tvb/tests/library/simulator/integrators_test.py` | symbols: integrator test cases | behavior check: integration stability contracts.
- `tvb_contrib/tvb/contrib/tests/rateML/rateml_test.py` | symbols: `TestRateML` | behavior check: optional generated-model pipeline and skip/fail behavior.

## Practical validation checkpoints
1. Target model tests pass after parameter/model code changes.
2. Framework simulation resource tests still accept view-model payloads.
3. Baseline tutorial parameter sets remain numerically stable for short runs.
