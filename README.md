![release](https://img.shields.io/maven-central/v/app.cash.kotlin-editor/core?label=release&color=blue)  
![snapshot](https://img.shields.io/nexus/s/app.cash.kotlin-editor/core?server=https%3A%2F%2Foss.sonatype.org&label=snapshot)   
![main](https://github.com/cashapp/kotlin-editor/actions/workflows/push.yml/badge.svg)

# KotlinEditor

A library for parsing Kotlin source code into a [parse tree](https://en.wikipedia.org/wiki/Parse_tree) 
for semantic analysis, linting, and rewriting in-place. Based on the official 
[Kotlin grammar](https://kotlinlang.org/docs/reference/grammar.html). Supports normal Kotlin source,
Kotlin scripts, and Gradle Kotlin DSL.

## Project overview

The project is split between three main components:

1. A grammar and the generated parser code, from the [ANTLR](https://www.antlr.org/) tool; 
2. A "core" library with some high-level concepts build on the parse tree; and
3. A set of "recipes," that do something interesting to or with Kotlin source.

### The grammar

The grammar itself is broken into three components, one parser and two lexers:

1. The parser, `KotlinParser.g4`.
2. A lexer, `KotlinLexer.g4`.
3. Another lexer, `UnicodeClasses.g4`.

These files were all originally borrowed from the
[official Kotlin Grammar](https://kotlinlang.org/docs/reference/grammar.html).

Note that the `.tokens` files are all generated by the `antlr` tool, but are checked into the main
source set to make the IDE experience better.

### The recipes

Some sample recipes are in the `recipes` directory. Feel free to contribute new recipes if you 
believe them to be generally useful; otherwise, simply treat this library as a normal dependency for
your own projects.

## Contributing

### Applications (Recipes)

The grammar (see below) is simply the foundation on which we will build applications. These 
take the parse trees and "do something" with them. The two _categories_ of application are
**linting** and **rewriting in place**.

Applications of the grammar are in the `recipes/` directory. At time of writing, there are several
recipes available for users:

1. Centralize `repositories` declarations into a `dependencyResolutionManagement` block in Gradle
   build scripts.
2. Normalize plugin applications in a `plugins` block, using an opinionated concept of what a 
   "normal" form is (see the tests).
3. Mutate a plugins block by removing some plugins and adding others.

### The grammar

If you don't intend to interact directly with the grammar, you can skip this section, although we
still recommend having the ANTLR v4 IDEA plugin installed, at minimum.

Ensure the [ANTLR v4 IDEA plugin](https://plugins.jetbrains.com/plugin/7358-antlr-v4) is installed.

Consider buying
[The Definitive ANTLR 4 Reference](https://pragprog.com/titles/tpantlr2/the-definitive-antlr-4-reference/).
(Or just play with the sandbox and read the online documentation.)

With the plugin installed, there should be a little **ANTLR Preview** icon on your left sidebar. If
it's not there, tap `shift-cmd-a` (on macOS) and type "antlr preview" and access it that way.
Navigate to one of the grammar (`.g4`) files in `grammar/src/main/antlr`, such as `KotlinParser.g4`.
Under "Input" on the left side of the tool window, paste in some Kotlin code (as simple or complex
as you like). On the right, you can switch between the "Parse tree" and "Hierarchy" views,
each of which are useful. "Profiler" is for profiling performance issues with your grammar.

The **ANTLR Preview** tool seems to prefer to parse source by starting with the `kotlinFile` start 
rule. If you want it to parse your source as `script` instead, navigate to `KotlinParser.g4`, 
right-click on `script`, and select **Test Rule script** from the context menu.

Note that, if you edit the grammar (which you shouldn't need to do!), the tool window won't
necessarily pick up those changes immediately. The best way to deal with this is to navigate to
another file and then back to the grammar file. Changes to the "input" will, however, be read 
immediately in the right side of the tool window.

### Gradle build scans

This project is configured to publish build scans to the public 
[build scan service](https://scans.gradle.com/). Publication is disabled by default but can be 
enabled via ... TODO(tsr): find best way to allow users to opt-in (look at other OSS projects).
