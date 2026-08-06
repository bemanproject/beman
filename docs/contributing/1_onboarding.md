# Developer Onboarding

Welcome to the Beman Project!

This guide is intended to help new contributors get from "I've never contributed before" to "I've opened my first pull request."

---

## What is the Beman Project?

The Beman Project is a community effort to develop high-quality C++ libraries intended for eventual standardization through ISO C++. Rather than existing as a single monolithic repository, Beman is an organization containing many independent libraries, each focused on a particular proposal or area of functionality. The project also maintains shared tooling, documentation, and development standards.

Every repository generally follows the same philosophy:

- Modern C++
- CMake-based builds
- Extensive automated testing
- Consistent project layout
- High-quality documentation
- Production-ready code

Because repositories share many conventions, learning one Beman repository makes it much easier to contribute to another.

---

## Prerequisites

Most repositories require:

- A C++20 (or newer) compiler
- CMake
- Git
- Ninja (recommended)
- A GitHub account

Individual repositories may require newer language versions or additional dependencies, so always read the repository's README first.

---

## Finding a Repository

Browse the organization [GitHub](https://github.com/bemanproject) page.

Choose a library that interests you. Every repository includes its own README with build instructions and project-specific information.

---

## Cloning a Repository

Fork the repository on GitHub if you plan to contribute.

Then clone your fork:

```bash
git clone https://github.com/<your-username>/<repository>.git
cd <repository>
```

Add the upstream repository:

```bash
git remote add upstream https://github.com/bemanproject/<repository>.git
```

Verify:

```bash
git remote -v
```

You should see both your fork (`origin`) and the official repository (`upstream`).

---

## Building the Project

Most Beman libraries use a standard CMake workflow.

Configure:

```bash
cmake -B build -G Ninja
```

Build:

```bash
cmake --build build
```

Run tests:

```bash
ctest --test-dir build
```

To make this easier, you can also use the workflow presets built in to CMake:

```bash
cmake --workflow --preset <preset-name>
```

> ![NOTE]
> Most users will be able to do `cmake --workflow --preset gcc-debug` for a simple build and test workflow. See the repository's README for more information.

---

## Exploring the Codebase

Before making changes, spend a little time understanding the project.

A typical repository contains directories similar to:

```
include/
src/
tests/
examples/
docs/
cmake/
```

Helpful places to start include:

- `README.md`
- `CMakeLists.txt`
- existing unit tests
- GitHub Issues

Reading existing tests is often the fastest way to understand how a library is expected to behave.

---

## Finding Something to Work On

The easiest way to begin contributing is through issues labeled **good first issue**.

These issues are intentionally selected by maintainers because they:

- have a well-defined scope
- are approachable for newcomers
- help contributors become familiar with the project
- typically do not require deep architectural knowledge

[Search](https://github.com/search?q=org%3Abemanproject+label%3A%22good+first+issue%22&type=issues&state=open) all beginner-friendly issues across the organization:

If no good first issues are currently available, consider looking for:

- documentation improvements
- additional unit tests
- typo fixes
- missing examples
- small bug fixes

---

## Before You Start

If you want to work on an issue:

1. Read the discussion.
2. Make sure nobody is already working on it.
3. Leave a comment saying you'd like to take it.
4. Wait for feedback if the issue requests maintainer confirmation.

This helps avoid duplicate work.

---

## Making Changes

Create a feature branch:

```bash
git checkout -b feature/my-change
```

Implement your changes.

Before committing, make sure:

- the project builds
- all tests pass
- new functionality includes tests
- documentation is updated if needed
- run `beman-tidy` to ensure compliance with project coding standards

---

## Committing

Create clear, descriptive commits.

Examples:

```
Fix parser edge case

Add tests for allocator support

Improve documentation for expected<T>
```

Small commits are generally easier to review than one large commit.

---

## Opening a Pull Request

Push your branch:

```bash
git push origin feature/my-change
```

Open a Pull Request against the Beman repository.

A good Pull Request should include:

- a description of the problem
- what changed
- any relevant issue number
- notes for reviewers, if applicable

---

## Code Review

Code review is a normal part of contributing.

Maintainers may request:

- additional tests
- documentation updates
- implementation changes
- style improvements

Treat review as a collaborative process. The goal is to improve both the code and the contributor experience.

---

## Need Help?

If you have questions:

- Ask in the Beman Discourse and/or on the Discord.
- Leave a comment on the issue you're working on.

The community is happy to help new contributors get started.

---

## Welcome!

Every contribution matters—whether it's fixing a typo, improving documentation, writing tests, or implementing a new feature.

We hope this is the beginning of many contributions to the Beman Project.
