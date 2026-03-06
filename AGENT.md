# AGENT.md - Repository Guide (CycloneDDS)

This repository contains the core C implementation of Eclipse Cyclone DDS.
Primary build system: CMake (>= 3.16). Primary code lives under `src/`.

## Quick Repo Map

- `CMakeLists.txt`
  - Top-level build entry.
  - Defines global compile options, feature toggles, and integrates optional deps
    (e.g., OpenSSL, iceoryx).

- `src/`
  - Main source tree (built via `src/CMakeLists.txt`).
  - Module layout:
    - `src/core/`
      - `src/core/ddsc/`  DDS C API implementation (user-facing `dds_*` API)
      - `src/core/ddsi/`  DDSI-RTPS protocol stack (discovery/transport/etc.)
      - `src/core/xtests/` Extended tests / stress tests
    - `src/ddsrt/`        Runtime abstraction layer (threads/time/atomics/sockets)
    - `src/idl/`          IDL parsing/processing (used by tools/tests)
    - `src/security/`     OMG DDS Security (API, core library, built-in plugins)
    - `src/tools/`        Tools (e.g., `idlc`, `idlpp`, `ddsperf`, config generator)

- `docs/`
  - `docs/dev/` design notes and developer docs.
  - Start here for architecture: `docs/dev/modules.md`.

- `examples/`
  - Sample applications.
  - Useful reference for a "topic viewer": `examples/listtopics/listtopics.c`.

- `ports/`
  - Platform ports and special environments (Android, FreeRTOS-POSIX, Solaris 2.6).

- `fuzz/` fuzzing targets
- `scripts/` helper scripts (incl. docker)

## Common Build Commands

Configure + build:

```sh
mkdir -p build
cmake -S . -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build build
```

With examples:

```sh
cmake -S . -B build -DBUILD_EXAMPLES=ON
cmake --build build
```

With tests (requires CUnit):

```sh
cmake -S . -B build -DBUILD_TESTING=ON
cmake --build build
ctest --test-dir build
```

## Selected Feature Toggles (CMake)

- `BUILD_EXAMPLES=ON/OFF`
- `BUILD_TESTING=ON/OFF`
- `BUILD_IDLC=ON/OFF`
- `BUILD_DDSPERF=ON/OFF`
- `ENABLE_SSL=ON/OFF/AUTO`        (OpenSSL)
- `ENABLE_SHM=ON/OFF/AUTO`        (iceoryx shared memory)
- `ENABLE_SECURITY=ON/OFF`        (DDS Security hooks + plugins)
- `ENABLE_TYPE_DISCOVERY=ON/OFF`  (XTypes/type discovery support)
- `ENABLE_TOPIC_DISCOVERY=ON/OFF` (topic discovery; requires type discovery)

See `README.md` for a broader list.

## Where To Start Reading

- `docs/dev/modules.md` (module boundaries and intent)
- `src/core/ddsc/` (public DDS API surface and behavior)
- `src/core/ddsi/` (wire protocol, discovery, transports)
- `src/ddsrt/` (portability layer used by everything else)

## Run
export CYCLONEDDS_URI=./cyclonedds.xml 