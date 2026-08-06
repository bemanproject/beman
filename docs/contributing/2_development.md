# Development Guide

This document describes the conventions and development philosophy used throughout Beman libraries. While each repository is independent, contributors should strive to follow these patterns to keep projects consistent.

---

## Philosophy

Beman libraries are designed to be:

- standards-oriented
- portable
- well-tested
- readable
- implementation-independent

Whenever possible, implementations should closely follow the wording and intent of the corresponding ISO C++ proposal.

The goal is that someone familiar with the proposal should be able to navigate the implementation naturally.

---

## Organizing Code

A typical repository is organized as follows:

```
include/
    beman/
        library/
            foo.hpp
            bar.hpp
            detail/

src/ (if required)
    beman/
        library/
            foo.cpp
            bar.cpp
            detail/

tests/
    beman/
        library/
            example1.test.cpp
            example2.test.cpp
examples/
docs/
```

Public APIs belong in `include/`.

Tests belong in `tests/`.

Examples belong in `examples/`.

Documentation belongs in `docs/`.

Implementation details that are not part of the public interface should generally live in `detail/`.

---

## Organizing Around Specifications

Many Beman libraries are implementations of ISO proposals.

Rather than organizing code around implementation techniques, organize it around the specification itself.

For example, suppose a proposal contains sections such as:

```
1.2.3
    new_vector_type

1.2.3.1
    Synopsis

1.2.3.2
    Constructors

1.2.3.3
    Assignment

1.2.3.4
    Iterators

1.2.3.5
    Capacity

1.2.3.6
    Modifiers
```

Corresponding tests might naturally mirror this structure:

```
tests/
    beman/
        constructors.test.cpp
        assignment.test.cpp
        iterators.test.cpp
        modifiers.test.cpp
```

This organization makes it easy to:

- locate code from proposal wording
- determine implementation status
- compare wording against implementation
- review changes section-by-section

When reviewing a proposal, contributors should be able to answer:

> "Where is this paragraph implemented?"

with minimal searching.

---

## Following Proposal Wording

Whenever practical:

- use terminology from the proposal
- use the same function names
- preserve ordering of declarations
- preserve preconditions and postconditions
- preserve complexity requirements

This makes the implementation easier to audit against the specification.

---

## The `detail` Namespace

The `detail` namespace contains implementation details that are not part of the library's public API.

Examples include:

- helper traits
- utility algorithms
- storage implementations
- compiler workarounds
- internal customization points
- implementation-specific types

Example:

```cpp
namespace beman::foo {

class widget {
    ...
};

namespace detail {

template <class T>
constexpr bool uses_small_buffer = ...;

}

}
```

Users should not depend on anything inside `detail`.

Nothing inside `detail` is considered part of the library's stable interface.

---

## When to Use `detail`

Good candidates:

- reusable implementation helpers
- private algorithms
- helper templates
- implementation-only concepts
- compiler compatibility utilities

Poor candidates:

- public extension points
- user-visible customization APIs
- documented interfaces

If something appears in documentation, it probably should not be in `detail`.

---

## Exception Specifications

Exception specifications are part of the interface.

When a function can never throw, prefer

```cpp
noexcept
```

rather than omitting an exception specification.

When possible, use conditional noexcept:

```cpp
noexcept(std::is_nothrow_move_constructible_v<T>)
```

This preserves the guarantees of the underlying operations.

Avoid marking functions `noexcept` unless the guarantee is actually correct.

---

## Freestanding Support

Many Beman libraries are intended to support freestanding implementations whenever practical - i.e. implementations that do not require a complete environment.

Avoid unnecessary dependencies on hosted facilities.

Prefer:

- `<type_traits>`
- `<utility>`
- `<bit>`
- `<concepts>`

Be cautious when introducing:

- iostreams
- filesystem
- threads
- locale
- exceptions (where avoidable)

If hosted functionality is required, isolate it where possible.

---

## Configuration Macros

Libraries occasionally need configuration macros for:

- compiler workarounds
- platform differences
- feature detection
- optional functionality

Prefer centralized configuration headers rather than scattered `#ifdef`s throughout the implementation.

Example:

```cpp
#if BEMAN_HAS_BUILTIN_IS_CONSTANT_EVALUATED
    ...
#endif
```

instead of repeatedly checking compiler-specific macros.

---

## Keeping `#ifdef`s Local

Conditional compilation should be isolated whenever possible.

Prefer:

```cpp
#ifdef BEMAN_HAS_FOO
using storage = foo_storage;
#else
using storage = portable_storage;
#endif
```

rather than:

```cpp
#ifdef ...
void push_back(...)
#endif

#ifdef ...
iterator begin()
#endif

#ifdef ...
...
#endif
```

Minimizing scattered preprocessor logic improves readability and reduces maintenance costs.

---

## Testing

Every bug fix should include a regression test.

New functionality should include tests covering:

- normal usage
- edge cases
- constexpr evaluation
- exception behavior (when applicable)
- boundary conditions

Tests should describe observable behavior rather than implementation details.

---

## Documentation

Public APIs should be documented.

Examples are encouraged for complex features.

Documentation should explain:

- what the API does
- when to use it
- important constraints
- complexity guarantees (when relevant)

---

## Code Review

Before opening a pull request, ask:

- Does this match the proposal?
- Is the implementation simpler?
- Is the behavior tested?
- Does the implementation unnecessarily expose internals?
- Are compiler-specific details isolated?
- Are `noexcept` specifications correct?
- Is the code readable without understanding every implementation detail?

Consistency across Beman repositories is one of the project's strengths. Following these conventions helps contributors move easily between libraries and keeps implementations approachable for reviewers.
