---
name: tvb-root-build-and-install
description: This skill should be used when users ask about build and install in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Build and Install

## High-Signal Playbook

### Route conditions
- Use this skill for environment bootstrap, package install mode decisions, profile startup, and install-time failures.
- Route to `tvb-root-getting-started` for first-user GUI walkthroughs after install.
- Route to `tvb-root-api-and-scripting` for REST/client scripting after services are up.
- Route to `tvb-root-inputs-and-modeling` when the issue is test failures or model correctness rather than setup.

### Triage questions
- Are you using `TVB_Distribution`, PyPI packages, or editable monorepo source?
- Do you need GUI (`WEB_PROFILE`), scripting (`COMMAND_PROFILE` / `LIBRARY_PROFILE`), or REST server?
- Is this single-user desktop usage or headless/server usage?
- Are ports `8080`, `9090`, `8081`, `8888` free (GUI/REST/Keycloak/auth callback paths)?
- Is `tvb-data` installed from Zenodo when running full tests or demos?
- Do you need SQLite defaults or PostgreSQL-backed setup?

### Canonical workflow
1. Choose install mode: distribution bundle for fastest startup, or source/PyPI for development.
2. For editable monorepo installs, run `tvb_build/install_full_tvb.sh` from `tvb_build`.
3. For package installs, install `tvb-library` then `tvb-framework` (plus `tvb-storage` and `tvb-contrib` as needed).
4. Start GUI with `python -m tvb.interfaces.web.run WEB_PROFILE tvb.config` (expects port `8080`).
5. For headless config, define `.tvb.configuration` (or use GUI settings) and set storage/DB/threads explicitly.
6. For REST, configure Keycloak first, then run `python -m tvb.interfaces.rest.server.run`.
7. Verify startup with smoke commands, then run package-scoped tests with required extras.

### Minimal working example
```bash
# Editable monorepo install (repo clone)
cd tvb_build
sh install_full_tvb.sh

# Start TVB web profile
python -m tvb.interfaces.web.run WEB_PROFILE tvb.config
```

```bash
# PyPI install path + test extras used by TVB docs/README
python -m pip install -U tvb-library tvb-framework tvb-storage tvb-contrib
python -m pip install pytest pytest-benchmark pytest-mock BeautifulSoup4
python -m pip install h5py pytest-xdist tvb-gdist tvb-data
```

### Pitfalls and fixes
- GUI does not start: check port `8080` availability and startup logs (`tvb.interfaces.web.run`).
- First-run config write errors: set `TVB_USER_HOME` to a writable folder.
- REST does not start: `KEYCLOAK_CONFIG` missing/invalid blocks server startup.
- Missing data in demos/tests: install `tvb-data` from Zenodo (`14992335`).
- Data loss surprise: `tvb_clean`/`tvb_bin/clean.sh` resets DB and removes all TVB project data.
- DB migration setup confusion: if moving to PostgreSQL from old SQLite usage, export projects before clean/reset.

### Convergence and validation checks
- GUI reachable at `http://localhost:8080/` after startup.
- `.tvb.configuration` and `TVB_STORAGE` are created in expected location.
- REST docs reachable at `http://localhost:9090/doc/` when Keycloak path is valid.
- Framework smoke test passes with profile flag:
  - `cd tvb_framework && python -m pytest tvb/tests/framework --profile=TEST_SQLITE_PROFILE`
- Library smoke test passes:
  - `cd tvb_library && python -m pytest tvb/tests/library`

### Source-code fast path
- `tvb_build/install_full_tvb.sh` (editable multi-package install order).
- `tvb_framework/tvb/interfaces/web/run.py` (GUI profile/bootstrap path).
- `tvb_framework/tvb/interfaces/rest/server/run.py` (REST startup + Keycloak gating).
- `tvb_framework/tvb/interfaces/rest/server/README.md` (local REST + Keycloak setup sequence).
- `tvb_framework/pyproject.toml`, `tvb_library/pyproject.toml`, `tvb_storage/pyproject.toml` (optional test dependencies).

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `README.md`
- `tvb_documentation/manuals/UserGuide/UserGuide-Installation.rst`
- `tvb_documentation/manuals/UserGuide/UserGuide-Config.rst`
- `tvb_framework/README.rst`
- `tvb_library/README.rst`
- `tvb_framework/tvb/interfaces/rest/server/README.md`
- `tvb_framework/README_bids_monitor.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `tvb_documentation/demos`
- `tvb_documentation/tutorials`
- `tvb_contrib/tvb/contrib/simulator/demos`
- `tvb_framework/tvb/interfaces/command/demos`

## Test references
- `tvb_framework/tvb/tests`
- `tvb_library/tvb/tests`
- `tvb_storage/tvb/tests`
- `tvb_contrib/tvb/contrib/tests`

## Source entry points for unresolved issues
- `tvb_build/install_full_tvb.sh`
- `tvb_framework/tvb/interfaces/web/run.py`
- `tvb_framework/tvb/interfaces/rest/server/run.py`
- `tvb_framework/tvb/interfaces/rest/server/resources/simulator/simulation_resource.py`
- `tvb_framework/tvb/interfaces/rest/server/facades/simulation_facade.py`
- `tvb_framework/tvb/interfaces/rest/bids_monitor/launch_bids_monitor.py`
- `tvb_framework/tvb/interfaces/rest/bids_monitor/bids_data_builder.py`
- `tvb_framework/tvb/interfaces/rest/bids_monitor/bids_dir_monitor.py`
- `tvb_framework/tvb/interfaces/rest/server/request_helper.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_build tvb_framework tvb_library tvb_storage tvb_contrib`).
