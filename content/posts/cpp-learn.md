---
author: "Phong Nguyen"
title: "Cpp"
date: "2025-02-09"
description: "Cpp Notes"
tags: ["cpp"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 11    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
See plus plus :) .

<br>

[Refer](https://www.learncpp.com/)

## 1. Introduction
- C++ was developed as an extension to C. It adds man few features to the C language, and tis perhaps best through of as a superset of C. 


**Step 1:** Define the problem that you would like to solve
- I want to write a program that will ...

**Step 2:** Determine how you are going to solve the problem
Determine how we are going to solve the problem you came up with in step 1.
- They are straightforward (not overly complicated or confusing).

**Step 3:** Write the program
```c++
#include <iostream>

int main()
{
    std::cout << "Here is some text.";
    return 0;
}
```

**Step 4:** Compiling your source code
- We use a C++ compiler: MinGW/GCC, Clang, ... for many different OS.
- The C++ compiler sequentially goes through each source code file and does two important tasks:
  - checks your C++ code to make sure it **follows the rules** of the C++ language.
  - translates your C++ code into **machine language instructions**. These instructions are stored in an intermediate file called **an object file**.

**Step 5:** Linking object files and libraries
- After the compiler has successfully finished, another program called the linker kicks in: ar,ld, ...
- Linking is to combine all the object files and produce the desired output file (.exe, .elf, .hex ..)

> NOTE: Building refer to the full process of converting source code files into an executable that can be run.
> For complex project, build automation tools such as make, cmake are often used.

**Steps 6 & 7:** Testing and Debugging

## 2. Setup Environment, IDE (Integrated Development Environment):
- We need to installing IDE or st that comes with a compiler that supports at least C++17: **GCC/G++7, Clang++ 8,...**

- Some of the options typically does:
  - **Build:** compiles all modified code files in the project or workspace/solution, and then links the object files into an executable. If no code files have been modified since the last build, this option does nothing.
  - **Clean:** removes all cached objects and executables so the next time the project is built, all files will be recompiled and a new executable produced.
  - **Rebuild:** does a “clean”, followed by a “build”.
  - **Compile:** recompiles a single code file (regardless of whether it has been cached previously). This option does not invoke the linker or produce an executable.
  - **Run/start:** executes the executable from a prior build. Some IDEs (e.g. Visual Studio) will invoke a “build” before doing a “run” to ensure you are running the latest version of your code. Otherwise (e.g. Code::Blocks) will just execute the prior executable.


- **Examples:**
1. Create main.c
```
#include <iostream>
#include <limits>

int main(){
	std::cout << "Hello world";
	std::cin.get(); // get one more char from the user
	return 0;	
}

```
2. Run following commands
  
```makefile
Microsoft Windows [Version 10.0.19045.5371]
(c) Microsoft Corporation. All rights reserved.

C:\Users\phong.nguyen-van\Desktop\datasets\github\Cpp\example1>ls
main.cpp

C:\Users\phong.nguyen-van\Desktop\datasets\github\Cpp\example1>g++ --version
g++ (MinGW.org GCC-6.3.0-1) 6.3.0
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


C:\Users\phong.nguyen-van\Desktop\datasets\github\Cpp\example1>g++ main.cpp -o main.o

C:\Users\phong.nguyen-van\Desktop\datasets\github\Cpp\example1>g++ main.o -o main.exe

C:\Users\phong.nguyen-van\Desktop\datasets\github\Cpp\example1>main.exe
Hello world

```

## 3. Configuring the compiler
### 3.1. Build configurations:
- It is a collection of project settings that determines how the project will be built.
- **Debug configurations**: for debugging, turns off all optimizations, larger and slow, but ease
- **Release configurations**: for releasing, optimized for size and performance.
- For gcc/clang, the `-0#` option is used to control optimize settings.

### 3.2. Compiler extensions:
- Many compilers implement their own changes to the language, often to enhance compatibility with other versions of the language
- For gcc/clang, the `-pedantic-errors` option is used to disable the compiler extension.

### 3.3. Warning/Error level
- When compile the program, the compiler will check the rules of languages/compiler extension, and emit diagnostic messages.
- For gcc users: the `-Wall -Weffc++ -Wextra -Wconversion -Wsign-conversion` options is used to enable the warning levels.

### 3.4. Language  standard 
- `C++98, C++03, C++11, C++14, C++17, C++20, C++23,...`
- For gcc/g++/clang, the `-std=c++17` option is used to set the language standard.

## 4. C++ Basic
- `Data`: information
- `Value`: piece of data
- `Object`: store a value in memory
- `Variable`: an object has a name(identifier)
- `Initialization`:  provides an initial value for a variable.

1. **Initialization**
```cpp
// 1. default
int a; 

// 2. traditional
int b = 5; // copy-initialization
int c (6); // direct-initialization

// 3. Modern
int d{7};
int e{};
```

- `{}` vs `()`:
```cpp
int a{3.14};  // Compile ERROR
int b = 3.14; // Compile OK but b = 3 

int x{};  // = 0
int y;    // garbage value
```

- `copy initialization` vs `direct initialization`
```cpp
struct Foo {
    Foo(int) {}
    explicit Foo(double) {}
};

Foo f1 = 5;   // OK copy init → calls Foo(int)
Foo f2(5);    // OK direct init → calls Foo(int)

Foo f3 = 3.14; //  ERROR (explicit ctor not allowed here)
Foo f4(3.14);  //  OK direct init works with explicit → calls Foo(double)
```


2. **iostream**
- `std::cin >> x`: console input to x
- `std::cout << x`: console output
- `\n` vs `std::endl`:
    - `\n`: newline (fast, no flush)
    - `std::endl`: newline + flush output buffer (slower)

```cpp
#include <iostream>
using namespace std;

int main() {
    int x;

    // input
    cout << "Enter a number: ";
    cin >> x;   // std::cin >> x → console input

    // output
    cout << "You entered: " << x << '\n';        // \n → newline only
    cout << "You entered again: " << x << endl;  // endl → newline + flush

    return 0;
}
```


3. **Keywords and identifiers**
- Keywords: alignas alignof and and_eq asm auto bitand bitor bool break case catch char char8_t (since C++20) char16_t char32_t class compl concept (since C++20) const consteval (since C++20) constexpr constinit (since C++20) const_cast continue co_await (since C++20) co_return (since C++20) co_yield (since C++20) decltype default delete do double dynamic_cast else enum explicit export extern false float for friend goto if inline int long mutable namespace new noexcept not not_eq nullptr operator or or_eq private protected public register reinterpret_cast requires (since C++20) return short signed sizeof static static_assert static_cast struct switch template this thread_local throw true try typedef typeid typename union unsigned using virtual void volatile wchar_t while xor xor_eq

- Identifiers: `snake_case` vs `camelCase`
> When working in an existing program, use the conventions of that program. Use modern best practices when writing new programs.

3. **Literals vs Operators**
- `Literals`: fixed values like 42, 3.14, 'A', "Hello", true, nullptr.
- `Operators`:   symbols that act on values (+ - * / %, == != < >, && || !, = += -=, etc.).

4. **Expression**
- An expression is anything that produces a value.
```cpp
5 + 3        // expression → evaluates to 8
x * y - 2    // expression → depends on x, y
func(10)     // function call expression
true && flag // logical expression
```

## 5. C++ Basic: Functions and Files
1. **Function**
- Syntax:
```cpp
returnType functionName(); // forward declaration

.....

returnType functionName() // This is the function header (tells the compiler about the existence of the function)
{
    // This is the function body (tells the compiler what the function does)
}
```
- A `forward declaration` allows us to tell the compiler about the existence of an identifier before actually defining the identifier.
class B;  

2. **name space**
- `namespace` is a way to group names (variables, functions, classes) together and avoid name conflicts. It guarantees that all identifiers within the namespace are unique
```cpp
// Issue
int value = 10;

int main() {
    int value = 20;   // conflict with global value
    return 0;
}

// Solution
namespace Config {
    int value = 10;
}

int main() {
    int value = 20;
    std::cout << value << " " << Config::value;
}
```

- `using namespace`: this is a using-directive that allows us to access names in the std namespace with no namespace prefix

3. **Preprocessor**
- The preprocessor is a process that runs on the code before it is compiled.
```cpp
#include <iostream>     // insert file contents
#define NAME "Alex"     // replace NAME → "Alex"

#ifdef NAME_DEFINED     // only compile if defined
std::cout << NAME;
#endif
```

4. **Header files**
- Header files are files designed to propagate declarations to code files. 
- Include header files:
```cpp
#include <iostream>  
#include "my_header.h" 

"" → search local first, then system
<> → search system only
```

- Include header files from other directories: using `g++ -o main -I./source/includes -I/home/abc/moreHeaders main.cpp`
```cpp
// Issue: hard-coding long/absolute paths in #include
#include "headers/myHeader.h"
#include "/home/abc/moreHeaders/myOtherHeader.h"

// Solution: add include directories with -I
g++ -o main -I./source/includes -I/home/abc/moreHeaders main.cpp

#include "myHeader.h"
#include "myOtherHeader.h"
```

5. **Header guard**
- Header guards prevent the contents of a header from being included more than once into a given code file. 
- For cross-platform library code, `#ifndef` is safest.
- For modern projects using GCC/Clang/MSVC, `#pragma once` is simpler and safe.

6. **Others**
`https://www.learncpp.com/cpp-tutorial/how-to-design-your-first-programs/`