<!--
SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
-->

# The Beman Project

## About

### Mission

The Beman Project’s [mission](docs/MISSION_STATEMENT.md) is to **support the
efficient design and adoption of the highest quality C++ standard libraries**
through implementation experience, user feedback, and technical expertise.

### Community

We have two principle audiences: Library Developers and the C++ community.
We want to allow Library Developers to have a clear path on the road to
Standardization.  And we want to make it easy for the C++ community to use
these libraries to ensure we have real world usage.

### Governance

This project is organized by our [Governance structure](docs/GOVERNANCE.md).

### Participating and contributing

We welcome all to participate in the Beman project through
[Discourse](https://discourse.boost.org/t/welcome-to-beman-project-development/3)
discussions or GitHub contributions. Our discourse community is the best way to
stay up to date with the most recent developments.

If you are looking for ways to contribute code, see the open issues of
[beman-project repositories](https://github.com/orgs/bemanproject/repositories).
The [beman repository's issue list](https://github.com/bemanproject/beman/issues),
in particular, has implementation requests for various papers. Issues marked
with `good first issue` are perfect for new contributors and usually have an
assigned mentor. Don't hesitate to post on discourse with any questions.

Please refer to our [code of conduct](/docs/CODE_OF_CONDUCT.md) and the
[Beman standard](/docs/BEMAN_STANDARD.md) for further information about the
community and development guidelines.

### FAQ

Questions? Maybe they have already been answered in our [FAQ](docs/FAQ.md).

### About the Name

The Beman project is named in memory of Beman Dawes - co-founder of
[Boost](https://www.boost.org).

### Documentation

The Beman Project has a variety of documentation available in the [docs/](./docs/) directory of this repository.

## Getting Started

```bash
git clone https://github.com/beman-org/beman.git
cd beman
cmake -S . -B build
cmake --build build
ctest --test-dir build
```

### CMake Configuration Options

#### `BEMAN_USE_MAIN_BRANCHES`

*Type*: BOOL

*Default*: OFF

By default, the CMake workflow in this repo will clone and build all constituent
libraries from git refs as specified in `git_tag` fields in `libraries.json`. If
you would instead like to get the *latest* of each library, provide
`-DBEMAN_USE_MAIN_BRANCHES=ON` when configuring your CMake build. Be aware that
`BEMAN_USE_MAIN_BRANCHES=ON` can result in a less stable user experience as some
versions of some libraries may not be published or even fully tested yet.

## Adding a Library

Libraries are enumerated in `libraries.json` in this repository. To add a
library to The Beman Project, create a pull request adding a new object to the
`libraries` field in that JSON file.

### Library Object JSON Schema

### Example

This is an example JSON object representing a Beman Project library:

```json
{
  "name": "examplar",
  "git_repository": "https://github.com/bemanproject/examplar.git",
  "git_tag": "375f3e7",
  "default_branch": "main"
}
```

#### `name`

*Type*: String

A *unique*, logical name for the library.

#### `git_repository`

*Type*: String

A URL for cloning the repository containing the library. This URL does not need
to be part of the `bemanproject` GitHub organization, or even on GitHub as
long as the repo is publicly accessible for cloning.

#### `git_tag`

*Type*: String

A git reference (tag, branch, commit, etc.) that contains a known-working
version of the provided library. It is recommended to keep this field
up-to-date as the library project and its associated paper evolves.

#### `default_branch`

*Type*: String

The default branch of the library repository, such as `main`. This will be used
in workflows where users want the *latest* version of every library, regardless
of whether the libraries work, either individually or integrated together.
