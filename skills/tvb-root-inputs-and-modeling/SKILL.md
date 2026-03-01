---
name: tvb-root-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in tvb-root; it prioritizes documentation references and then source inspection only for unresolved details.
---

# tvb-root: Inputs and Modeling

## High-Signal Playbook

### Route conditions
- Use this skill for model/parameter input choices, connectivity/dataset setup, and simulation validation strategy.
- Use this skill first for test execution requests across framework/library/storage/contrib (pass-3 audit dependency).
- Route to `tvb-root-simulation-workflows` for run orchestration and restart/output mechanics.
- Route to `tvb-root-build-and-install` if failures are missing dependencies or environment setup.

### Triage questions
- Are we selecting scientific model parameters, or diagnosing failing tests/regressions?
- Which model family is in scope (Generic2dOscillator, Epileptor, Wong-Wang, etc.)?
- Region-level or surface-level assumptions?
- Which test suite must pass (`framework`, `library`, `storage`, `contrib`)?
- Which profile is active for framework tests (`TEST_SQLITE_PROFILE` default or `TEST_POSTGRES_PROFILE`)?
- Are optional test dependencies and `tvb-data` installed?

### Canonical workflow
1. Start from tutorial-backed model configuration defaults for a small, stable run.
2. Lock input datatypes first (connectivity, region/surface mappings, sensors/monitors where needed).
3. Validate model assumptions using short runs before long or stochastic runs.
4. Run package-focused tests in this order when debugging end-to-end issues:
   - library simulator tests,
   - framework simulation/resource tests,
   - storage tests,
   - contrib tests (RateML/cosimulation as needed).
5. For framework tests, explicitly set profile if DB backend is relevant.
6. If a test fails, locate nearest model/view-model/source adapter before changing high-level parameters.

### Minimal working example
```bash
# Library simulator/model smoke tests
cd tvb_library
python -m pytest tvb/tests/library/simulator/models_test.py
python -m pytest tvb/tests/library/simulator/integrators_test.py
```

```bash
# Framework simulation path with explicit profile
cd tvb_framework
python -m pytest tvb/tests/framework/interfaces/rest/simulation_resource_test.py --profile=TEST_SQLITE_PROFILE
```

### Pitfalls and fixes
- `ImportError` in tests: install package-specific optional test deps from `tvb_framework/pyproject.toml`, `tvb_library/pyproject.toml`, and `tvb_storage/pyproject.toml`.
- Missing demo/reference datatypes: ensure `tvb-data` is installed from Zenodo package.
- Framework DB profile mismatch: pass `--profile=TEST_POSTGRES_PROFILE` only when DB URL is configured.
- Contrib CUDA test noise: `tvb_contrib/tvb/contrib/tests/rateML/rateml_test.py` intentionally skips CUDA paths when `pycuda` is absent; treat skip vs failure differently.
- Unstable model behavior interpreted as bug: first re-check integrator step size and coupling scales against tutorial defaults.

### Convergence and validation checks
- `tvb_library` simulator tests pass for model/integrator/coupling subsets.
- Framework simulation REST/resource tests pass under intended profile.
- Storage tests pass (`tvb.tests.storage`) after framework/library changes touching I/O.
- Contrib tests run for touched paths (RateML/cosimulation) with expected skip/fail behavior documented.
- Scientific output stays qualitatively consistent when rerunning baseline tutorial parameters.

### Source-code fast path
- `tvb_library/tvb/simulator/models/base.py`, `tvb_library/tvb/simulator/models/oscillator.py`, `tvb_library/tvb/simulator/models/epileptor.py`, and `tvb_library/tvb/simulator/models/wong_wang.py` (core model definitions and dynamics functions).
- `tvb_framework/tvb/core/entities/file/simulator/view_model.py` (simulation input schema from UI/API).
- `tvb_framework/tvb/tests/framework/conftest.py` (profile flag wiring and DB fixtures).
- `tvb_library/tvb/tests/library/simulator` (library-level scientific behavior tests).
- `tvb_storage/tvb/tests/storage/storage_test.py` (storage fixture/test behavior).
- `tvb_contrib/tvb/contrib/tests/rateML/rateml_test.py` (RateML/CUDA test gating).

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `tvb_documentation/tutorials/tutorial_1_BuildingYourOwnBrainNetworkModel.rst`
- `tvb_documentation/tutorials/tutorial_3_ModelingEpilepsy.rst`
- `tvb_documentation/tutorials/tutorial_2_RestingStateNetworks.rst`
- `tvb_documentation/sim_doc/tvb.simulator.models.rst`
- `tvb_documentation/manuals/UserGuide/Complete_Dataset_Description.rst`
- `README.md`
- `tvb_framework/README.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Test references
- `README.md` (cross-package test commands and extras)
- `tvb_framework/tvb/tests`
- `tvb_library/tvb/tests`
- `tvb_storage/tvb/tests`
- `tvb_contrib/tvb/contrib/tests`

## Source entry points for unresolved issues
- `tvb_framework/tvb/core/entities/file/simulator/view_model.py`
- `tvb_library/tvb/simulator/models/__init__.py`
- `tvb_library/tvb/simulator/models/epileptor.py`
- `tvb_library/tvb/simulator/models/wong_wang.py`
- `tvb_library/tvb/simulator/models/wong_wang_exc_inh.py`
- `tvb_library/tvb/simulator/models/wilson_cowan.py`
- `tvb_framework/tvb/tests/framework/conftest.py`
- `tvb_library/tvb/tests/library/simulator/models_test.py`
- `tvb_storage/tvb/tests/storage/storage_test.py`
- `tvb_contrib/tvb/contrib/tests/rateML/rateml_test.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tvb_library/tvb/simulator/models tvb_framework/tvb/core/entities/file/simulator tvb_*/*/tests`).
