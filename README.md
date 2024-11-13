# Table of contents
- [Overview](#overview)
- [Workflows](#workflows)
   * [Build & test](#build-test)
   * [Analysis and coverage](#analysis-and-coverage)
   * [Pylint](#pylint)
   * [CodeQL](#codeql)
- [CMake](#cmake)
- [.gitignore](#gitignore)
- [.clang-format](#clang-format)
- [Licensing](#licensing)

# Overview
This repository contains reusable files including:
- `GitHub Actions workflows` that can be shared across multiple repositories.
- `CMake` files for base and test subproject.
- `.gitignore` file,
- `.clang-format` file.

# Workflows
Repository contains multiple reusable workflows. There are workflows for building and testing, onarCloud analysis and coverage reporting using SonarCloud and Codecov, custom CodeQL analysis and Pylint analysis. For some workflows there are seperate files dedicated each supported programming language. As of Nov 2024 languages are: C++, Go, Python.

> **_NOTE:_**  for some workflows there are multiple versions dedicated different programming language. Examples are for C++.

> **_NOTE:_**  C++ workflow supports project using Qt. Use `use-qt` bool param to enable/disable it, `qt-version` to set specific Qt version to use.

## Build & test
To use building and testing reusable workflow, place following file in your repo:
```yaml
name: Build & test

on: [push, pull_request]

jobs:
  linux:
    uses: przemek83/common/.github/workflows/build-and-test-cpp.yml@main
    with:
      platform: ubuntu-latest

  windows:
    uses: przemek83/common/.github/workflows/build-and-test-cpp.yml@main
    with:
      platform: windows-latest
```
Workflow will launch building and testing of project using CMake on Linux and Windows platforms. Check my other projects like `data-explorer` or `penna-model` for usage examples.

## Analysis and coverage
To use SonarCloud static analysis and coverage on Codecov and SonarCloud, place following file in your repo:
```yaml
name: Static analysis and coverage

on: [push, pull_request]

jobs:
  analyze:
    uses: przemek83/common/.github/workflows/static-analysis-and-coverage-cpp.yml@main
    secrets: inherit
```
Make sure that appropriate secrets:
- CODECOV_TOKEN - token for Codecov coverage reporting,
- SONAR_TOKEN - token for SonarCloud
 analysis and coverage reporting.

Are present in secrets section on GitHub. Also copy `sonar-project.properties` to your project and fill project key and project name. Check my other project like `data-explorer` or `penna-model` for usage examples.

## Pylint
Static analysis reusable workflow for Python projects using Pylint. To call reusable workflow place following content in your workflow file:
```
name: Pylint

on: [push, pull_request]

jobs:
  analyze:
    uses: przemek83/common/.github/workflows/pylint.yml@main
```

Check `penna-model` project for example use.

## CodeQL
Workflow for CodeQL analysis in case default version does not work correctly. Dedicated for C++ projects using Qt framework.

```
name: "CodeQL analysis"

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  schedule:
    - cron: '29 2 * * 0'

jobs:
  analyze:
    uses: przemek83/common/.github/workflows/codeql-cpp.yml@main
    with:
      use-qt: true
      qt-version: 6.5.*
```
Check `sqlite-browser` project for example use.

# CMake
CMake templates for use in C++ projects and libraries. Expected structure of C++ project is one containing sources in the `src` directory and tests in `test` directory. Expected structure for C++ library is one containing sources in `src` directory, tests in `test` directory, public headers in `include/<library name>` and use examples in `examples`. Also, present are files supporting use of `GTest` and `Catch2` testing frameworks.

For standard C++ projects, use:
- `CMakeLists.txt` as the main project file,
- `Tests.cmake.catch2` or `Tests.cmake.gtest` for initializing the appropriate testing framework,
- `test/CMakeLists.txt.catch2` or `test/CMakeLists.txt.gtest` for testing subproject.

For C++ library projects, use:
- `CMakeLists.txt.library` as the main project file,
- `library_name.pc.in` for additional configuration needed by projects using library,
- `Tests.cmake.catch2` or `Tests.cmake.gtest` for initializing the appropriate testing framework,
- `test/CMakeLists.txt.catch2` or `test/CMakeLists.txt.gtest` for testing subproject.

Check out my other projects like `data-explorer`, `penna-model` or `cpputils` for usage examples.

# .gitignore
Adjusted version of stock GitHub one for C++ projects. Added some entries related to Python and VSC use.

# .clang-format
Format file for `clang-format` tool. To be used with C++ projects enforcing coherent formatting along codebase. Just copy it to your project and setup IDE to use it.

# Licensing
Software is released under the MIT license.
