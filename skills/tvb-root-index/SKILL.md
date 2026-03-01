---
name: tvb-root-index
description: This skill should be used when users ask how to use tvb-root and the correct generated documentation skill must be selected before going deeper into source code.
---

# tvb-root Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer abstract, workflow-level guidance for large scientific packages; do not attempt full function-by-function coverage unless explicitly requested.

## Generated topic skills
- `tvb-root-inputs-and-modeling`: Inputs and Modeling (inputs, system setup, models, and physical parameterization)
- `tvb-root-build-and-install`: Build and Install (build, installation, compilation, and environment setup)
- `tvb-root-api-and-scripting`: API and Scripting (language bindings, APIs, and programmatic interfaces)
- `tvb-root-getting-started`: Getting Started (initial setup, quickstarts, and core concepts)
- `tvb-root-simulation-workflows`: Simulation Workflows (simulation setup, execution flow, and runtime controls)
- `tvb-root-examples-and-tutorials`: Examples and Tutorials (worked examples, tutorials, and cookbook usage)
- `tvb-root-developer-guide`: Developer Guide (developer architecture, extension points, and contribution workflow)
- `tvb-root-tvb-documentation`: Tvb Documentation (documentation grouped under the 'tvb-documentation' theme)
- `tvb-root-tvb-framework`: Tvb Framework (documentation grouped under the 'tvb-framework' theme)
- `tvb-root-tvb-storage`: Tvb Storage (documentation grouped under the 'tvb-storage' theme)

## Documentation-first inputs
- `tvb_contrib`
- `tvb_documentation`
- `tvb_framework`
- `tvb_library`
- `tvb_storage`
- `dev_resources/docs`

## Tutorials and examples roots
- `tvb_documentation/demos`
- `tvb_documentation/tutorials`
- `tvb_contrib/tvb/contrib/simulator/demos`
- `tvb_framework/tvb/interfaces/command/demos`

## Test roots for behavior checks
- `dev_resources/jmeter_tests`
- `tvb_framework/tvb/tests`
- `tvb_library/tvb/tests`
- `tvb_storage/tvb/tests`
- `tvb_contrib/tvb/contrib/tests`

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, search the selected topic skill `skills/<topic-skill>/references/doc_map.md`.
- If documentation still leaves ambiguity, open `skills/<topic-skill>/references/source_map.md` and inspect the suggested source entry points.
- Use targeted symbol search while inspecting source (e.g., `rg -n "<symbol_or_keyword>" tvb_deprecated tvb_bin/tvb_bin tvb_build/tvb_build tvb_contrib/tvb tvb_framework/tvb tvb_library/tvb tvb_storage/tvb`).

## Source directories for deeper inspection
- `tvb_deprecated`
- `tvb_bin/tvb_bin`
- `tvb_build/tvb_build`
- `tvb_contrib/tvb`
- `tvb_framework/tvb`
- `tvb_library/tvb`
- `tvb_storage/tvb`
