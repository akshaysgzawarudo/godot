# Review Policies

## Core/rendering/driver critical internals
- **Paths**: `core/**`, `servers/rendering/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`
- **Severity**: critical
- **Reason**: Small mistakes in these low-level paths can cause crashes, corruption, GPU races, or device-specific regressions that automated tests often miss.

## GDExtension ABI and lifecycle internals
- **Paths**: `core/extension/**`, `modules/gdextension/**`, `core/object/class_db.cpp`, `core/object/script_language_extension.h`, `core/extension/gdextension_interface.json`, `misc/extension_api_validation/**`, `**/*.compat.inc`
- **Severity**: critical
- **Reason**: ABI/versioning/deprecation/lifecycle mistakes may compile but break third-party extensions and language bindings at runtime.

## Resource loading and threaded lifecycle
- **Paths**: `core/io/**`, `core/io/resource_loader*`, `core/resource/**`, `scene/resources/**`, `editor/file_system/**`
- **Severity**: critical
- **Reason**: Threaded loading/teardown changes can introduce deadlocks, races, hangs, or partially initialized resources that require stress testing.

## Object/signal concurrency internals
- **Paths**: `core/object/**`
- **Severity**: high
- **Reason**: Signal/object lifecycle and locking behavior are concurrency-sensitive and prone to subtle race or ordering regressions.

## Platform and XR runtime backends
- **Paths**: `platform/**`, `modules/openxr/**`, `modules/visionos_xr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: high
- **Reason**: Platform/XR behavior is runtime/device-dependent and can regress despite green CI without real-device validation.

## Export pipeline and platform exporters
- **Paths**: `editor/export/**`, `platform/*/export/**`
- **Severity**: high
- **Reason**: Export flow and notifier/order changes can silently break packaging/runtime behavior across targets beyond CI coverage.

## Editor UI workflow and scene sync
- **Paths**: `editor/docks/**`, `editor/scene/**`, `editor/inspector/**`, `scene/gui/**`
- **Severity**: high
- **Reason**: Stateful editor/layout/live-edit logic can regress usability or correctness in ways that require interactive manual validation.

## 3D physics joints and space semantics
- **Paths**: `scene/3d/physics/joints/**`, `servers/physics_3d/**`, `modules/jolt_physics/**`, `modules/godot_physics_3d/**`
- **Severity**: critical
- **Reason**: Coordinate-space and rotation/math changes can be subtly wrong while passing basic tests, causing runtime simulation breakage.

## Build scripts and CI configuration
- **Paths**: `.github/workflows/**`, `.github/actions/**`, `SConstruct`, `SCsub`, `methods.py`, `misc/scripts/install_*.py`, `misc/scripts/install_*.sh`, `platform/linuxbsd/**`, `platform/windows/**`, `platform/web/**`, `drivers/sdl/**`
- **Severity**: high
- **Reason**: Toolchain/detection/workflow changes can reduce coverage or break downstream builds on environments not represented in default CI.

## Third-party vendoring and license metadata
- **Paths**: `thirdparty/**`, `thirdparty/README.md`, `COPYRIGHT.txt`
- **Severity**: critical
- **Reason**: Vendored code and licensing metadata errors create legal/compliance and platform-behavior risks that automation cannot fully validate.

## GDScript language server performance/behavior
- **Paths**: `modules/gdscript/language_server/**`
- **Severity**: high
- **Reason**: Small changes can cause major latency/CPU regressions in large projects and need realistic workload testing.

## Mono/Web threading build configuration
- **Paths**: `modules/mono/build_scripts/mono_configure.py`, `modules/mono/**`, `**/*.csproj`
- **Severity**: high
- **Reason**: Mismatched WASM/Mono threading settings can produce linker/runtime incompatibilities not caught by unit tests.

## Instructions
- Humans must judge changes that alter long-standing behavior, defaults, naming, enum labels/order, or precedence/fallback semantics for migration and backward compatibility.
- Humans must judge whether warning/error visibility and severity changes are actionable, non-spammy, and appropriate across editor/runtime/debug/release contexts.
- Humans must decide when replacing explicit failures with fallback behavior is acceptable versus when it risks masking real bugs.
- Humans must approve intentional cross-layer dependency exceptions (for example core↔editor/module) and verify architectural tradeoffs.
- Humans should verify that micro-optimizations or added state/abstractions provide real benefit worth readability and maintenance costs.
- Humans must judge low-level API shape/signature/terminology changes for long-term usability, consistency, and migration burden.
- Humans must assess additions/removals of defensive checks, fallback/restoration behavior, or setter early returns for safety versus noise/regression risk.
- Humans must validate that platform/device/driver-specific workarounds are correctly scoped and do not regress unaffected users.
- Humans must judge editor control placement, visibility, iconography, and accessibility tradeoffs where multiple layouts are plausible.
- Humans should decide whether substantial wording/verbosity/terminology rewrites are worth translation and maintenance cost when behavior is unchanged.
- Humans must review blocking/wait/task lifecycle semantic changes in shared threading paths to confirm they match intended caller contracts.
- Humans should verify whether an issue is true engine defect or configuration misuse before accepting behavior-changing fixes.
- Humans must decide whether evidence and minimal reproduction quality are sufficient before merging a fix.
