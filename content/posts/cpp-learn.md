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

## 0. Notes
### printf/snprintf Cheat Sheet {C / C++}

#### Integer

| Data type                         | Specifier | Notes           |
| --------------------------------- | --------- | --------------- |
| `int8_t` / `signed char`          | `%hhd`    | signed 8-bit    |
| `uint8_t` / `unsigned char`       | `%hhu`    | unsigned 8-bit  |
| `int16_t` / `short`               | `%hd`     | signed 16-bit   |
| `uint16_t` / `unsigned short`     | `%hu`     | unsigned 16-bit |
| `int32_t` / `long`                | `%ld`     | signed 32-bit   |
| `uint32_t` / `unsigned long`      | `%lu`     | unsigned 32-bit |
| `int64_t` / `long long`           | `%lld`    | signed 64-bit   |
| `uint64_t` / `unsigned long long` | `%llu`    | unsigned 64-bit |

#### Floating point

| Data type     | Specifier | Notes                       |
| ------------- | --------- | --------------------------- |
| `float`       | `%f`      | 4-byte float                |
| `double`      | `%f`      | Arduino AVR: double = float |
| `long double` | `%Lf`     | depends on platform         |

- `%e` → scientific notation  
- `%g` → auto select `%f` or `%e`

#### Char / String

| Data type          | Specifier | Notes                  |
| ------------------ | --------- | ---------------------- |
| `char`             | `%c`      | single character       |
| `char*` / `String` | `%s`      | null-terminated string |

#### Pointer / Address

| Data type | Specifier | Notes               |
| --------- | --------- | ------------------- |
| `void*`   | `%p`      | memory address, hex |

#### Hex / Octal / Binary

| Data type    | Specifier   | Notes       |
| ------------ | ----------- | ----------- |
| unsigned int | `%x` / `%X` | hexadecimal |
| unsigned int | `%o`        | octal       |
| Arduino only | `%b`        | binary      |

#### Flags, Width, Precision

- `%-10d` → left-justify, width 10  
- `%010d` → pad with zeros, width 10  
- `%.2f` → 2 decimal digits  
- `%*d` → dynamic width  

#### Specific Notes

- `uint32_t` → `%lu`  
- `int32_t` → `%ld`  
- `uint16_t` → `%u`  
- `int16_t` → `%d` or `%hd`  
- `uint8_t` → `%u` or `%hhu`  
- `int8_t` → `%d` or `%hhd`  
- `float` → `%f`  
- Use `snprintf()` with correctly sized buffer to avoid overflow


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

### 4.1. Initialization
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


### 4.2. iostream
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


### 4.3. Keywords and identifiers
- Keywords: alignas alignof and and_eq asm auto bitand bitor bool break case catch char char8_t (since C++20) char16_t char32_t class compl concept (since C++20) const consteval (since C++20) constexpr constinit (since C++20) const_cast continue co_await (since C++20) co_return (since C++20) co_yield (since C++20) decltype default delete do double dynamic_cast else enum explicit export extern false float for friend goto if inline int long mutable namespace new noexcept not not_eq nullptr operator or or_eq private protected public register reinterpret_cast requires (since C++20) return short signed sizeof static static_assert static_cast struct switch template this thread_local throw true try typedef typeid typename union unsigned using virtual void volatile wchar_t while xor xor_eq

- Identifiers: `snake_case` vs `camelCase`
> When working in an existing program, use the conventions of that program. Use modern best practices when writing new programs.

### 4.4. Literals vs Operators
- `Literals`: fixed values like 42, 3.14, 'A', "Hello", true, nullptr.
- `Operators`:   symbols that act on values (+ - * / %, == != < >, && || !, = += -=, etc.).

### 4.5. Expression
- An expression is anything that produces a value.
```cpp
5 + 3        // expression → evaluates to 8
x * y - 2    // expression → depends on x, y
func(10)     // function call expression
true && flag // logical expression
```

## 5. C++ Basic: Functions and Files
### 5.1. Function
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

### 5.2. name space
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

### 5.3. Preprocessor 
- The preprocessor is a process that runs on the code before it is compiled.
```cpp
#include <iostream>     // insert file contents
#define NAME "Alex"     // replace NAME → "Alex"

#ifdef NAME_DEFINED     // only compile if defined
std::cout << NAME;
#endif
```

### 5.4. Header files
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

### 5.5. Header guard 
- Header guards prevent the contents of a header from being included more than once into a given code file. 
- For cross-platform library code, `#ifndef` is safest.
- For modern projects using GCC/Clang/MSVC, `#pragma once` is simpler and safe.

### 5.6. Others 
`https://www.learncpp.com/cpp-tutorial/how-to-design-your-first-programs/`


## 6. Debugging C++ Programs

## 7. Fundamental Data Types
- Memory can only store bits. Data type help compiler and CPU take care of encoding the value into the sequence of bits.
  
### 7.1. Basic datatype (Primitive type)

| Types                                                              | Category             | Meaning                                          | Example |
| ------------------------------------------------------------------ | -------------------- | ------------------------------------------------ | ------- |
| float, double, long double                                         | Floating Point       | a number with a fractional part                  | 3.14159 |
| bool                                                               | Integral (Boolean)   | true or false                                    | true    |
| char, wchar_t, char8_t (C++20), char16_t (C++11), char32_t (C++11) | Integral (Character) | a single character of text                       | 'c'     |
| short int, int, long int, long long int (C++11)                    | Integral (Integer)   | positive and negative whole numbers, including 0 | 64      |
| std::nullptr_t (C++11)                                             | Null Pointer         | a null pointer                                   | nullptr |
| void                                                               | Void                 | no type                                          | n/a     |

### 7.2. Sizeof 
- We can use `sizeof` can be used to return the `size of a type in bytes`.

| Category       | Type           | Minimum Size     | Typical Size       |
| -------------- | -------------- | ---------------- | ------------------ |
| Boolean        | bool           | 1 byte           | 1 byte             |
| Character      | char           | 1 byte (exactly) | 1 byte             |
| Character      | wchar_t        | 1 byte           | 2 or 4 bytes       |
| Character      | char8_t        | 1 byte           | 1 byte             |
| Character      | char16_t       | 2 bytes          | 2 bytes            |
| Character      | char32_t       | 4 bytes          | 4 bytes            |
| Integral       | short          | 2 bytes          | 2 bytes            |
| Integral       | int            | 2 bytes          | 4 bytes            |
| Integral       | long           | 4 bytes          | 4 or 8 bytes       |
| Integral       | long long      | 8 bytes          | 8 bytes            |
| Floating point | float          | 4 bytes          | 4 bytes            |
| Floating point | double         | 8 bytes          | 8 bytes            |
| Floating point | long double    | 8 bytes          | 8, 12, or 16 bytes |
| Pointer        | std::nullptr_t | 4 bytes          | 4 or 8 bytes       |

### 7.3. Signed/ Unsigned 
-  a signed integer can hold both positive and negative numbers (and 0). 
-  Unsigned integers are integers that can only hold non-negative whole numbers.

### 7.4. Fixed-width integers and size_t
- `fixed-width integers`: C++11 provides an alternate set of integer types that are guaranteed to be the same size on any architecture
```cpp
#include <cstdint>
// Fixed-width integer types

std::int8_t   i8;   // 1 byte signed   range: -128 to 127
std::uint8_t  u8;   // 1 byte unsigned range: 0 to 255

std::int16_t  i16;  // 2 bytes signed  range: -32,768 to 32,767
std::uint16_t u16;  // 2 bytes unsigned range: 0 to 65,535

std::int32_t  i32;  // 4 bytes signed  range: -2,147,483,648 to 2,147,483,647
std::uint32_t u32;  // 4 bytes unsigned range: 0 to 4,294,967,295

std::int64_t  i64;  // 8 bytes signed  range: -9,223,372,036,854,775,808 
                    //        to  9,223,372,036,854,775,807
std::uint64_t u64;  // 8 bytes unsigned range: 0 to 18,446,744,073,709,551,615
```

- `fast integers`: guarantee at least # bits, but pick the type that the CPU can process fastest (even if it uses more memory).
- `least integers`: guarantee at least # bits, but pick the type that uses the least memory (even if it’s slower).
```cpp
#include <cstdint> // for fast and least types
#include <iostream>

int main()
{
	std::cout << "least 8:  " << sizeof(std::int_least8_t)  * 8 << " bits\n";
	std::cout << "least 16: " << sizeof(std::int_least16_t) * 8 << " bits\n";
	std::cout << "least 32: " << sizeof(std::int_least32_t) * 8 << " bits\n";
	std::cout << '\n';
	std::cout << "fast 8:  "  << sizeof(std::int_fast8_t)   * 8 << " bits\n";
	std::cout << "fast 16: "  << sizeof(std::int_fast16_t)  * 8 << " bits\n";
	std::cout << "fast 32: "  << sizeof(std::int_fast32_t)  * 8 << " bits\n";

	return 0;
}
``` 

### 7.5. std::size_t 
- The type returned by the `sizeof`.
- size_t is an unsigned integral type that is used to represent the size or length of objects.

```cpp
#include <cstddef> // for std::size_t
#include <iostream>

int main()
{
	std::cout << sizeof(std::size_t) << '\n';

	return 0;
}
```

### 7.6. Scientific notation 
- `e`/`E`:  to represent the “times 10 to the power of” part of the equation. (e.g. 5.9722 x 10²⁴ -> 5.9722e24)

### 7.7. Floating point number 
- `std::cout` has a default precision of 6
- We can override the default precision that std::cout shows by using an output manipulator function named `std::setprecision()`
  
- `f` suffix means float
- `rounding error `: cannot be represented exactly in binary floating-point, so printing with high precision reveals a tiny rounding error.
```cpp
#include <iomanip> // for std::setprecision()
#include <iostream>

int main()
{
    double d{0.1};
    std::cout << d << '\n'; // use default cout precision of 6     -> 0.1
    std::cout << std::setprecision(17); //                         -> 0.10000000000000001
    std::cout << d << '\n';

    return 0;
}
```

- `Inf`: which represents infinity. Inf is signed, and can be positive (+Inf) or negative (-Inf). (5/0)
- `NaN`: which stands for “Not a Number”. (mathematically invalid)

### 7.8. Boolean values 
- `std::boolalpha`: use to print true or false (and allow std::cin to accept the words false and true as inputs)

### 7.9. Chars 
- The integer stored by a `char` variable are intepreted as an `ASCII character`.
- `std::cin.get()` this function does not ignore leading whitespace
- `Escape sequences`:
  
| Name            | Symbol       | Meaning                                        |
| --------------- | ------------ | ---------------------------------------------- |
| Alert           | `\a`         | Makes an alert, such as a beep                 |
| Backspace       | `\b`         | Moves the cursor back one space                |
| Formfeed        | `\f`         | Moves the cursor to next logical page          |
| Newline         | `\n`         | Moves cursor to next line                      |
| Carriage return | `\r`         | Moves cursor to beginning of line              |
| Horizontal tab  | `\t`         | Prints a horizontal tab                        |
| Vertical tab    | `\v`         | Prints a vertical tab                          |
| Single quote    | `\'`         | Prints a single quote                          |
| Double quote    | `\"`         | Prints a double quote                          |
| Backslash       | `\\`         | Prints a backslash                             |
| Question mark   | `\?`         | Prints a question mark *(no longer relevant)*  |
| Octal number    | `\{number}`  | Translates into char represented by octal      |
| Hex number      | `\x{number}` | Translates into char represented by hex number |


- `'t'`: Text between single quotes is treated as a char literal, which represents a single character.
- `"text"`: Text between double quotes (e.g. “Hello, world!”) is treated as a C-style string literal, which can contain multiple characters.

### 7.10. Type conversion
-  `implicit type conversion`: e.g. `double d { 5 };` // okay: int to double is safe
-  `explicit type conversion`: `static_cast<new_type>(expression)`


## 8. Constant
- A constant is a value that may not be changed during the program’s execution. C++ supports two types of constants: named constants, and literals.
- **Named constants** are constant values that are associated with an identifier. Included:
  - Constant variables
  - Macros with substitution text
  - Enumerated constant
- **Literal** are constant values that are not associated with an identifier.Literals are values that are inserted directly into the code.

### 8.1. Named constants
```cpp
// Const variable
const double gravity { 9.8 }; 

// Object-like macros with substitution text
#define MY_NAME "Phong" 

// Enumerated constant
```
> As of C++23, C++ only has two type qualifiers: const and volatile. The volatile qualifier is used to tell the compiler that an object may have its value changed at any time. This rarely-used qualifier disables certain types of optimizations.


### 8.2. Literals
- **Literals** are values that are inserted directly into the code.
- **Type of a literal** is deduced from the literal's value.
```cpp
return 5;                     // 5 is an integer literal -> type: int
bool myNameIsAlex { true };   // true is a boolean literal -> type: bool
double d { 3.4 };             // 3.4 is a double literal -> type: double
std::cout << "Hello, world!"; // "Hello, world!" is a C-style string literal -> type: const char[14]
```
- **Literal suffixes** used to explicitly declare the type for a literal.
```cpp
#include <iostream>

int main()
{
    std::cout << 5.0 << '\n';  // 5.0 (no suffix) is type double (by default)
    std::cout << 5.0f << '\n'; // 5.0f is type float

    return 0;
}
```

### 8.3. Numeral systems (decimal, binary, hexadecimal, and octal)
- Numeral system literals in C++:
    - Decimal (no prefix, 42)
    - Binary (0b101010) `b`
    - Hexadecimal (0x2A) `x`
    - Octal (052) — all represent the same value.
```cpp
#include <iostream>

int main()
{
    int bin{};    // assume 16-bit ints
    bin = 0x0001; // assign binary 0000 0000 0000 0001 to the variable
    bin = 0b1;        // assign binary 0000 0000 0000 0001 to the variable
    bin = 0b11;       // assign binary 0000 0000 0000 0011 to the variable

    std::cout << std::bitset<4>{ 0b1010 } << '\n'; // create a temporary std::bitset and print it

    // -  C++14 also adds the ability to use a quotation mark (‘) as a digit separator.
    int bin { 0b1011'0010 };  // assign binary 1011 0010 to the variable
    long value { 2'132'673'462 }; // much easier to read than 2132673462
    return 0;
}
```
- Can change the output format via use of the `std::dec, std::oct, and std::hex` I/O manipulators:
- We can define a `std::bitset` variable and tell `std::bitset` how many bits we want to store.
  
### 8.4. Constant expressions & Constexpr variables
- `Constant expressions`: expressions whose values can be fully determined at compile time.
- A compile-time constant is a constant whose value is known at compile-time.
- A runtime constant is a constant whose initialization value isn’t known until runtime.
- `constexpr` is used to ensure we get a compile-time constant variable where we desire one. Means that the object can be used in a constant expression. The value of the initializer must be known at compile-time. The constexpr object can be evaluated at runtime or compile-time.
> Benefits:
Compile-time evaluation → reduces runtime overhead by precomputing values.
Safer code → ensures that certain values (like array sizes, template parameters) are truly constant.
Optimizations → allows the compiler to inline and optimize more aggressively.
Expressiveness → lets you write functions and objects that can be used in both compile-time and runtime contexts.

```cpp
#include <iostream>
// The return value of a non-constexpr function is not constexpr
int five()
{
    return 5;
}

int main()
{
    constexpr double gravity { 9.8 }; // ok: 9.8 is a constant expression
    constexpr int sum { 4 + 5 };      // ok: 4 + 5 is a constant expression
    constexpr int something { sum };  // ok: sum is a constant expression

    std::cout << "Enter your age: ";
    int age{};
    std::cin >> age;

    constexpr int myAge { age };      // compile error: age is not a constant expression
    constexpr int f { five() };       // compile error: return value of five() is not constexpr

    return 0;
}
```

## 9. std::string
- The easiest way to work with strings and string objects in C++ is via the `std::string`/`<string>`
### 9.1. std::cout << , std::cin >> , std::getLine(std::cin >> std::ws, std::string string)
- `std::ws` tells `std::cin` to ignore leading whitespace(tab/enter/newline(s)) before extraction.  
- `std::string::length` returns length of a string that does not included the null terminator character.
- `s` suffix is a `std::string` literally, no suffix is a C-style string literally. e.g. `std::cout << "goo\n"s`
> Initializing and copy a `std::string` is slow. That's inefficient 
### 9.2. std::string_view  (C++17)
- std::string_view provides read-only access to an existing string (a C-style string literal, a std::string, or a char array) without making a copy. 
- A std::string_view that is viewing a string that has been destroyed is sometimes called a dangling view. When a std::string is modified, all views into that std::string are invalidated, meaning those views are now invalid. Using an invalidated view (other than to revalidate it) will produce undefined behavior.

- `sv` suffix is a `std::string_view` literally
- Modify a std::string is likely to invalidate all std::string_view that view into that.
- It may or may not be null-terminated.