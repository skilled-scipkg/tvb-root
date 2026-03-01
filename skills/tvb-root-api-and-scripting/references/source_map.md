# tvb-root source map: API and Scripting

Use this map after doc_map.md when you need implementation-level behavior checks.

## Fast source navigation
- `rg -n "class TVBClient|class SimulationApi|class FireSimulationResource|def fire_simulation" tvb_framework/tvb/interfaces`
- `rg -n "browser_login|get_project_list|launch_simulation|wait_to_finish" tvb_framework/tvb/interfaces`

## Function-level entry points
- `tvb_framework/tvb/interfaces/rest/client/tvb_client.py` | symbols: `TVBClient.browser_login`, `TVBClient.get_project_list`, `TVBClient.launch_simulation` | behavior check: auth/session, project listing, launch invocation.
- `tvb_framework/tvb/interfaces/rest/client/simulator/simulation_api.py` | symbols: `SimulationApi.fire_simulation`, `SimulationApi.get_operations_for_datatype` | behavior check: payload assembly and simulation launch request.
- `tvb_framework/tvb/interfaces/rest/client/project/project_api.py` | symbols: `ProjectApi.retrieve_project`, `ProjectApi.import_project` | behavior check: project resolution/import edge cases.
- `tvb_framework/tvb/interfaces/rest/client/operation/operation_api.py` | symbols: `OperationApi.get_operation_status`, `OperationApi.get_operations_results` | behavior check: operation polling and results retrieval.
- `tvb_framework/tvb/interfaces/rest/client/main_api.py` | symbols: `MainApi.secured_request` | behavior check: request wrapping and token propagation.
- `tvb_framework/tvb/interfaces/command/lab.py` | symbols: `fire_simulation`, `fire_operation`, `wait_to_finish` | behavior check: command-profile orchestration path.
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py` | symbols: `FireSimulationResource.post` | behavior check: server endpoint contract for simulation launch.
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py` | symbols: `SimulationFacade.launch_simulation` | behavior check: bridge from REST resource to operation service.
- `tvb_framework/tvb/interfaces/rest/server/rest_api.py` | symbols: `RestApi` route registration | behavior check: endpoint availability and resource wiring.
- `tvb_framework/tvb/interfaces/rest/server/request_helper.py` | symbols: `set_current_user`, `get_current_user` | behavior check: request-scoped user context.
- `tvb_framework/tvb/interfaces/rest/server/run.py` | symbols: `initialize_tvb_flask`, `initialize_flask` | behavior check: REST bootstrap prerequisites.
- `tvb_framework/tvb/interfaces/rest/client/tests/rest_test.py` | symbols: `TestRestClient` test cases | behavior check: client-level integration expectations.

## Practical validation checkpoints
1. `python -m tvb.interfaces.rest.server.run` starts without Keycloak/config path errors.
2. `TVBClient(...).browser_login()` succeeds and `get_project_list()` returns expected projects.
3. Simulation launch returns a valid operation gid and status poll reaches terminal state.
