---
layout: docs
title: Coding Style Guide
description: What code style and syntax to use
menubar: docs_menu
---

# Code Style

We try and adopt the default styles used in each programming language
but there are aspects which apply to all repos. These are described below.

[Thus spake the Master Programmer:](https://www.mit.edu/~xela/tao.html)

## All languages

The below rules apply to all programming languages. Check back regularly
for changes and clarifications.

### Standard style

The folling rules always apply:-

- A short copyright statement must be added at the top of every file, with the creation year and the current year of the latest change. E.g. `// Copyright 2020-2021 Herald Project Contributors`
- The Short form opensource license reference must be included at the top of each source code file, commented out for the particular language. E.g. `//  SPDX-License-Identifier: Apache-2.0`
- British English used for all names (E.g. classes, variables) and documentation.
- Spaces instead of tabs
- No trailing spaces at the end of lines
- No empty lines at the end of files
- For long if statements, `&&` or `||` at end of each line, not the start
- 80 character single line limit
- Variable names must be descriptive unless obvious. E.g. i or idx in a loop around a single array (for index) is fine, but not a, b, c, d. Description must not be open to misinterpretation
- Function names must be accurate. E.g. an init() function called multiple times probably isn't an initialisation function

### Performance

There are som apparently trivial style changes we mandate for performance reasons:-

- Loop variables must be declared outside of tight loops
- Don't do anything that rapidly allocates/deallocates temporary memory repeatedly
- Don't do anything that results in a copy of class or memory use

### Apply Test Driven Development (TDD)

Write a test first, see it fail, write just enough until it passes. Repeat.

Whilst not strictly a style, it ensures all new code has tests.

The following are things you should think of when writing tests:-

- Don't just write sunny day tests - test for known / POSSIBLE input failures
- Test for deliberately invalid data. E.g. negative sizes, null values, missing OS APIs
- Be alive to asynchronous invocation and any needed synchronisation

### Boolean equality / inequality comparisons

Where a static value or constant is compared against a variable's value,
the variable is always to the RIGHT of the (in)equality operator.

CORRECT SYNTAX: `null == myVar`

INCORRECT SYNTAX: `myVar == null`

If you miss the final '=' symbol as in the incorrect syntax, 
you will set the value of the variable rather than compare it.
In most languages this operation returns true, which means it still compiles.
This leads to hard to find logic errors.
Writing the comparison around this way prevents this error and throws
a compilation error instead. This is much safer. It is a habit worth forming.

## Java code

### Java / Android version

We compile against Java 8 as standard and Android SDK 21 as we have to support
a very wide range of phones. SDK 21 allows us to support all Android phones
that have Bluetooth Low Energy support, globally.

Where version-specific functionality is required it MUST NOT reduce interoperability
or epidemiological efficacy in older versions.

You MUST support SDK 21 and above if you have if/else statements for OS versions.

### Standard style

The following rules apply in addition to the all languages rules:-

- 4 spaces to one tab
- Opening braces at end of line for functions / if statements

## Swift code

### Swift / iOS versions

We target Swift 5 but iOS 9.3+. This allows us to support all iOS phones
that support Bluetooth Low Energy, globally.

### Standard style

Default as per XCode. 

### Single class per file

We try to keep the same file names
as the Java and C++ equivalents, even though you can declare multiple
classes in a single source file. This makes locating code easier.

Exceptions are enumeration types, constants, and inner classes.

## C++

### C++ version

We use C++17 with no compiler extensions as standard. We do not rely on API above
this version. STL use is allowed if of the same version.

### Standard style

- Two spaces per tab
- Use `#define`'s rather than `#pragma once`, with `HERALD_` as the start of the define name
- Namespaces and class declarations have trailing `{` at the end of the lines
- Functions defined within a class declaration have trailing `{` at the end of the line
- Function definitions have both `{` and `}` on their own lines, without indent.
- A namespace declaration does not force the contained namespace/classes/functions to be indented [Example](https://github.com/theheraldproject/herald-for-cpp/blob/f86aed9289fe349028d7559ea4c562d70e988ef4/herald/src/zephyr_context.cpp#L24)
- Colon class constructor definitions are always on a new line, and always indented two spaces
- If initialisers within a colon section of a class constructor go beyond one line, they line up together (i.e. line 2+ are indented 4 spaces) [Example](https://github.com/theheraldproject/herald-for-cpp/blob/f86aed9289fe349028d7559ea4c562d70e988ef4/herald/src/zephyr_context.cpp#L34)

### Single class per file

We try to keep the same file names
as the Java equivalents, even though you can declare multiple
classes in a single source file. This makes locating code easier.

Exceptions are enumeration types, constants, and inner classes. (E.g. Impl idiom)




Continue in this guide by visiting: [Committing code]({{"/contribute/commit" | relative_url }}).