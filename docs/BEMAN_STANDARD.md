<!--
SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
-->

# The Beman Standard

This document specifies rules and recommendations for Beman project libraries.
Its goal is to create consistency facilitating the evaluation of, and
contribution to Beman libraries.

## Introduction

### Core principles

This standard is driven by four core principles:

1. **[CORE.QUALITY] Highest quality**. Standards track libraries impact
   countless engineers and, consequently, should be of the highest quality.
2. **[CORE.PRODUCTION_READY] Production-ready**. Production feedback
   necessitates reliable, well-documented software.
3. **[CORE.INDUSTRY_STANDARD] Industry standard technology**. Where there's
   industry consensus on best practices, we should take advantage. Innovation in
   tooling and style is not our purpose.
4. **[CORE.INCLUSIVE] Welcoming and inclusive community**. Broad, useful
   feedback requires an unobstructed path for using, reviewing, and
   contributing to Beman libraries. This principle encompasses ergonomics,
   cross-industry participation, and cultural accommodation.

### Changing this document

This is a living document that adapts to evolving best practices and community
needs. To make changes:

1. Create a [discourse topic](https://discourse.boost.org) detailing the change
   and how it aligns with the core principles.
2. After some community discussion, create a PR with the actual change on
   [GitHub](https://github.com/bemanproject/beman) with a *leads question*
   label. The PR should also link to the discourse topic.
3. Continue discussions on the PR and discourse topic.
4. Await a leads a decision based on the community feedback.

### Conventions

This standard consists of entries that include an all-caps, dot-separated
identifier for referencing.

With the exception of the core principles, these entries are either rules or
recommendations.

- **Requirements** must be followed in order to conform to this standard. These entries
  are prefixed by "REQUIREMENT:".
- **Recommendations** should be followed in general, but specific circumstances
  may make this a less-than-ideal choice. Libraries not following a specific
  recommendation can still conform to this standard. These entries are prefixed
  by "RECOMMENDATION:".

## License

**[LICENSE.APPROVED]** REQUIREMENT: All Beman libraries must be licensed
under an approved license. These are:

1. [Apache License v2.0 with LLVM Exceptions](https://llvm.org/LICENSE.txt)
2. [Boost Software License 1.0](https://www.boost.org/LICENSE_1_0.txt)
3. [The MIT License](https://opensource.org/license/mit)

**[LICENSE.APACHE_LLVM]** RECOMMENDATION: All Beman libraries should be licensed
under the [Apache License v2.0 with LLVM
Exceptions](https://llvm.org/LICENSE.txt).

**[LICENSE.CRITERIA]** REQUIREMENT: All approved licenses must meet the
following requirements:

1. Simple to read and understand.
2. Permission without fee to copy, use and modify the software for any
   use (commercial and non-commercial).
3. Requires that the license appears on all copies of the software source code.
4. Must not require that the license appears with executables or other binary
   uses of the library.
5. Must not require that the source code be available for execution or other
   binary uses of the library.

## General

**[LIBRARY.NAMES]** RECOMMENDATION: Beman libraries names begin with `beman.`
followed by an `snake_case` short name.

Examples: `beman.smart_pointer` and `beman.sender_receiver`.

**[REPOSITORY.NAME]** RECOMMENDATION: The repository should be named after the
library name excluding the `beman.` prefix.

Examples: A `beman.smart_pointer` library's repository should be named `smart_pointer`.

**[REPOSITORY.CODEOWNERS]** REQUIREMENT: There must be a `.github/CODEOWNERS` file
with a relevant set of codeowners.

## Top-level

The top-level of a Beman library repository must consist of `CMakeLists.txt`,
`LICENSE`, and `README.md` files.

**[TOPLEVEL.CMAKE]** REQUIREMENT: There must be a `CMakeLists.txt` file at the repository's root
that builds and tests (via. CTest) the library.

**[TOPLEVEL.LICENSE]** REQUIREMENT: There must be a `LICENSE` file at the
repository's root with the contents of an approved license that covers the
contents of the repository.

**[TOPLEVEL.README]** REQUIREMENT: There must be a markdown-formatted
`README.md` file at the repository's root that describes the library, explains how
to build it, and links to further documentation.

## `README.md`

**[README.TITLE]** RECOMMENDATION: The `README.md` should begin with a level 1
header with the name of the library optionally followed with a ":" and short
description.

Examples:

```markdown
# beman.sender_receiver: Scalable Asychronous Program Building Blocks
```

**[README.PURPOSE]** RECOMMENDATION: Following the title, the `README.md` should
contain a one- or two-paragraph summary describing the library's purpose.

**[README.IMPLEMENTS]** RECOMMENDATION: Following the purpose and a newline, the
`README.md` should indicate which papers the repository implements. Use the following style:

```markdown
**Implements:** [`std::optional<T&>` (P2988R5)](https://wg21.link/P2988R5) and
[Give *std::optional* Range Support (P3168R1)](https://wg21.link/P3168R1).
```

```suggestion
**[README.LIBRARY_STATUS]**  RECOMMENDATION: Following the implements section and a newline, the `README.md` should state the library status (missing section implies development status). Beman libraries possible status: 
    * `BEMAN DEVELOPMENT`: The target paper(s) may change within the ISO standardization stages, thus also the implementation. Testing may be incomplete in this stage.
    * `BEMAN UNSTABLE`: The paper(s) may be still pending for final ISO C++ Standardization review. The library implementation matches the paper(s), but needs more testing and possible more updates to be Beman Standard conformant.
    * `BEMAN STABLE`: All target papers were accepted into the ISO C++ Working Draft and all of them were implemented in the current Beman library. The testing is completed. The code is production ready. Only bugfixes expected in this stage. Any other addition to the same utility (e.g., adding another C++29 std::optional extension on top of C++26 std::optional) would be done into a new Beman library / repo.


```markdown
**Library status:** `beman.optional26` is `BEMAN STABLE` (since (#abcdef)[https://github.com/bemanproject/optional26/commit/#abcef] - 20.11.2024).
```

## CMake

**[CMAKE.DEFAULT]** RECOMMENDATION: The root `CMakeLists.txt` should build all targets by default (including dependency targets).

**[CMAKE.PROJECT_NAME]** RECOMMENDATION: The CMake project name should be
identical to the beman library name.

**[CMAKE.LIBRARY_NAME]** RECOMMENDATION: The CMake library target's name should
be identical to the library name.

Examples:

```CMake
add_library(beman.smart_pointer STATIC)
#...
```

**[CMAKE.LIBRARY_ALIAS]** REQUIREMENT: The CMake code must create an alias of
the library target named `beman::<short_name>`. This target is intended for
external use.

Examples:

```CMake
add_library(beman::smart_pointer ALIAS beman.smart_pointer)
#...
```

**[CMAKE.TARGET_NAMES]** RECOMMENDATION: All targets, aside from the library
target, should begin with a `<library_name>.` prefix

```CMake
add_executable(beman.smart_pointer.examples.basic)
#...
add_executable(beman.smart_pointer.tests.roundtrip)
#...
```

**[CMAKE.CONFIG]** REQUIREMENT: At `install` time, a
`<library_name>Config.cmake` must be created which exports a
`beman::<short_name>` target.

**[CMAKE.SKIP_TESTS]** RECOMMENDATION: The root `CMakeLists.txt` should not build tests and their dependencies when `BEMAN_<short_name>_BUILD_TESTS` is set to `OFF` (see [CTest docs](https://cmake.org/cmake/help/latest/module/CTest.html) - similar to cmake's `BUILD_TESTING`). The option is prefixed with the project so that projects can compose. Turning on testing for the top level project should not turn on testing for dependencies. Since testing is part of the normal development workflow it is appropriate to set the option on by default for the top level project.

Use the following style:

```CMake
# <repo>/CMakeLists.txt
# ...
option(
    BEMAN_<short_name>_BUILD_TESTS
    "Enable building tests and test infrastructure. Default: ON. Values: { ON, OFF }."
    ${PROJECT_IS_TOP_LEVEL}
)

if(BEMAN_<short_name>_BUILD_TESTS)
  FetchContent_Declare(
    googletest
    EXCLUDE_FROM_ALL
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG e39786088138f2749d64e9e90e0f9902daa77c40 # release-1.15.0
  )
  FetchContent_MakeAvailable(googletest)
endif()

# ...
if(BEMAN_<short_name>_BUILD_TESTS)
  add_subdirectory(tests)
endif()
```

**[CMAKE.SKIP_EXAMPLES]** RECOMMENDATION: The root `CMakeLists.txt` should not build examples and their dependencies when `BEMAN_<short_name>_BUILD_EXAMPLES` is set to `OFF`. The option is prefixed with the project so that projects can compose. Turning on examples for the top level project should not turn on examples for dependencies. 

Use the following style:

```CMake
# <repo>/CMakeLists.txt
# ...
option(
    BEMAN_<short_name>_BUILD_EXAMPLES
    "Enable building examples. Default: ON. Values: { ON, OFF }."
    ${PROJECT_IS_TOP_LEVEL}
)

# add actual code to be build here
...

# ...
if(BEMAN_<short_name>_BUILD_EXAMPLES)
  add_subdirectory(examples)
endif()
```

**[CMAKE.AVOID_PASSTHROUGHS]** RECOMMENDATION: Avoid `CMakeLists.txt` files
consisting of a single `add_subdirectory` call.

In other words prefer,

```CMake
# <repo>/CMakeLists.txt
# ...
add_subdirectory(src/beman/optional26)
```

to,

```CMake
# <repo>/CMakeLists.txt
# ...
add_subdirectory(src) # Don't do this

# <repo>/src/CMakeLists.txt
add_subdirectory(beman) # Don't do this

# <repo>/src/beman/CMakeLists.txt
add_subdirectory(optional26) # Don't do this
```

## Directory layout

**[DIRECTORY.INTERFACE_HEADERS]** REQUIREMENT: Header files that are part of the
public interface must reside within the `include/beman/<short_name>/` directory.

Examples:

```shell
include
└── beman
    └── exemplar
        └── identity.hpp
        └── ...
        └── ...
```

**[DIRECTORY.IMPLEMENTATION_HEADERS]** REQUIREMENT: Header files residing within
`include/beman/<short_name>/` that are not part of the public interface
must either begin with `detail_` or reside within a subdirectory of
`include/beman/<short_name>/` called `detail` or begins with `detail_`.

Examples:

```shell
include
└── beman
    └── optional26
        ├── detail                           # Private implementation subdirectory.
        │   ├── iterator.hpp
        │   └── stl_interfaces
        │       ├── config.hpp
        │       ├── fwd.hpp
        │       └── iterator_interface.hpp
        └── optional.hpp                     # Public interface.
```

**[DIRECTORY.SOURCES]** RECOMMENDATION: Sources and headers not part of the
public interface should reside in the top-level `src/` directory, and should use
the same structure from `include/` - e.g., `src/beman/<short_name>/`. Check `CMAKE.AVOID_PASSTHROUGHS`.

Examples:

```shell
src
└── beman
    └── exemplar
        ├── CMakeLists.txt
        └── identity.cpp

src
└── beman
    └── optional26
        ├── CMakeLists.txt
        ├── detail
        │   └── iterator.cpp
        └── optional.cpp
```

**[DIRECTORY.TESTS]** REQUIREMENT: All test files must reside within the top-level `tests/`
directory, and should use the same structure from `include/`. If multiple test types are present,
subdirectories can be made (e.g., unit tests, performance etc).

Examples:

```shell
tests
└── beman
    └── exemplar
        └── identity.test.cpp

tests
└── beman
    └── optional26
        ├── CMakeLists.txt
        ├── detail
        │   └── iterator.test.cpp
        ├── optional.test.cpp
        ├── optional_constexpr.test.cpp
        ├── optional_monadic.test.cpp
        ├── optional_range_support.test.cpp
        ├── test_types.cpp
        ├── test_types.hpp
        ├── test_utilities.cpp
        └── test_utilities.hpp
```

**[DIRECTORY.EXAMPLES]** REQUIREMENT: If present, all example files must reside within the top-level `examples/`
directory. Each project must have at least one relevant example.

Examples:

```shell
examples
├── CMakeLists.txt
├── identity_as_default_projection.cpp
└── identity_direct_usage.cpp
```

**[DIRECTORY.DOCS]** REQUIREMENT: If present, all documentation files, except the root `README.md`, must reside within the top-level `docs/` directory. If multiple docs types are present, subdirectories can be made (e.g., dev, public/private etc).

Examples:

```shell
docs
├── debug
│   └── ci.md
├── dev
│   └── lint.md
├── local.md
└── optional26.md
```

**[DIRECTORY.PAPERS]** REQUIREMENT: If present, all paper related files (e.g., WIP LaTeX/Markdown projects for ISO Standardization), must reside within the top-level `papers/` directory.

Examples:

```shell
papers
└── P2988
    ├── Makefile
    ├── README.md
    ├── abstract.bst
    ...
```

## File layout

**[FILE.NAMES]** RECOMMENDATION: Source code and header should use the `snake_case` naming convention (similar to `LIBRARY.NAMES`).

Examples: `identity.hpp`, `identity.cpp`, `iterator_interface.hpp` or `optional_range_support.test.cpp`.

**[FILE.TEST_NAMES]** REQUIREMENT: Test source code files must use the `*.test.cpp` naming convention.

Examples: `identity.test.cpp`, `optional_ref.test.cpp` or `optional_range_support.test.cpp`.

**[FILE.LICENSE_ID]** REQUIREMENT: The [SPDX license
identifier](https://spdx.dev/learn/handling-license-info/) must be added at the
first possible line in all files which can contain a comment
(e.g., C++, scripts, CMake/Makefile, YAML/YML, JASON, XML, HTML, LaTeX, Dockerfile etc).

Examples:

* C++ files shall use the following form:

```C++
// SPDX-License-Identifier: <SPDX License Expression>
```

* CMake files and scripts shall use the following form:

```CMake
# SPDX-License-Identifier: <SPDX License Expression>
```

* Markdown files will use a comment following the title:

```markdown
# Title

<!--
SPDX-License-Identifier: <SPDX License Expression>
-->
```

**[FILE.COPYRIGHT]** RECOMMENDATION: Source code files should NOT include a
copyright notice following the SPDX license identifier.

## C++

**[CPP.NAMESPACE]** RECOMMENDATION: Headers in `include/beman/<short_name>/` should export
entities in the `beman::<short_name>` namespace.
