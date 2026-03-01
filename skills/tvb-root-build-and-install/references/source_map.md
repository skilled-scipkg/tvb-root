# tvb-root source map: Build and Install

Use this map after doc_map.md when setup behavior must be confirmed in code/scripts.

## Fast source navigation
- `rg -n "WEB_PROFILE|COMMAND_PROFILE|TEST_SQLITE_PROFILE|KEYCLOAK" tvb_framework/tvb/config tvb_framework/tvb/interfaces`
- `rg -n "initialize|start_tvb|initialize_flask|monitor_dir" tvb_framework/tvb tvb_build tvb_bin`

## Function-level entry points
- `tvb_build/install_full_tvb.sh` | symbols: shell install sequence | behavior check: editable install order and package dependencies.
- `tvb_framework/tvb/interfaces/web/run.py` | symbols: `init_cherrypy`, `start_tvb`, `run_browser` | behavior check: web profile startup and server bootstrap.
- `tvb_framework/tvb/interfaces/rest/server/run.py` | symbols: `initialize_tvb_flask`, `initialize_flask` | behavior check: REST startup and auth gating.
- `tvb_framework/tvb/config/profile_settings.py` | symbols: `WebSettingsProfile`, `CommandSettingsProfile`, `TestSQLiteProfile` | behavior check: profile-specific config values.
- `tvb_framework/tvb/config/init/initializer.py` | symbols: `initialize`, `command_initializer`, `reset` | behavior check: first-run init and reset semantics.
- `tvb_framework/tvb/interfaces/rest/bids_monitor/launch_bids_monitor.py` | symbols: `get_bids_dir`, `monitor_dir` | behavior check: monitor startup arguments.
- `tvb_framework/tvb/interfaces/rest/bids_monitor/bids_data_builder.py` | symbols: `BIDSDataBuilder` | behavior check: BIDS payload assembly.
- `tvb_framework/tvb/interfaces/rest/bids_monitor/bids_dir_monitor.py` | symbols: `BIDSDirWatcher` | behavior check: filesystem watch and import trigger.
- `tvb_framework/tvb/interfaces/rest/server/request_helper.py` | symbols: `set_current_user`, `get_current_user` | behavior check: request context used in server startup paths.
- `tvb_bin/clean.sh` | symbols: shell cleanup flow | behavior check: explicit user-data cleanup path.
- `tvb_framework/tvb/tests/framework/conftest.py` | symbols: profile fixtures/config setup | behavior check: expected test profile wiring.
- `tvb_framework/tvb/tests/framework/interfaces/rest/simulation_resource_test.py` | symbols: REST launch tests | behavior check: post-install REST smoke behavior.

## Practical validation checkpoints
1. `python -m tvb.interfaces.web.run WEB_PROFILE tvb.config` serves GUI on `http://localhost:8080/`.
2. `python -m tvb.interfaces.rest.server.run` serves docs on `http://localhost:9090/doc/` after auth config.
3. `python -m pytest tvb/tests/framework --profile=TEST_SQLITE_PROFILE` passes framework smoke tests.
