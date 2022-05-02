---
layout: default
title: Unit testing
parent: Installation
nav_order: 22
---

## Unit Testing

REST uses [GoogleTest](https://google.github.io/googletest/) as its testing framework. Unit testing is enabled via the `-DTEST=ON` flag and disabled by default. Required dependencies for testing are managed via [CMake FetchContent](https://cmake.org/cmake/help/git-master/module/FetchContent.html) so the user does not need to install any additional dependencies.

To run all tests go to the build directory (after a successful build) and type: `ctest`. All general tests will be ran for all the compiled modules. You can also run only select tests via the `--gtest_filter=TestPrefix*` which will run only tests whose names match the expression.

Passing all tests is required for all REST release versions and it is part of the pipeline. It is highly encouraged for contributors to run tests before pushing changes. Currently it recommended to use the `-DTEST=ON` only for running the tests and not enabling it otherwise.
