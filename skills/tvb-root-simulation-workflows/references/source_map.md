# tvb-root source map: Simulation Workflows

Use this map after doc_map.md when simulation launch/run/restart behavior must be verified in code.

## Fast source navigation
- `rg -n "class Simulator|def run|class Integrator|class Heun" tvb_library/tvb/simulator`
- `rg -n "SimulationHistory|run_simulation|FireSimulationResource|SimulationFacade" tvb_framework/tvb`

## Function-level entry points
- `tvb_library/tvb/simulator/simulator.py` | symbols: `Simulator` | behavior check: configure/run pipeline and output tuple shape.
- `tvb_library/tvb/simulator/integrators.py` | symbols: `HeunDeterministic`, `EulerDeterministic`, `IntegratorStochastic` | behavior check: integration step behavior and stochastic handling.
- `tvb_library/tvb/simulator/_numba/integrators.py` | symbols: numba integrator variants | behavior check: accelerated path consistency.
- `tvb_library/tvb/simulator/monitors.py` | symbols: `TemporalAverage`, `Raw`, `EEG` | behavior check: monitor sampling/output semantics.
- `tvb_framework/tvb/core/entities/file/simulator/view_model.py` | symbols: `SimulatorAdapterModel`, integrator/monitor view-model classes | behavior check: framework input schema compatibility.
- `tvb_framework/tvb/core/entities/file/simulator/simulation_history_h5.py` | symbols: `SimulationHistory`, `SimulationHistoryH5` | behavior check: restart history persistence and reload.
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py` | symbols: `run_simulation` | behavior check: framework-managed run path.
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` | symbols: `FireSimulationResource.post` | behavior check: REST simulation launch behavior.
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py` | symbols: `SimulationFacade.launch_simulation` | behavior check: operation launch orchestration.
- `tvb_framework/tvb/interfaces/rest/client/simulator/simulation_api.py` | symbols: `SimulationApi.fire_simulation` | behavior check: client payload and endpoint interaction.
- `tvb_library/tvb/tests/library/simulator/simulator_test.py` | symbols: `TestSimulator` | behavior check: simulator-level regressions.
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py` | symbols: `TestSimulationResource` | behavior check: REST run contract.

## Practical validation checkpoints
1. Local script run (`sim.run()`) returns expected `(time, data)` shape.
2. Framework/REST launch reaches a terminal operation status and yields expected result datatype.
3. Restart/history path reuses compatible state with no schema mismatch.
