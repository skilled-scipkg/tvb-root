# tvb-root source map: Getting Started

Use this map after doc_map.md when onboarding behavior must be confirmed in implementation files.

## Fast source navigation
- `rg -n "def start_tvb|def init_cherrypy|def list_projects|def new_project" tvb_framework/tvb/interfaces`
- `rg -n "project_import|project_export|run_simulation|XML2model" tvb_framework/tvb/interfaces tvb_contrib/tvb/contrib/rateML`

## Function-level entry points
- `tvb_framework/tvb/interfaces/web/run.py` | symbols: `init_cherrypy`, `start_tvb` | behavior check: GUI bootstrap for first-run flow.
- `tvb_framework/tvb/interfaces/command/lab.py` | symbols: `list_projects`, `new_project`, `import_conn_zip`, `fire_simulation` | behavior check: command-profile onboarding actions.
- `tvb_framework/tvb/interfaces/command/demos/project/project_import.py` | symbols: project import script flow | behavior check: importing starter projects.
- `tvb_framework/tvb/interfaces/command/demos/project/project_export.py` | symbols: project export script flow | behavior check: exporting/re-sharing project artifacts.
- `tvb_framework/tvb/interfaces/command/demos/operations/run_simulation.py` | symbols: `run_simulation` | behavior check: first scripted simulation run.
- `tvb_framework/tvb/config/init/initializer.py` | symbols: `initialize`, `command_initializer` | behavior check: first-run environment setup.
- `tvb_framework/tvb/core/services/project_service.py` | symbols: `ProjectService` | behavior check: project creation and storage binding.
- `tvb_framework/tvb/interfaces/rest/client/tvb_client.py` | symbols: `TVBClient.browser_login`, `TVBClient.get_project_list` | behavior check: optional REST onboarding path.
- `tvb_contrib/tvb/contrib/rateML/XML2model.py` | symbols: `RateML` generation flow | behavior check: optional advanced starter path.
- `tvb_contrib/tvb/contrib/rateML/run/model_driver.py` | symbols: driver script argument flow | behavior check: generated model run baseline.
- `tvb_library/tvb/tests/library/simulator/simulator_test.py` | symbols: `TestSimulator` | behavior check: scientific baseline sanity for first runs.
- `tvb_framework/tvb/tests/framework/core/services/project_service_test.py` | symbols: `TestProjectService` | behavior check: project lifecycle reliability.

## Practical validation checkpoints
1. GUI or command profile starts and can list/create projects.
2. One short simulation run completes and produces a readable `TimeSeries` result.
3. Project import/export scripts succeed for starter artifacts.
