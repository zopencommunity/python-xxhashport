# Patches for python-xxhash

## Overview
This port of python-xxhash v3.7.0 to z/OS required no source code patches. The build succeeded with the following configuration:

## Build Configuration

### Key Settings
- **C Extensions**: Enabled with `-fvisibility=default` flag
- **Git Submodules**: Enabled to clone the bundled xxHash C library from `deps/xxhash/`
- **LDFLAGS**: Appended `$LIBS` in `zopen_init()` for proper C extension linking with zoslib

### Custom Test Execution
The port uses a custom test execution flow to avoid Python import shadowing:
- Tests are copied to a temporary directory outside the source tree
- The built wheel is installed into the virtual environment
- pytest runs from the temporary directory, ensuring it imports the installed wheel rather than the source tree

## Test Results
- **Total Tests**: 40
- **Passed**: 40
- **Failed**: 0
- **Success Rate**: 100%

All xxhash algorithms (xxh32, xxh64, xxh3_64, xxh3_128/xxh128) passed their test suites successfully on z/OS.

## Dependencies
- check_python (Python 3.12+)
- check_clang (C compiler for extensions)
- zoslib (z/OS compatibility layer)

## Notes
- The `-fvisibility=default` compiler flag ensures all C extension symbols are properly exported
- Git submodules are required to include the upstream xxHash C library
- No platform-specific patches were needed; the upstream code is z/OS compatible
