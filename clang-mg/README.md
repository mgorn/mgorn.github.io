# [clang-mg](https://github.com/mgorn/mg-cxx)

## Project Purpose

The purpose of this project is to implement a C++ compiler for the future of software development, where humans and AI agents work together more closely to design, generate, and maintain complex systems.

Modern C++ is powerful, but it can also be difficult to write and reason about because advanced compile-time behavior often depends on templates, macros, and indirect patterns. These tools are useful, but they can make code harder for both humans and AI systems to understand and modify safely.

This project focuses on modifying Clang to experiment with new compiler features that make C++ more direct, more configurable, and easier to generate correctly. The goal is to create a programming environment where humans can express intent clearly, while AI agents can assist in building efficient software without needing to rely on overly complicated workarounds.

In this way, the project is not only about adding new language features. It is about exploring how C++ can evolve into a better tool for human-AI collaboration, allowing both to work together effectively while still producing fast, reliable, and highly optimized software.

---

## For AI and Humans

Another important reason this project is useful is that it could make C++ easier for AI agents to work with.

AI coding tools are becoming better at generating software, but they still struggle when a language requires complicated patterns, scattered template specializations, or macro-heavy code. These approaches may be powerful, but they can make code harder for an AI agent to understand, modify, and verify.

A feature like class-scope `if constexpr` could help AI agents generate cleaner and more efficient software because the optional parts of a type would be written directly where they belong.

Instead of asking an AI agent to generate several versions of a class using templates or macros, the code could be expressed more simply:

```cpp
struct Component {
    static constexpr bool HasPhysics = true;

    int id;

    if constexpr (HasPhysics) {
        float mass;
        float velocity;
    }
};
```

This kind of structure is easier for both humans and AI systems to reason about. The condition is visible, the affected members are grouped together, and the final compiled result can still remove anything that is not needed.

This could be especially useful for AI-generated software because an AI agent could create configurable systems that are still efficient after compilation. For example, an AI agent could generate code for a game engine, embedded device, WebAssembly application, or graphics system and include only the features required for that specific build.

The result is software that is easier to generate, easier to inspect, and easier to optimize.

In this way, the project is not only about adding convenience to C++. It also explores how programming languages could evolve to better support AI-assisted development. Cleaner compile-time features could allow AI agents to produce code that is both simple at the source level and efficient at the machine level.

---

## Why This Problem Matters

C++ is often used for large, complex, and performance-critical software. In these projects, developers often need code that can change based on compile-time settings.

This is common in areas like:

- game engines
- embedded systems
- operating systems
- graphics libraries
- cross-platform applications
- WebAssembly projects
- compiler tooling
- performance-sensitive libraries

In many of these cases, developers want to remove unused code completely at compile time instead of relying on runtime checks.

A cleaner compile-time conditional system could make C++ code easier to organize. Instead of writing many separate versions of the same class, a programmer could describe the optional parts directly inside the class where they belong.

---

## Proposed Solution

This project modifies Clang to support custom language behavior. The main experimental feature is allowing `if constexpr` inside class and struct bodies.

The compiler would parse the conditional block, evaluate the compile-time condition, and only add the selected declarations to the class.

For example:

```cpp
struct Player {
    static constexpr bool HasInventory = true;

    int health;

    if constexpr (HasInventory) {
        int inventorySlots;
    }
};
```

If `HasInventory` is true, the class contains `inventorySlots`.

If it is false, that member is not added to the class at all.

This makes the class definition more direct and easier to understand.

---

## How It Is Useful

This feature is useful because it reduces the amount of boilerplate needed for compile-time configuration.

Instead of spreading logic across templates, specializations, macros, and helper types, the conditional logic can stay close to the members it controls.

This could make code:

- easier to read
- easier to modify
- easier to debug
- less dependent on macros
- more expressive
- better organized

It also gives developers more control over what exists in a type at compile time.

For projects with many build options or platform differences, this could be especially helpful.

---

## Example Use Case

A project may have a struct that needs extra debugging fields only when debug mode is enabled:

```cpp
struct Entity {
    int id;
    float x;
    float y;

    static constexpr bool DebugMode = true;

    if constexpr (DebugMode) {
        const char* debugName;
        int debugFlags;
    }
};
```

With this feature, debug-only fields can be written directly inside the struct. When debug mode is disabled, those members would not exist.

This avoids needing a separate debug version of the struct or using preprocessor macros around the fields.

---

## Patch-Based Workflow

Another important part of this project is the patch management system.

Rather than keeping all changes inside one large fork, features are saved as patch sets. This makes it easier to:

- separate experimental features
- apply features one at a time
- update against newer LLVM versions
- track what files were changed
- share or remove features cleanly

This workflow makes the project more organized and easier to maintain over time.

---

## Challenges

This project is difficult because Clang is a large and complex codebase. A language feature usually affects more than one part of the compiler.

For a feature like class-scope `if constexpr`, changes may be needed in areas such as:

- the parser
- semantic analysis
- AST declaration handling
- template instantiation
- diagnostics
- tests

It is also important that the feature behaves correctly in both simple cases and more complicated C++ cases, such as templates, access specifiers, member functions, typedefs, using declarations, and static members.

---

## Conclusion

This project tackles the problem of making compile-time C++ programming cleaner and more flexible. By modifying Clang, the project experiments with new language features such as allowing `if constexpr` inside class bodies and adding custom include behavior.

The main value of the project is that it makes certain kinds of C++ code easier to express. It reduces the need for complicated template tricks or macros and allows optional class members to be written directly where they belong.

Overall, this project is useful because it explores how C++ could become more expressive while also giving hands-on experience with one of the most important compiler infrastructures used today.
