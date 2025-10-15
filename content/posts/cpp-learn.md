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

#### Code Timming
-  C++11 comes with some functionality in the chrono library to time our code to see how long it takes to run.
-  e.g.
```cpp
#include <array>
#include <chrono> // for std::chrono functions
#include <cstddef> // for std::size_t
#include <iostream>
#include <numeric> // for std::iota

const int g_arrayElements { 10000 };

class Timer
{
private:
    // Type aliases to make accessing nested type easier
    using Clock = std::chrono::steady_clock;
    using Second = std::chrono::duration<double, std::ratio<1> >;

    std::chrono::time_point<Clock> m_beg{ Clock::now() };

public:

    void reset()
    {
        m_beg = Clock::now();
    }

    double elapsed() const
    {
        return std::chrono::duration_cast<Second>(Clock::now() - m_beg).count();
    }
};

void sortArray(std::array<int, g_arrayElements>& array)
{

    // Step through each element of the array
    // (except the last one, which will already be sorted by the time we get there)
    for (std::size_t startIndex{ 0 }; startIndex < (g_arrayElements - 1); ++startIndex)
    {
        // smallestIndex is the index of the smallest element we’ve encountered this iteration
        // Start by assuming the smallest element is the first element of this iteration
        std::size_t smallestIndex{ startIndex };

        // Then look for a smaller element in the rest of the array
        for (std::size_t currentIndex{ startIndex + 1 }; currentIndex < g_arrayElements; ++currentIndex)
        {
            // If we've found an element that is smaller than our previously found smallest
            if (array[currentIndex] < array[smallestIndex])
            {
                // then keep track of it
                smallestIndex = currentIndex;
            }
        }

        // smallestIndex is now the smallest element in the remaining array
        // swap our start element with our smallest element (this sorts it into the correct place)
        std::swap(array[startIndex], array[smallestIndex]);
    }
}

int main()
{
    std::array<int, g_arrayElements> array;
    std::iota(array.rbegin(), array.rend(), 1); // fill the array with values 10000 to 1

    Timer t;

    sortArray(array);

    std::cout << "Time taken: " << t.elapsed() << " seconds\n";

    return 0;
}
```

- **Things that can impact the performance of the program:** TBD
- **Measuring performance:**
  - gather at least 3 results.
  - the program runs in 10 seconds etc
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
- An expression is anything that produces a value when compilling.
> Khai báo: tên biến chỉ là identifier (chưa phải expression).
Sử dụng: tên biến trở thành một expression

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
- A `forward declaration` allows us to tell the compiler about the existence of an identifier before actually defining the identifier. We have to be explicit about the return type.
- e.g.
```cpp
#include <iostream>
#include <type_traits> // for std::common_type_t

// Forward declare
template <typename T, typename U>
auto max(T x, U y) -> std::common_type_t<T, U>; // returns the common type of T and U

int main()
{
    std::cout << max(2, 3.5) << '\n';

    return 0;
}

// Definition
template <typename T, typename U>
auto max(T x, U y) -> std::common_type_t<T, U>
{
    return (x < y) ? y : x;
}

```

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
- define namespaces and use the scope resolution operator (::) to access.
- the scope resolution operator can also be used in front of an identifier without providing a namespace name.
- namespaces can be nested inside other namespaces.
- we can create namespace aliases, which allow us to temporarily shorten a long sequence of namespaces into something shorter.

```cpp
void doSomething() // this print() lives in the global namespace
{
	std::cout << " there\n";
}
namespace Foo // define a namespace named Foo
{
    // This doSomething() belongs to namespace Foo
    int doSomething(int x, int y)
    {
        return x + y;
    }
}

int main()
{
    std::cout << Foo::doSomething(4, 3) << '\n'; // use the doSomething() that exists in namespace Foo
    std::cout << ::doSomething(4, 3) << '\n'; // use the global doSomething()

    namespace Active = Foo::Goo; // active now refers to Foo::Goo
    return 0;
}
``` 

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
- Fundamental Data Types Compound Data Types
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
- `constexpr` is used to ensure we get a compile-time constant variable where we desire one. Means that the object can be used in a constant expression. The value of the initializer must be known at compile-time. The constexpr object can be evaluated at runtime or compile-time.(immediate constant expression)
> Benefits:
Compile-time evaluation → reduces runtime overhead by precomputing values.
Safer code → ensures that certain values (like array sizes, template parameters) are truly constant.
Optimizations → allows the compiler to inline and optimize more aggressively.
Expressiveness → lets you write functions and objects that can be used in both compile-time and runtime contexts.
- `const` can be fully determined at runtime\compile time.
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
### 8.6. Constexpr functions
-  `constexpr` function is a function that is allowed to be called in a constant expression. 
  -  guaranteed to be evaluated at compile-time when used in a context that requires a constant expression.
  -  may be evaluated at compile-time (if eligible) or runtime in a other contexts.
  -  are implicitly inline, and the compiler must see the full definition of the constexpr function to call it at compile-time.
- `consteval` function is a function that must evaluate at compile-time. Consteval functions otherwise follow the same rules as constexpr functions.

- e.g.
```cpp
#include <iostream>
#include <array>

//  constexpr function: can be evaluated at compile-time *or* runtime
constexpr int square(int x) {
    return x * x;
}

//  consteval function: must be evaluated at compile-time
consteval int cube(int x) {
    return x * x * x;
}

int main() {
    // --- Compile-time evaluation ---
    constexpr int a = square(5);     // evaluated at compile-time
    constexpr int b = cube(3);       // must be compile-time

    // --- Runtime evaluation ---
    int n;
    std::cin >> n;                   // input at runtime
    int c = square(n);               // evaluated at runtime (since n is not constant)
    // int d = cube(n);              //  ERROR: consteval requires compile-time argument

    std::cout << "square(5) = " << a << "\n";
    std::cout << "cube(3) = " << b << "\n";
    std::cout << "square(n) = " << c << "\n";

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

## 10. Operators & Bit Manipulation
### 10.1. Operators
- ez , refer : https://www.learncpp.com/cpp-tutorial/operator-precedence-and-associativity/
- `Increment/decrement`:
  -  `++x`: increment x, then return x
  -  `x++`: copy x, then increment x, return the copy
- `Comma`: 
  - `(x, y)`: Evaluate x then y, returns value of y
  - avoid use this

### 10.2. bit manipulation.
- To define a set of bit flags, use `uint8/16/32`... or `std::bitset`
- Refers: https://www.learncpp.com/cpp-tutorial/bit-flags-and-bit-manipulation-via-stdbitset/

## 11. Scope, duration, and linkage summary
> A variable’s duration determines when it is created and destroyed.
Variables with automatic duration: are created at the point of definition, and destroyed when the block they are part of is exited. This includes:
    - Local variables
    - Function parameters
Variables with static duration: are created when the program begins and destroyed when the program ends. This includes:
    - Global variables
    - Static local variables
Variables with dynamic duration:  are created and destroyed by programmer request. This includes:
    - Dynamically allocated variables

> extern	static (or thread_local) storage duration and external linkage	
static	static (or thread_local) storage duration and internal linkage	
thread_local	thread storage duration	
mutable	object allowed to be modified even if containing class is const	
auto	automatic storage duration	Deprecated in C++11
register	automatic storage duration and hint to the compiler to place in a register	Deprecated in C++17

- When you write an implementation file (.cpp, .cxx, etc) your compiler generates a translation unit. This is the source file from your implementation plus all the headers you #included in it.
Internal linkage refers to everything only in scope of a translation unit.
External linkage refers to things that exist beyond a particular translation unit. In other words, accessible through the whole program, which is the combination of all translation units (or object files).

![image](/images/learncpp_1.png)

<br>

### 11.1. Internal linkage
- An identifier’s linkage determines whether a declaration of that same identifier in a different scope refers to the same entity (object, function, reference, etc…) or not.

- An identifier with no linkage means another declaration of the same identifier refers to a unique entity. Entities whose identifiers have no linkage include:
Local variables
Program-defined type identifiers (such as enums and classes) declared inside a block

- An identifier with `internal linkage` means a declaration of the same identifier within the same translation unit refers to the same object or function. Entities whose identifiers have internal linkage include:
    - Static global variables (initialized or uninitialized)
    - Static functions
    - Const global variables
    - Unnamed namespaces and anything defined within them
- To make things to internal linkage, we can:
  - `static` global variables/functions
  - `const` and `constexpr` globals ((and thus don’t need the static keyword -- if it is used, it will be ignored)) ## C
  - unnamed namespace { ... } (modern C++)

- `static` <global_variable>: make the global variable  to internal linkage
- `static` <local_variable>: changes its duration from automatic duration to static duration. And its initializer is only executed once.
- `static` <const/constexpr local_varialbe>: used to avoid expensive local object initialization each time a function is called because it inits once time.

- Internal linkage for const global variables can change to external with the keyword `extern`. e.g. `extern const PI = 3.14;`
- e.g.
```cpp
// a.cpp ============================================================
#include <iostream>

static int g_internal { 42 };    // internal linkage (only in a.cpp)
static void helper() {           // internal function
    std::cout << "Helper in a.cpp\n";
}

int main() {
    std::cout << g_internal << '\n'; // OK
    helper();                         // OK
    return 0;
}

// main.cpp ============================================================
extern int g_internal; //  ERROR: g_internal not visible outside a.cpp
void helper();         //  ERROR: helper not visible outside a.cpp

int main() {
    // g_internal; // linker error if uncommented
    // helper();   // linker error if uncommented
    return 0;
}
```

### 11.2. External linkage
- An identifier with `external linkage` means a declaration of the same identifier within the entire program refers to the same object or function. Entities whose identifiers have external linkage include:
    - Non-static functions
    - Non-const global variables (initialized or uninitialized)
    - Extern const global variables
    - Inline const global variables
    - Namespaces
- `functions`: default external linkage
- `global variables`: 
  - `non-const globals`: external by default.
  - `const/constexpr globals`: internal by default
- To access an external global variable from another file, use `extern` without initializer (forward declaration).

- e.g.
```cpp
// a.cpp ============================================================
#include <iostream>

int g_external { 100 };            // external by default
extern const int g_limit { 200 };  // const made external

void sayHello() {                  // external by default
    std::cout << "Hello from a.cpp\n";
}

// main.cpp ============================================================
#include <iostream>

extern int g_external;       // forward declaration 
extern const int g_limit;    // forward declaration > < const int g_limit: definition
void sayHello();             // forward declaration

int main() {
    sayHello();
    std::cout << g_external << " / " << g_limit << '\n';
    return 0;
}
```

### 11.3. Inline functions and variables
> When a call to min() is encountered, the CPU must store the address of the current instruction it is executing (so it knows where to return to later) along with the values of various CPU registers (so they can be restored upon returning). Then parameters x and y must be instantiated and then initialized. Then the execution path has to jump to the code in the min() function. When the function ends, the program has to jump back to the location of the function call, and the return value has to be copied so it can be output. This has to be done for each function call.
All of the extra work that must happen to setup, facilitate, and/or cleanup after some task (in this case, making a function call) is called overhead.
#include <iostream>
int min(int x, int y)
{
    return (x < y) ? x : y;
}
int main()
{
    std::cout << min(5, 6) << '\n';
    std::cout << min(3, 2) << '\n';
    return 0;
}
For functions that are large and/or perform complex tasks, the overhead of the function call is typically insignificant compared to the amount of time the function takes to run. However, for small functions (such as min() above), the overhead costs can be larger than the time needed to actually execute the function’s code! In cases where a small function is called often, using a function can result in a significant performance penalty over writing the same code in-place.
However, inline expansion has its own potential cost: if the body of the function being expanded takes more instructions than the function call being replaced, then each inline expansion will cause the executable to grow larger.

- `inline-expansion`:
  - is a process where a function call is replaced by the code from the called function’s definition.
  - use this to avoid such overhead cost.
  - do not use the `inline` keyword to request inline expansion for your functions, because optimizing compilers.
- `modernly-inline`: 
  - should not implement functions (with external linkage) in header files because it lead to `multiple definitions` error.
  - so we can use `inline-function`, it's useful for header-only libraries

### 11.4. Sharing global constants
- 1. `global constants as internal variables`: 
  - Advantages:
    - Works prior to C++16.
    - Can be used in constant expressions in any translation unit that includes them.
  - Downsides:
    - Changing anything in the header file requires recompiling files including the header.
    - Each translation unit including the header gets its own copy of the variable.
- e.g.
```cpp
// constants.h:============================================================
#ifndef CONSTANTS_H
#define CONSTANTS_H

// Define your own namespace to hold constants
namespace constants
{
    // Global constants have internal linkage by default
    constexpr double pi { 3.14159 };
    constexpr double avogadro { 6.0221413e23 };
    constexpr double myGravity { 9.2 }; // m/s^2 -- gravity is light on this planet
    // ... other related constants
}
#endif

// main.cpp::============================================================
#include "constants.h" // include a copy of each constant in this file
#include <iostream>

int main()
{
    std::cout << "Enter a radius: ";
    double radius{};
    std::cin >> radius;

    std::cout << "The circumference is: " << 2 * radius * constants::pi << '\n';

    return 0;
}
```

<br>

- 2. `global constants as external variables`: 
  - Advantages:
    - Works prior to C++16.
    - Only one copy of each variable is required.
    - Only requires recompilation of one file if the value of a constant changes.
  - Downsides:
    - Forward declarations and variable definitions are in separate files, and must be kept in sync.
    - Variables not usable in constant expressions outside of the file in which they are defined.
- e.g.
```cpp
// constants.h:============================================================
#ifndef CONSTANTS_H
#define CONSTANTS_H

namespace constants
{
    // Since the actual variables are inside a namespace, the forward declarations need to be inside a namespace as well
    // We can't forward declare variables as constexpr, but we can forward declare them as (runtime) const
    extern const double pi;
    extern const double avogadro;
    extern const double myGravity;
}

#endif

// constants.cpp:============================================================
#include "constants.h"

namespace constants
{
    // We use extern to ensure these have external linkage
    extern constexpr double pi { 3.14159 };
    extern constexpr double avogadro { 6.0221413e23 };
    extern constexpr double myGravity { 9.2 }; // m/s^2 -- gravity is light on this planet
}

// main.cpp::============================================================
#include "constants.h" // include all the forward declarations
#include <iostream>

int main()
{
    std::cout << "Enter a radius: ";
    double radius{};
    std::cin >> radius;

    std::cout << "The circumference is: " << 2 * radius * constants::pi << '\n';

    return 0;
}
```
- 3. `global constants as inline variables`: 
  - If you need global constants and your compiler is C++17 capable, prefer defining inline constexpr global variables in a header file.
  - Advantages:
    - Can be used in constant expressions in any translation unit that includes them.
    - Only one copy of each variable is required.
  - Downsides:
    - Only works in C++17 onward.
    - Changing anything in the header file requires recompiling files including the header.
- e.g.
```cpp
// constants.h:============================================================
#ifndef CONSTANTS_H
#define CONSTANTS_H

// define your own namespace to hold constants
namespace constants
{
    inline constexpr double pi { 3.14159 }; // note: now inline constexpr
    inline constexpr double avogadro { 6.0221413e23 };
    inline constexpr double myGravity { 9.2 }; // m/s^2 -- gravity is light on this planet
    // ... other related constants
}
#endif

// main.cpp::============================================================
#include "constants.h"
#include <iostream>

int main()
{
    std::cout << "Enter a radius: ";
    double radius{};
    std::cin >> radius;

    std::cout << "The circumference is: " << 2 * radius * constants::pi << '\n';

    return 0;
}
```

## 12. Control Flow
![image](/images/learncpp_2.png)
### 12.1. Constexpr if statement
- C++17, that requires the conditional to be a constant expression. It means the condition will be evaluated at runtime.
- Favor constexpr if statements over non-constexpr if statements when the conditional is a constant expression.
- e.g. 
```cpp
#include <iostream>
int main()
{
	constexpr double gravity{ 9.8 };

	if constexpr (gravity == 9.8) // now using constexpr if
		std::cout << "Gravity is normal.\n";
	else
		std::cout << "We are not on Earth.\n";

	return 0;
}
```
### 12.2. Switch fallthrough and scoping
- The `[[fallthrough]]` attribute modifies a null statement to indicate that fallthrough is intentional (and no warnings should be triggered).
- Initialization is not allowed before case labels because control flow in a switch may jump over the initializer, leaving the variable uninitialized.
- Declarations without an initializer are allowed before case labels because they only reserve space for the variable in the function’s stack frame (decided at compile time). No runtime initialization code is generated, so nothing can be “skipped” by jumping to a case.
- e.g.
```cpp
#include <iostream>

int main() {
    int x = 2;

    switch (x) {
        int a;       //  allowed (no initializer, just reserves space)
        // int b{5}; //  not allowed (initializer may be skipped)

    case 1:
        a = 10; // safe: 'a' exists, we assign here
        std::cout << "Case 1, a = " << a << '\n';
        [[fallthrough]]; //  intentional fallthrough to case 2

    case 2:
        a = 20; // reassign
        std::cout << "Case 2, a = " << a << '\n';
        break;

    default:
        std::cout << "Default case\n";
        break;
    }

    return 0;
}
```
### 12.3. Halts
- Halts allow us to terminate our program.Only use a halt if there is no safe or reasonable way to return normally from the main function. If you haven’t disabled exceptions, prefer using exceptions for handling errors safely.
- `std::exit` is called implicitly when main() returns, it does not clean up local variables in the current function or up the call stack.
- `std::abort()` function causes your program to terminate abnormally. Abnormal termination means the program had some kind of unusual runtime error and the program couldn’t continue to run. 
- `std::terminate()` function is typically used in conjunction with exceptions . By default, it calls `std::abort()`
- e.g.
```cpp
#include <iostream>
#include <cstdlib>    // for std::exit, std::abort
#include <exception>  // for std::terminate

void cleanup() {
    std::cout << "Cleaning up...\n";
}

void riskyFunction(bool fatalError) {
    if (fatalError) {
        std::cout << "Fatal error occurred!\n";

        // std::abort: abnormal termination, no cleanup
        std::abort();

        // Or: std::terminate(); // usually called when exception handling fails
    }
}

int main() {
    cleanup(); // local function call

    // Example 1: return normally from main
    std::cout << "Program running normally...\n";

    // Example 2: using std::exit (implicit when main returns)
    if (false) {
        std::cout << "Exiting via std::exit...\n";
        std::exit(0); // does not call destructors of locals in main()
    }

    // Example 3: risky code that might abort/terminate
    riskyFunction(true);

    // Example 4: normal end of main calls std::exit implicitly
    std::cout << "Main returns normally.\n";
    return 0; // std::exit(0) is called implicitly here
}
```

## 13. Error Detection and Handling

## 14. Type Conversion, Type Aliases, and Type Deduction
```xml
Type conversions
│
├── Implicit conversions (compiler tự làm)
│   ├── Numeric promotions             ← an toàn, không mất dữ liệu
│   │    ├── char → int
│   │    └── float → double
│   │
│   └── Numeric conversions            ← có thể mất dữ liệu
│        ├── Widening conversions      ← mở rộng, thường an toàn (int → double)
│        └── Narrowing conversions     ← thu hẹp, có thể mất dữ liệu (double → int, int → char)
│
└── Explicit conversions (do lập trình viên ép kiểu)
     ├── static_cast<int>(3.14)
     ├── reinterpret_cast
     ├── const_cast
     └── (int)3.14   // C-style cast
```
### 14.1. Implicit type conversion
- **Implicit type conversion** is performed automatically by the compiler when an expression of some type is supplied in a context where some other type is expected.
-  **numeric promotion**  is the conversion of certain smaller numeric types to certain larger numeric types (typically int or double).no data loss.
   -  `floating point promotion`:  a value of type float can be converted to a value of type double.
   -  `integral promotions`:
      -  signed char or signed short can be converted to int.
      -  unsigned char, char8_t, and unsigned short can be converted to int if int can hold the entire range of the type, or unsigned int otherwise.
      -  If char is signed by default, it follows the signed char conversion rules above. If it is unsigned by default, it follows the unsigned char conversion rules above.
      -  bool can be converted to int, with false becoming 0 and true becoming 1.
- **numeric conversion** is a type conversion between fundamental types that isn’t a numeric promotion. A narrowing conversion is a numeric conversion that may result in the loss of value or precision.
  - :( https://www.learncpp.com/cpp-tutorial/narrowing-conversions-list-initialization-and-constexpr-initializers/)

### 14.2. Explicit type conversion
- C++ supports 5 different types of casts: `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`, and `C-style` casts
- 1. `C-style` casts
  - Using operator`(<type>)value`. and the value to convert to placed immediately to the right of the closing parenthesis `)`. e.g. `(int)7/3`
- 2. `static_cast` casts
  - Using `static_cast<type>(value)
  - provides compile-time type checking
  - less powerful than a C-style cast
  - direct-initialized vs list-initialization
### 14.3. Typedefs and type aliases
- `type aliases`: use the `using` keyworld
  - `using <NewtypeAlias> = <type>`
- `typedefs`: is an old way of creating an alias for a type. Using `typedef` keyworld.
  - `typedef <type> <NewtypeAlias> ` 
- Prefer `type alias` over `typedef`
```cpp
using MyDouble = double;
typedef double MyDouble;

typedef int (*FcnType)(double, char); // FcnType hard to find
using FcnType = int(*)(double, char); // FcnType easier to find
```
### 14.5. Type deduction using auto keyworld
- Type deduction allows the compiler to deduce the type of an object from the object’s initializer. 
- using `auto` keyworld.
- Type deduction must have something to deduce from
- Type deduction drops const from the deduced type
- ... (more)
- Use type deduction for your variables when the type of the object doesn’t matter.
- Auto can also be used as a function return type to have the compiler infer the function’s return type from the function’s return statements, though this should be avoided for normal functions.
- The auto keyword can also be used to declare functions using a trailing return syntax, where the return type is specified after the rest of the function prototype.
- e.g.
```cpp
int add(int x, int y)
{
  return (x + y);
}

// Using the trailing return syntax, this could be equivalently written as:
auto add(int x, int y) -> int
{
  return (x + y);
}

#include <type_traits> // for std::common_type

std::common_type_t<int, double> compare(int, double);         // harder to read (where is the name of the function in this mess?)
auto compare(int, double) -> std::common_type_t<int, double>; // easier to read (we don't have to read the return type unless we care)
```

<br>


## 15. Function Overloading and Function Templates
### 15.1. Function overloading
- `function overloading` allows us to create multiple functions with the same name, so long as each identically named function has different parameter types/numbers.  Return types are not considered for 
- `function delete` using `delete` keywork. delete means “I forbid this”, not “this doesn’t exist”. 
- e.g.
```cpp
#include <iostream>

// This function will take precedence for arguments of type int
void printInt(int x)
{
    std::cout << x << '\n';
}

// This function template will take precedence for arguments of other types
// Since this function template is deleted, calls to it will halt compilation
template <typename T>
void printInt(T x) = delete;

int main()
{
    printInt(97);   // okay
    printInt('a');  // compile error
    printInt(true); // compile error

    return 0;
}
```
- `default-arguments`: is a default value provided for a function parameter. Parameters with default arguments must always be the rightmost parameters, and they are not used to differentiate functions when resolving overloaded functions.

### 15.2. Function Templates
- The template system was designed to simplify the process of creating functions (or classes) that are able to work with different data types (that are compiled and executed).
- `template types` are sometimes called generic types, and programming using templates is sometimes called generic programming.
- `placeholder types` use for any parameter types, return types, or types used in the function body that we want to be specified later, by the user of the template.
- `template parameter declaration` defines any template parameters that will be subsequently used.
- `function templates` allow us to create a function-like definition that serves as a pattern for creating related functions. In a function template, we use type template parameters as placeholders for any types we want to be specified later. The syntax that tells the compiler we’re defining a template and declares the template types is called a template parameter declaration.
- Using function templates in multiple files. should be defined in a header file, and then #included wherever needed. 
- `template argument deduction` to have the compiler deduce the actual type that should be used from the argument types in the function call.
- e.g. 
```cpp
template <typename T> // this is the template parameter declaration defining T as a type template parameter , `typename` or `class` can be used
T max(T x, T y) // this is the function template definition for max<T>
{
    return (x < y) ? y : x;
}

template<>
int max<int>(int x, int y) // the generated function max<int>(int, int)
{
    return (x < y) ? y : x;
}

template<>
double max<double>(double x, double y) // the generated function max<double>(double, double)
{
    return (x < y) ? y : x;
}

int main()
{
    std::cout << max<int>(1, 2) << '\n'; // calls max<int>(int, int)
    std::cout << max<>(1, 2) << '\n';    // deduces max<int>(int, int) (non-template functions not considered)
    std::cout << max(1, 2) << '\n';      // calls max(int, int)

    return 0;
}
```

- `function templates with multiple template` types example:
```cpp
#include <iostream>

template <typename T, typename U>
auto max(T x, U y) // ask compiler can figure out what the relevant return type is
{
    return (x < y) ? y : x;
}

int main()
{
    std::cout << max(2, 3.5) << '\n';

    return 0;
}
```

- `non-type template parameter` is a template parameter with a fixed type that serves as a placeholder for a constexpr value passed in as a template argument.
```cpp
#include <iostream>

template <int N> // int non-type template parameter
void print()
{
    std::cout << N << '\n';
}

int main()
{
    print<5>();   // no conversion necessary
    print<'c'>(); // 'c' converted to type int, prints 99

    return 0;
}
```

<br>

## 16. Compound Types: References and Pointers

| Type            | Meaning                                                                                                           | Examples                              |
|-----------------|-------------------------------------------------------------------------------------------------------------------|---------------------------------------|
| **Fundamental** | A basic type built into the core C++ language                                                                    | `int`, `std::nullptr_t`               |
| **Compound**    | A type defined in terms of other types                                                                            | `int&`, `double*`, `std::string`, `Fraction` |
| **User-defined** | A class type or enumerated type <br> (Includes those defined in the standard library or implementation) <br> (In casual use, typically used to mean program-defined types) | `std::string`, `Fraction`             |
| **Program-defined** | A class type or enumerated type <br> (Excludes those defined in standard library or implementation)            | —                                     |

- `Compound data types` (also called composite data type) are data types that can be constructed from fundamental data types (or other compound data types).
### 16.1. lvalues and rvalues
- `lvalues` is an expression that evaluates to an identifiable object or function (or bit-field). Can be accessed via an identifier, reference, or pointer, and typically have a lifetime longer than a single `expression` or `statement`.
- `rvalues` is an expression that evaluate to a value. Only exist within the scope of the expression in which they are used.
- `lvalues` can be used anywhere an `rvalue` is expected.
- An **assignment operation** requires its `left` operand to be a modifiable `lvalue` expression. And its `right` operand to be a `rvalue` expression.

### 16.2. References
- **references** is an alias for an existing object/function. `reference` itself is like a `const pointer`
  - Declared as `<type>& reference_name`
  - Any operation on the reference is applied to the object being referenced.
  - All references must be initialized.
  - Cannot be reseated.
  - They aren't objects
  - Can only accept modifiable lvalue arguments (const or non-const)

- **pass-by-reference** allows us:
  - to pass arguments to a function without making copies of those arguments each time the function is called. (class types)
  - to change the value of an argument

- **pass-by-const-reference** guaranteeing that the function can not change the value being referenced.
- **lvalue-reference** just a reference for an existing lvalue.
- **lvalue-reference-types** determines what type of object it can reference by using a single ampersand  `<type>&` .
- **lvalue-reference-variable** is a variable that acts as a reference to an lvalue.
- **lvalue-reference-to-const** can bind with const or non-const objects. `const <type>& name`

>The const applies to what is immediately to its left, unless there’s nothing to its left, in which case it applies to what’s on the right.

- E.g.
```cpp
#include <iostream>
#include <string>

// ---------------------------
// Example of references
// ---------------------------
void increment(int& x) {   // pass-by-reference (modifiable lvalue reference)
    x += 1;                // changes original argument
}

void printConstRef(const std::string& s) { // pass-by-const-reference
    // s cannot be modified here
    std::cout << "Const-ref: " << s << "\n";
}

int main() {
    int a = 10;
    
    // ---- Reference basics ----
    int& ref = a;       // reference must be initialized
    ref = 20;           // modifies 'a', since ref is just an alias
    std::cout << "a = " << a << "\n";  // prints 20
    
    // Cannot reseat: once 'ref' is bound to 'a', it cannot be bound to another variable
    int b = 30;
    // ref = &b;   invalid, would assign value instead of rebinding
    
    // ---- Pass by reference ----
    increment(a);  // modifies original 'a'
    std::cout << "a after increment = " << a << "\n"; // prints 21

    // ---- Pass by const reference ----
    std::string text = "Hello";
    printConstRef(text);  // avoids making a copy

    // ---- Lvalue reference variable ----
    int& lref = a;  // lref is an lvalue-reference-variable to 'a'
    lref += 5;      // modifies 'a'
    std::cout << "a after lref += 5: " << a << "\n";

    // ---- Lvalue reference to const ----
    const int x = 100;
    const int& cref1 = x; // bind to const object
    const int& cref2 = a; // also works with non-const object
    std::cout << "cref1 = " << cref1 << ", cref2 = " << cref2 << "\n";
    
    // ---- They aren't objects ----
    // sizeof(ref) == sizeof(a), because ref is just an alias
    std::cout << "sizeof(a) == sizeof(ref): "
              << (sizeof(a) == sizeof(ref)) << "\n";

    return 0;
}
```

### 16.3. Pointer
- **address-of-operator (&<variable>)** returns the memory address of its operand, **but not as an address literal. Instead, it returns a pointer to the operand.** This pointer holds the address value, and when passed to cout, the stream simply prints that value.
- **dereference-operator  (*<address>)** returns the value at a given memory address as an lvalue, used to access the object being pointed at.
- **pointer** is an object that holds a memory address as its value:
  - declared as `<type>* ptr_name`
  -  This allows us to store the address of some other object to use later.
  -  we should init the pointers.
  -  the size of pointer is allways the same (32 or 64-bit architecture)
  -  can assign an invalid pointer a new value, such as `nullptr`
  -  `wild pointer`: pointer that has not been initialized is sometimes called a .
  -   `dangling pointer`: pointer that is holding the address of an object that is no longer valid
- **pointer-type** is a type that specifies a pointer (like reference-type) by using an asterisk `(<type>*)`.The type of the pointer has to match the type of the object being pointed at.
- **null-pointer** (its type is `std::nullptr_t`) means something has no value. It often associated with memory address 0. 

- **pointer-to-const**: that points to a value that cannot be modified through the pointer, but the pointer itself is not const.
  - declared as `const <type> ptr_name*`.
  - cannot change the value being pointed to, but can make the pointer point to a different address.
  - may also point to non-const variables.
- **const-pointer**: whose stored address cannot be changed after initialization, but the value at that address can be modified.
  - declared as `<type>* const ptr_name`.
  - fixed to one address, but we can change the value at that address.
- **const-pointer-to-const**: cannot be reseated (address fixed) and cannot modify the value it points to.
  - declared as `const <type>* const ptr_name`.
  - can only be dereferenced to read the value.
>pointer-to-pointer
void allocateArray(int** ptr, int size) {
    *ptr = new int[size]; // allocate memory and update the original pointer
}
int* myArray = nullptr;
allocateArray(&myArray, 5);
myArray[0] = 10; // works
delete[] myArray;
- e.g.
```cpp
#include <iostream>
#include <cstdint>  // for uintptr_t

int main() {
    int x = 42;

    // address-of operator (&) returns a pointer to x (not an address literal)
    int* ptr = &x;  
    
    // Printing the pointer: cout prints the stored address value
    std::cout << "Address of x (&x): " << &x << "\n";
    std::cout << "Value stored in pointer (ptr): " << ptr << "\n";

    // dereference operator (*) gives access to the value at the stored address
    std::cout << "Value of x via *ptr: " << *ptr << "\n";

    // a pointer is an object that holds a memory address
    // we can change its value
    ptr = nullptr;  
    std::cout << "Pointer reset to nullptr: " << ptr << "\n";

    // pointer type is declared with '*'
    double d = 3.14;
    double* pd = &d;    // type matches: double* for double
    // uintptr_t lets us see the raw numeric value of the address
    std::cout << "Numeric address of d: " << (uintptr_t)pd << "\n";
    std::cout << "Value of d via *pd: " << *pd << "\n";
    
    // NullPTR =======
    int* myNullPtr {};        // value-initialized to nullptr
    int* myNullPtr2 {nullptr}; // explicitly initialized to nullptr

    // Old C-style (still works, but less safe in C++):
    // int* myNullPtr3 {NULL};   // requires <cstddef>
    
    // Const ptr =======
    int a = 10;
    int b = 20;
    // 1. pointer-to-const (const int*)
    const int* p1 = &a;      // can point to non-const variable
    // *p1 = 15;             //  error: cannot modify value through p1
    p1 = &b;                 //  can point to another address
    std::cout << "p1 points to: " << *p1 << '\n';
    // 2. const-pointer (int* const)
    int* const p2 = &a;      // must be initialized, fixed address
    *p2 = 30;                //  can modify the value at that address
    // p2 = &b;              //  error: cannot change stored address
    std::cout << "p2 points to: " << *p2 << '\n';
    // 3. const-pointer-to-const (const int* const)
    const int* const p3 = &b; // fixed address + read-only value
    // *p3 = 40;             //  error: cannot modify value
    // p3 = &a;              //  error: cannot reseat pointer
    std::cout << "p3 points to: " << *p3 << '\n';
       

    return 0;
}
```

### 16.4. Pass by value/reference/address

```cpp
#include <iostream>
#include <string>

void printByValue(std::string val) // The function parameter is a copy of str
{
    std::cout << val << '\n'; // print the value via the copy
}

void printByReference(const std::string& ref) // The function parameter is a reference that binds to str
{
    std::cout << ref << '\n'; // print the value via the reference
}

void printByAddress(const std::string* ptr) // The function parameter is a pointer that holds the address of str
{
    std::cout << *ptr << '\n'; // print the value via the dereferenced pointer
}

int main()
{
    std::string str{ "Hello, world!" };

    printByValue(str); // pass str by value, makes a copy of str
    printByReference(str); // pass str by reference, does not make a copy of str
    printByAddress(&str); // pass str by address, does not make a copy of str

    return 0;
}
```
- **pass-by-address** allows us: ~ **pass-by-references**
  - to pass arguments to a function without making copies of those arguments each time the function is called. (class types)
  - to change the value of an argument
  - #null checking
> Pass by reference when you can, pass by address when you must

- **!! C++ really passes everything by value**

### 16.5. Return by value/reference/address
- **T returnValue(...)**: returns a copy (or move) of the object. The caller gets its own value.
- **T& returnReference(...)** returns a reference to an existing object. The caller does not own it, so the object must outlive the reference.
- **T * returnAddress(...)** returns a pointer (an address) to an object. The caller must handle the pointer carefully (ensure it’s valid and points to a live object).

- `return-by-reference`:
  - avoids making a copy of the object.
  - the referenced object must live beyond the scope of the function, otherwise the reference will dangle.
  - never return a non-static local variable or temporary by reference.
- `return-by-address` works almost identically to return-by-reference.
- `return-by-value` just make a copy
> Prefer return by reference over return by address unless the ability to return “no object” (using nullptr) is important.

- e.g.
```cpp
#include <iostream>

int global = 42;

// Return by value: makes a copy
int returnValue() {
    int x = 10;
    return x; // copy returned
}

// Return by reference: must refer to existing object
int& returnReference() {
    return global; // safe: global outlives the function
}

// Return by address: returns a pointer
int* returnAddress(bool valid) {
    if (valid)
        return &global; // valid pointer
    else
        return nullptr; // no object
}

int main() {
    int a = returnValue();
    std::cout << "By value: " << a << '\n';

    int& b = returnReference();
    std::cout << "By reference: " << b << '\n';
    b = 100; // modifies global
    std::cout << "Global after modification: " << global << '\n';

    int* c = returnAddress(true);
    if (c) std::cout << "By address: " << *c << '\n';

    int* d = returnAddress(false);
    if (!d) std::cout << "By address: got nullptr\n";
}
```

### 16.6. In/Out Params
- `in-parameters`:are typically passed `by value` or `by const reference`
- `out-parameters`:a function parameter that is used only for the purpose of returning information back to the caller.
  - Avoid out-parameters (except in the rare case where no better options exist).
  - Prefer pass by reference for non-optional out-parameters.

- e.g.
```cpp
#include <iostream>
#include <string>

// In-parameter by value (cheap to copy)
void greet(std::string name) {
    std::cout << "Hello, " << name << "!\n";
}

// In-parameter by const reference (avoid copy for large objects)
int length(const std::string& text) {
    return text.size();
}

// Out-parameter by reference (rare case)
void square(int input, int& output) {
    output = input * input;
}

int main() {
    // in-parameter by value
    greet("Alice");

    // in-parameter by const reference
    std::string msg = "Hello World";
    std::cout << "Length = " << length(msg) << "\n";

    // out-parameter by reference (not preferred, but possible)
    int result;
    square(5, result);
    std::cout << "Square = " << result << "\n";
}
```

### 16.7. Type deduction (auto) with pointers, references, and const
https://www.learncpp.com/cpp-tutorial/type-deduction-with-pointers-references-and-const/

### 16.8. std::optional
https://www.learncpp.com/cpp-tutorial/stdoptional/

## 18. Compound Types: Enums and Structs
- `program-defined-types` are types that programmers create themself.
> In C++, struct, class, and union automatically create a new type name, so you don’t need to prefix variables with the keywords struct or union as in C.
### 18.1. Enumerations
- `enum` is a compound types where every possible value is defined as a symbolic constant.
- Named starting with a capital letter. Named enumerators starting with a lower case letter.
- `unscoped-enum`: put their enumerator names into the same scope as the enumeration definition itself 
- `scoped-enum`: keep their enumerators inside the enum’s own scope.Using `enum class` keyworld.
- `using enum <EnumName>` statement imports all the emnumerators from an enum into the current scope. 
- putting your enumerations inside a named scope region (such as a namespace or class) so the enumerators don’t pollute the global namespace.
- Specify the base type of an enumeration only when necessary.
- e.g.
```cpp
#include <iostream>
#include <cstdint>   // for uint8_t

//  Good naming style:
// Enum name: Capitalized
// Enumerator names: lowercase

enum Color {
    red,
    green,
    blue
};

//  Scoped enum — enumerators are inside the enum’s scope
enum class Shape {
    circle,
    square,
    triangle
};

//  Scoped enum inside a namespace — prevents name pollution
namespace Game {
    enum class Direction {
        up,
        down,
        left,
        right
    };
}

//  Scoped enum with explicit base type
enum class Status : uint8_t {
    ok = 0,
    error = 1,
    unknown = 2
};

int main() {
    // Using unscoped enum
    Color c = red;               //  direct access (same scope)
    std::cout << "Color value: " << c << "\n";

    // Using scoped enum
    Shape s = Shape::circle;     //  must use scope name
    if (s == Shape::circle)
        std::cout << "Shape is circle\n";

    // Scoped enum inside namespace
    Game::Direction dir = Game::Direction::up;
    if (dir == Game::Direction::up)
        std::cout << "Direction is up\n";

    // Scoped enum with base type
    Status st = Status::ok;
    if (st == Status::ok)
        std::cout << "Status OK (base type uint8_t)\n";

    return 0;
}
```

- **Union**: https://www.geeksforgeeks.org/cpp/cpp-unions/

### 18.2. Struct
- A **struct** is a *class type* (just like `classes` or `union`),  allows us to bundle multiple variables together into a single type. As such, anything that applies to class types applies to structs.
- **Defining structs** using `struct` keywords.
- **Access struct members**:
  - Use member selection operator(dot operator) `.` for reference/object.
  - Use arrow operator `->` for pointers. `ptr->id = (*ptr).id`
- **Initialization**: 
  - using brace-initialization `{}` or by defining default member values.
  - should provide a default value for all members
  - Struct aggregate initializations:
```cpp
    Employee frank = { 1, 32, 60000.0 }; // copy-list initialization using braced list
    Employee joe { 2, 28, 45000.0 };     // list initialization using braced list (preferred)
``` 
- **Passing and returning structs**:
  - Passingy reference (efficient and avoids copying)
  - Passing temporary 
  - Create a struct variable and return
  - Returning a temporary (unnamed/anonymous) object 
- **Struct size and data structure alignment**:the size of a struct will be at least as large as the size of all the variables it contains. But it could be larger! For performance reasons, the compiler will sometimes add gaps into structures this is called **padding**. We can minimize padding by defining your members in **decreasing order of size**.(e.g., double → int → char).

- e.g.
```cpp
#include <iostream>
#include <string>
using namespace std;

// Define a struct
struct SensorData {
    double voltage{0.0};   // Default initialization
    int id{0};
    char status{'N'};      // 'N' = normal, 'E' = error
    string label{"Unknown"};

    // Member function
    void print() const { // Const class objects and const member functions
        cout << "Sensor " << id 
             << " [" << label << "] "
             << "Voltage: " << voltage 
             << " Status: " << status << endl;
    }
};

// Function that accepts struct by reference
void updateVoltage(SensorData &data, double newV) {
    data.voltage = newV;
}

// Function returning a temporary struct
SensorData makeSensor(int id, double v, const string &label) {
    return {v, id, 'N', label};
}

int main() {
    // Initialization using braces
    SensorData s1{3.3, 1, 'N', "Temperature"};
    s1.print();

    // Pointer access
    SensorData *ptr = &s1;
    ptr->status = 'E';
    ptr->print();

    // Passing by reference
    updateVoltage(s1, 4.8);
    s1.print();

    // Returning temporary struct
    SensorData s2 = makeSensor(2, 5.0, "Pressure");
    s2.print();

    cout << "Size of struct = " << sizeof(SensorData) << " bytes" << endl;
    return 0;
}
```

### 18.3. Class template
- a **class template** is a template definition for instantiating class types (structs, classes, or unions). Class template argument deduction (CTAD) is a C++17 feature that allows the compiler to deduce the template type arguments from an initializer.
- Using class template in a function:
- e.g.
```cpp
#include <iostream>

template <typename T>
struct Pair
{
    T first{};
    T second{};
};

template <typename T>
constexpr T max(Pair<T> p)
{
    return (p.first < p.second ? p.second : p.first);
}

int main()
{
    Pair<int> p1{ 5, 6 };        // instantiates Pair<int> and creates object p1
    std::cout << p1.first << ' ' << p1.second << '\n';

    Pair<double> p2{ 1.2, 3.4 }; // instantiates Pair<double> and creates object p2
    std::cout << p2.first << ' ' << p2.second << '\n';

    Pair<double> p3{ 7.8, 9.0 }; // creates object p3 using prior definition for Pair<double>
    std::cout << p3.first << ' ' << p3.second << '\n';

    std::cout << max<int>(p1) << " is larger\n"; // explicit call to max<int>

    return 0;
}

// Compiler ===================================================================================
#include <iostream>

// A declaration for our Pair class template
// (we don't need the definition any more since it's not used)
template <typename T>
struct Pair;

// Explicitly define what Pair<int> looks like
template <> // tells the compiler this is a template type with no template parameters
struct Pair<int>
{
    int first{};
    int second{};
};

// Explicitly define what Pair<double> looks like
template <> // tells the compiler this is a template type with no template parameters
struct Pair<double>
{
    double first{};
    double second{};
};

int main()
{
    Pair<int> p1{ 5, 6 };        // instantiates Pair<int> and creates object p1
    std::cout << p1.first << ' ' << p1.second << '\n';

    Pair<double> p2{ 1.2, 3.4 }; // instantiates Pair<double> and creates object p2
    std::cout << p2.first << ' ' << p2.second << '\n';

    Pair<double> p3{ 7.8, 9.0 }; // creates object p3 using prior definition for Pair<double>
    std::cout << p3.first << ' ' << p3.second << '\n';

    return 0;
}
```

- **Using class templates in multiple files**:Just like function templates, class templates are typically defined in header files

- **Alias templates**: is a template that can be used to instantiate type aliases.
- e.g.
```cpp
// Alias templates must be defined in global scope
template <typename T>
using Coord = Pair<T>; // Coord is an alias for Pair<T>
```
## 19. Classes
- **Overview:**
```cpp
// ===== File: main.cpp =====
#include <iostream>
#include <string>
#include <utility>
#include <vector>
using namespace std;

// ===== File: Person.h =====
class Person {
private:                       // (14.5) Data hiding and encapsulation
    string name;
    int age;

public:
    // (14.9) Constructor
    Person(string n, int a) : name{std::move(n)}, age{a} {
        cout << "Constructor: " << name << endl;
    }

    // (14.12) Delegating constructor
    Person() : Person("Unknown", 0) {}

    // (14.14) Copy constructor
    Person(const Person& other) : name{other.name}, age{other.age} {
        cout << "Copy constructor called for " << name << endl;
    }

    // (15.4) Destructor
    ~Person() {
        cout << "Destructor: " << name << endl;
    }

    // (14.3) Member function
    void print() const {  // (14.4) const member function
        cout << name << " (" << age << ")\n";
    }

    // (14.6) Access function
    string getName() const { return name; }
    int getAge() const { return age; }

    // (14.7) Return reference to data member
    int& getAgeRef() { return age; }

    // (14.16) Converting constructor (implicit)
    Person(int a) : name{"Anon"}, age{a} {}

    // (14.17) constexpr constructor (simple example)
    constexpr Person(const char* n, int a, bool) : name{n}, age{a} {}

    // (15.1) Function chaining using hidden this pointer
    Person& setName(const string& n) { name = n; return *this; }
    Person& setAge(int a) { age = a; return *this; }

    // (15.7) Static member function
    static void hello() { cout << "Hello from Person class!\n"; }

    // (15.8) Friend non-member function
    friend void showSecret(const Person& p);
};

// ===== File: Person.cpp =====
void showSecret(const Person& p) {   // (15.8) Friend function
    cout << "Friend: " << p.name << " is " << p.age << " years old.\n";
}

// ===== File: Employee.h =====
class Employee : public Person {    // Inheritance for demonstration
private:
    double salary;

public:
    Employee(string n, int a, double s)
        : Person(std::move(n), a), salary{s} {}

    void print() const {            // Override
        cout << "[Employee] ";
        Person::print();
        cout << "Salary: $" << salary << "\n";
    }

    // (15.9) Friend class
    friend class HR;
};

// ===== File: HR.h =====
class HR {
public:
    void adjustSalary(Employee& e, double newSalary) {
        e.salary = newSalary;
        cout << "HR adjusted salary!\n";
    }
};

// ===== File: Counter.h =====
class Counter {
private:
    static int count;   // (15.6) Static member variable

public:
    Counter() { ++count; }
    ~Counter() { --count; }
    static int getCount() { return count; } // (15.7)
};
int Counter::count = 0;

// ===== File: Box.h =====
// (15.5) Class template with member functions
template <typename T>
class Box {
private:
    T value;
public:
    explicit Box(T v) : value{v} {}
    T get() const { return value; }
    void set(T v) { value = v; }
};

// ===== File: main.cpp (continued) =====
int main() {
    cout << "=== OOP Example ===\n";

    // Constructors
    Person p1("Alice", 25);
    Person p2 = p1;              // Copy constructor
    Person p3;                   // Delegating constructor

    p1.print();

    // Function chaining
    p1.setName("Bob").setAge(30).print();

    // Static function
    Person::hello();

    // Friend function
    showSecret(p1);

    // Employee & HR
    Employee e1("Charlie", 35, 50000);
    HR hr;
    e1.print();
    hr.adjustSalary(e1, 60000);
    e1.print();

    // Static member variable usage
    Counter c1, c2;
    cout << "Counter count: " << Counter::getCount() << endl;

    // Template class
    Box<int> intBox(123);
    cout << "Box value: " << intBox.get() << endl;

    // (14.16) Implicit converting constructor
    Person p4 = 42;  // age=42, name="Anon"
    p4.print();

    // (14.17) constexpr example
    constexpr Person p5("ConstExpr", 10, true);
    p5.print();

    cout << "=== End ===\n";
    return 0;
}
```

### 19.1. Introduce
- A class is a program-defined compound type that can have many member variables with **different types** (this point is different from structure).
- A class is a program-defined compound type that bundles both data and functions that work on that data.

- Examples:
```cpp
#include <iostream>
class MyClass{
private:
     int variable1;
     int variable2;
public:
    MyClass(int v1, int v2){
        variable1 = v1;
        variable2 = v2;
    }

    void printVariables(){
        std::cout << variable1 << "-" <<variable2;
    }
};

int main()
{
    std::cout<<"Hello Classes";
    MyClass myObject = MyClass(3,4);
    myObject.printVariables();
    return 0;
}
```

### 19.2. Member Variables/ Functions:
- The variable/functions that belong to a class type are called member variables/ functions.
- In C, structs only have data members, not member functions.

### 19.3. Const class objects and const member functions
- Const class objects: just like with normal variables, we can make our class type objects const or constexpr.
- Const member function: is a member function that guarentees it will not modify the object or call any non-constant member functions.
	- Syntax `<returnType> <nameFunction> const(<params>){}`

### 19.4. Temporary class objects
-  A temporary object (sometimes called an `anonymous object` or an `unnamed object`) is an object that has no name and exists only for the duration of a single expression.
- e.g.
```cpp
#include <iostream>

class IntPair
{
private:
    int m_x{};
    int m_y{};

public:
    IntPair(int x, int y)
        : m_x { x }, m_y { y }
    {}

    int x() const { return m_x; }
    int y() const{ return m_y; }
};

void print(IntPair p)
{
    std::cout << "(" << p.x() << ", " << p.y() << ")\n";
}

int main()
{
    // Case 1: Pass variable
    IntPair p { 3, 4 };
    print(p);

    // Case 2: Construct temporary IntPair and pass to function
	// When the function call returns, the temporary object is destroyed.
    print(IntPair { 5, 6 } );

    // Case 3: Implicitly convert { 7, 8 } to a temporary Intpair and pass to function
    print( { 7, 8 } );

    return 0;
}
```

### 19.5. Public and private members and access specifiers
- By default, all members of a `struct` are public members.
- By default, the members of a class are `private`.

### 19.6. Access functions
TODO:
### 19.7.  Member functions returning references to data members
TODO:
### 19.8. Encapsulation - The benefits of data hiding
TODO:
### 19.9. Constructors
- We are normally using the aggregate to initialize the class type(Aggregate initialization means the use of brace-enclosed initializer lists to initialize all members of an aggregate (ie an array or struct)), but it does not work as soon as we make member variables private.
- A constructor is a special member function that is used to initialize class type objects. That is automatically called after a non-aggregate class type object is created.
	- perform initialization of any member variables (via a member initialization list)
    - perform other setup functions (via statements in the body of the constructor). This might include things such as error checking the initialization values, opening a file or database, etc…
- **Syntax**:
	- Constructors must have the same name as the class (with the same capitalization).
	- For template classes, this name excludes the template parameters.
    - Constructors have no return type (not even void).

### 19.10. Constructor - member initializer lists
- The **member initializer list** is defined after the constructor parameters. It begins with a colon (`:`), and then lists each member to initialize along with the initialization value for that variable, separated by a comma (`,`). We must use a direct form of initialization here (preferably using braces(`{}`), but parentheses(`()`) works as well) .Using copy initialization (with an equals(`=`)) does not work here.

- e.g.:
```cpp
Foo(int x, int y) : m_x { x }, m_y { y }
{
	// m_x  = x; this is an assignment
}
```
- Members in the member initializer list should be listed in the order in which they are defined in the class
- Prefer using the member initializer list to initialize your members over assigning values because in case where members are required to be initialized (such as for data members that are const or references) assignment will not work.

### 19.11. Constructor - Default and default arguments
- A **default constructor** is a constructor that accepts no arguments.
- Because constructors are functions, we can:
	- Constructors with default arguments
	- Overloaded constructors
- **An implicit default constructor** is generated by the compiler when the class has *no user-declared constructors*. This constructor has nothing.
- **An explicitly default constructor** is used in case we already create the constructor ourselves, but also want the compiler to generate the default constructor. Using keywork `default`
- e.g.
```cpp
class Foo
{
private:
    int m_x {};
    int m_y {};

public:
    Foo() = default; // generates an explicitly defaulted default constructor

    Foo(int x, int y)
        : m_x { x }, m_y { y }
    {
        std::cout << "Foo(" << m_x << ", " << m_y << ") constructed\n";
    }
};

int main()
{
    Foo foo{}; // calls Foo() default constructor
		
    return 0;
}
```

### 19.12. Constructor - Delegating
- **Delegating constructors** allow to delegate (transfer responsibility for) initialization to another constructor from the same class type.
- Simply *call the constructor in the member initializer list*
- Use of the `static` keyword for the const variables member allows us to have a single  member that is shared by all class objects.
- e.g.
```cpp
#include <iostream>
#include <string>
#include <string_view>

class Employee
{
private:
    std::string m_name { "???" };
    int m_id { 0 };

public:
    Employee(std::string_view name)
        : Employee{ name, 0 } // delegate initialization to Employee(std::string_view, int) constructor
    {
    }

    Employee(std::string_view name, int id)
        : m_name{ name }, m_id { id } // actually initializes the members
    {
        std::cout << "Employee " << m_name << " created\n";
    }

};

int main()
{
    Employee e1{ "James" };
    Employee e2{ "Dave", 42 };
}
```

### 19.13. Constructor - Copy
- **A copy constructor** is a constructor that is used to initialize an object with an existing object of the same type. After the copy constructor executes, the newly created object should be a copy of the object passed in as the initializer.
- **An implicit copy constructor**: C++ will create a public implicit copy constructor for us if we do not provide a one.
- **An explicitly copy constructor**: by explicitly define our own copy constructor
- e.g.
```cpp
    // Copy constructor
    Fraction(const Fraction& fraction)
        // Initialize our members using the corresponding member of the parameter
        : m_numerator{ fraction.m_numerator }
        , m_denominator{ fraction.m_denominator }
    {
        // do other things
    }

```
- Using `= default` to generate a default copy constructor.
- Using `= delete` to prevent copies.
```cpp
    // Explicitly request default copy constructor
    Fraction(const Fraction& fraction) = default;
    Fraction fCopy { f };
	
    // Delete the copy constructor so no copies can be made
    Fraction(const Fraction& fraction) = delete;
	Fraction f { 5, 3 };
	Fraction fCopy { f }; // compile error: copy constructor has been deleted

```
>The rule of three is a well known C++ principle that states that if a class requires a user-defined copy constructor, destructor, or copy assignment operator, then it probably requires all three. In C++11, this was expanded to the rule of five, which adds the move constructor and move assignment operator to the list.
Not following the rule of three/rule of five is likely to lead to malfunctioning code. We’ll revisit the rule of three and rule of five when we cover dynamic memory allocation


### 19.14. Class initialization and copy elision
- **For variables**:
```cpp
int a;         // no initializer (default initialization)
int b = 5;     // initializer after equals sign (copy initialization)
int c( 6 );    // initializer in parentheses (direct initialization)

// List initialization methods (C++11)
int d { 7 };   // initializer in braces (direct list initialization)
int e = { 8 }; // initializer in braces after equals sign (copy list initialization)
int f {};      // initializer is empty braces (value initialization)
```

- **For object with class types**:
```cpp
#include <iostream>

class Foo
{
public:

    // Default constructor
    Foo()
    {
        std::cout << "Foo()\n";
    }

    // Normal constructor
    Foo(int x)
    {
        std::cout << "Foo(int) " << x << '\n';
    }

    // Copy constructor
    Foo(const Foo&)
    {
        std::cout << "Foo(const Foo&)\n";
    }
};

int main()
{
    // Calls Foo() default constructor
    Foo f1;           // default initialization
    Foo f2{};         // value initialization (preferred)

    // Calls foo(int) normal constructor
    Foo f3 = 3;       // copy initialization (non-explicit constructors only)
    Foo f4(4);        // direct initialization
    Foo f5{ 5 };      // direct list initialization (preferred)
    Foo f6 = { 6 };   // copy list initialization (non-explicit constructors only)

    // Calls foo(const Foo&) copy constructor
    Foo f7 = f3;      // copy initialization
    Foo f8(f3);       // direct initialization
    Foo f9{ f3 };     // direct list initialization (preferred)
    Foo f10 = { f3 }; // copy list initialization

    return 0;
}
```
- For all types of initialization:
  - When initializing a class type, the set of constructors for that class are examined, and overload resolution is used to determine the best matching constructor. This may involve implicit conversion of arguments.
  - When initializing a non-class type, the implicit conversion rules are used to determine whether an implicit conversion exists.
  - List initialization disallows narrowing conversions.
  - Copy initialization only considers non-explicit constructors/conversion functions.
  - List initialization prioritizes matching list constructors over other matching constructors. 
	
### 19.15. Converting constructors and the explicit keyword
- For example:
```cpp
#include <iostream>

class Foo
{
private:
    int m_x{};
public:
    Foo(int x)
        : m_x{ x }
    {
    }

    int getX() const { return m_x; }
};

void printFoo(Foo f) // has a Foo parameter
{
    std::cout << f.getX();
}

int main()
{
    printFoo(5); // we're supplying an int argument

    return 0;
}
```
- The compiler will look to see if there is any constructor that it can be use to perform (5) -> Foo.
- A converting constructor is a constructor that can be used to perform an implicit conversion is called a converting constructor. By default, all constructors are converting constructors.
- Only one user-defined conversion may be applied.
  
  <br>

- **The `explicit` keyword** is used to to tell the compiler that a constructor should not be used as a converting constructor.
	- For constructors with a separate declaration (inside the class) and definition (outside the class), the explicit keyword is used only on the declaration.
	- Explicit constructors can be used for direct and direct list initialization
	- Prefer use this key work for constructors that take a single argument. 
- e.g.
```cpp
#include <iostream>

class Dollars
{
private:
    int m_dollars{};

public:
    explicit Dollars(int d) // now explicit
        : m_dollars{ d }
    {
    }

    int getDollars() const { return m_dollars; }
};

void print(Dollars d)
{
    std::cout << "$" << d.getDollars();
}

int main()
{
    print(5); // compilation error because Dollars(int) is explicit
	Dollars d1(5); // ok
    Dollars d2{5}; // ok
    return 0;
}
```

- **Return by value and explicit constructors**: when we return a value from a function, if that value does not match the return type of the function, an implicit conversion will occur. Just like with pass by value, such conversions cannot use explicit constructors.
- e.g.
```cpp
#include <iostream>

class Foo
{
public:
    explicit Foo() // note: explicit (just for sake of example)
    {
    }

    explicit Foo(int x) // note: explicit
    {
    }
};

Foo getFoo()
{
    // explicit Foo() cases
    return Foo{ };   // ok
    return { };      // error: can't implicitly convert initializer list to Foo

    // explicit Foo(int) cases
    return 5;        // error: can't implicitly convert int to Foo
    return Foo{ 5 }; // ok
    return { 5 };    // error: can't implicitly convert initializer list to Foo
}

int main()
{
    return 0;
}
```

### 19.16. Constexpr aggregates and classes:
@TODO:

### 19.17. The hidden “this” pointer and member function chaining
- Cpp utilizes a hidden pointer named `this`. This is a *const pointer* that holds the address of the current implicit object.
- e.g.
```cpp
simple.setID(2); 
Simple::setID(&simple, 2); // note that simple has been changed from an object prefix to a function argument!

// implementation
void setID(int id) { m_id = id; }
static void setID(Simple* const this, int id) { this->m_id = id; }
```
- **Explainations:**
  - When we call simple.setID(2), the compiler actually calls Simple::setID(&simple, 2), and simple is passed by address to the function.
The function has a hidden parameter named this which receives the address of simple.
Member variables inside setID() are prefixed with this->, which points to simple. So when the compiler evaluates this->m_id, it’s actually resolving to simple.m_id.
  - All non-static member functions have a this const pointer that holds the address of the implicit object. `this` always points to the object being operated on
  - Using this to create a reset function for a class back to default state:create a new object (using the default constructor), and then assign that new object to the current implicit object
```cpp
void reset()
{
    *this = {}; // value initialize a new object and overwrite the implicit object
}
```

- `this` and `const objects`:
```cpp
#include <iostream>
using namespace std;

class Something {
private:
    int value;

public:
    Something(int v) : value(v) {}

    // Non-const member function
    void setValue(int v) {
        cout << "Non-const setValue() called" << endl;
        value = v;  // allowed: can modify object
    }

    // Const member function
    int getValue() const {
        cout << "Const getValue() called" << endl;
        // value = 10; //  would be an error: cannot modify inside const function
        return value;
    }

    // Non-const getValue (overload)
    int getValue() {
        cout << "Non-const getValue() called" << endl;
        return value;
    }
};

int main() {
    Something a(5);
    const Something b(10);

    // Non-const object: can call both const and non-const functions
    cout << a.getValue() << endl;  // calls non-const version
    a.setValue(7);                 // OK
    cout << a.getValue() << endl;

    // Const object: can only call const functions
    cout << b.getValue() << endl;  // calls const version
    // b.setValue(20);             // error: cannot call non-const on const object

    return 0;
}

```

> Why this a pointer and not a reference:
Since the this pointer always points to the implicit object (and can never be a null pointer unless we’ve done something to cause undefined behavior), you may be wondering why this is a pointer instead of a reference. The answer is simple: when this was added to C++, references didn’t exist yet.

### 19.18. Classes and header files
- **Member functions** can be defined outside the class definition just like non-member functions. The only difference is that we must prefix the member function names with the name of the class type so the compiler knows we’re defining a member of that class type rather than a non-member. e.g. `void MyClass::myFnc(){//}`.
  - Putting class definitions in a header file.
  - Prefer to put your class definitions in a header file with the same name as the class. 
  - Trivial member functions (such as access functions, constructors with empty bodies,Default arguments for member functions, etc…) can be defined inside the class definition.
  - Prefer to define non-trivial member functions in a source file with the same name as the class.
- **Inline member functions**:
  - Member functions defined inside the class definition are implicitly inline. 
  - Member functions defined outside the class definition are not implicitly inline (and thus are subject to the one definition per program part of the one-definition rule). This is why such functions are usually defined in a code file (where they will only have one definition across the whole program).
  - Alternatively, member functions defined outside the class definition can be left in the header file if they are made inline.
- e.g.
```cpp
// Date.h:
#ifndef DATE_H
#define DATE_H

class Date
{
private:
    int m_year{};
    int m_month{};
    int m_day{};

public:
    Date(int year, int month, int day);

    void print() const;

    int getYear() const { return m_year; }
    int getMonth() const { return m_month; }
    int getDay() const { return m_day; }
};

#endif

// Date.cpp
#include "Date.h"

Date::Date(int year, int month, int day) // constructor definition
    : m_year{ year }
    , m_month{ month }
    , m_day{ day }
{
}

void Date::print() const // print function definition
{
    std::cout << "Date(" << m_year << ", " << m_month << ", " << m_day << ")\n";
};
```

### 19.19. Nested types (member types)
- Class types support another kind of member: `nested types` (also called member types). To create a nested type, you simply define the type inside the class, under the appropriate access specifier.
- Define any nested types at the top of your class type. 
- Outside the class, we must use the fully qualified name: `OuterClass::InerClass p{};`
- `nested classes` are members of the outer class, they can access any private members of the outer class that are in scope.
- A nested type cannot be forward declared prior to the definition of the enclosing class.\ư

### 19.20. Destructors
- For example, the classes that use a resource (most often memory, but sometimes files, databases, network connections, etc…) often need to be explicitly sent or closed before the class object using them is destroyed. In other cases, we may want to do some record-keeping prior to the destruction of the object, such as writing information to a log file, or sending a piece of telemetry to a server. The term “clean up” is often used to refer to any set of tasks that a class must perform before an object of the class is destroyed in order to behave as expected. If we have to rely on the user of such a class to ensure that the function that performs clean up is called prior to the object being destroyed, we are likely to run into errors somewhere.

<br>

- **destructor** is a special member function that is called automatically when an object of a non-aggregate class type is destroyed.
- **Syntax**:
  - The destructor must have the same name as the class, preceded by a tilde (`~`).
  - The destructor can not take arguments.
  - The destructor has no return type.
- A class can only have a single destructor.
- **An implicit destructor**: If a non-aggregate class type object has no user-declared destructor, the compiler will generate a destructor with an empty body. It is effectively just a placeholder.

- e.g.
```cpp
class NetworkData
{
private:
    std::string m_serverName{};
    DataStore m_dataQueue{};

public:
	NetworkData(std::string_view serverName)
		: m_serverName { serverName }
	{
	}

	~NetworkData()
	{
		sendData(); // make sure all data is sent before object is destroyed
	}

	void addData(std::string_view data)
	{
		m_dataQueue.add(data);
	}

	void sendData()
	{
		// connect to server
		// send all data
		// clear data
	}
};

int main()
{
    NetworkData n("someipAddress");

    n.addData("somedata1");
    n.addData("somedata2");

    return 0;
}
```

### 19.21. Class templates with member functions
-e.g.
```cpp
#include <ios>       // for std::boolalpha
#include <iostream>

template <typename T>
class Pair
{
private:
    T m_first{};
    T m_second{};

public:
    // When we define a member function inside the class definition,
    // the template parameter declaration belonging to the class applies
    Pair(const T& first, const T& second)
        : m_first{ first }
        , m_second{ second }
    {
    }

    bool isEqual(const Pair<T>& pair);
};

// When we define a member function outside the class definition,
// we need to resupply a template parameter declaration
template <typename T>
bool Pair<T>::isEqual(const Pair<T>& pair)
{
    return m_first == pair.m_first && m_second == pair.m_second;
}

int main()
{
    Pair p1{ 5, 6 }; // uses CTAD to infer type Pair<int>
    std::cout << std::boolalpha << "isEqual(5, 6): " << p1.isEqual( Pair{5, 6} ) << '\n';
    std::cout << std::boolalpha << "isEqual(5, 7): " << p1.isEqual( Pair{5, 7} ) << '\n';

    return 0;
}
```

### 19.22. Static member
- **Static member variables:** are static duration members that are shared by all objects of the class. Static members exist even if no objects of the class have been instantiated. Prefer to access them using the class name, the scope resolution operator, and the members name.
- Syntax: `static <type> <name>{<value>}`
- Static member variables are shared by all objects of the class
- Static members are not associated with class objects
- Static members are global variables that live inside the scope region of the class. 
- Access static members using the class name and the scope resolution operator (`::`).
- Defining and initializing static member variables:
	- Initialization of static member variables inside the class definition
	- Make your static members `inline` or `constexpr` so they can be initialized inside the class definition (.h).  
	
- **Static member functions:** are member functions that can be called with no object. They do not have a *this pointer, and cannot access non-static data members.
  - Can access to the satatic members via non-static function but requires us to instantiate an object to call
  - Because static member functions are not attached to an object, they have no this pointer
  - Can directly access other static members (variables or functions), but not non-static members.

- e.g.
```cpp
#include <iostream>
#include <string>

class Counter {
public:
    // Static member variable (shared by all instances)
    static inline int count {0};              // C++17+ inline initialization
    static constexpr const char* name {"App"}; // compile-time constant

    Counter() {
        ++count; // Increment static variable each time an object is created
    }

    ~Counter() {
        --count; // Decrement on destruction
    }

    // Static member function
    static void showCount() {
        std::cout << "Count: " << count << '\n';
        // std::cout << id;  not allowed — no access to non-static members
    }

    // Non-static function accessing static member
    void showInfo() const {
        std::cout << "Instance -> " << name << " | Current count: " << count << '\n';
    }

private:
    int id {0}; // non-static member variable
};

// (Alternative old-style definition — no longer needed with inline)
// int Counter::count = 0;

int main() {
    Counter c1, c2;
    c1.showInfo();     // Access static via non-static method
    Counter::showCount(); // Access static function via class name

    Counter c3;
    Counter::showCount();

    return 0;
}

```

### 19.23. Friend non-member functions
- **A friend declaration** (using the `friend` keyword) can be used to tell the compiler that some other class or function is now a friend. In C++, a friend is a class or function (member or non-member) that has been granted full access to the private and protected members of another class. In this way, a class can selectively give other classes or functions full access to their members without impacting anything else.

- **A friend function** is a function (member or non-member) that can access the private and protected members of a class as though it were a member of that class. In all other regards, the friend function is a normal function. 
```cpp
#include <iostream>

class Accumulator
{
private:
    int m_value { 0 };

public:
    void add(int value) { m_value += value; }

    // Here is the friend declaration that makes non-member function void print(const Accumulator& accumulator) a friend of Accumulator
	// member function but it have friend keyword, it is instead treated as a non-member function
    friend void print(const Accumulator& accumulator);
};

// non-member function
void print(const Accumulator& accumulator)
{
    // Because print() is a friend of Accumulator
    // it can access the private members of Accumulator
    std::cout << accumulator.m_value;
}

int main()
{
    Accumulator acc{};
    acc.add(5); // add 5 to the accumulator

    print(acc); // call the print() non-member function

    return 0;
}
```

- We can also define a friend non-member inside a class.
- Prefer non-friend functions to friend functions
- **Multiple friends:** A function can be a friend of more than one class at the same time. ( Class forward declarations serve the same role as function forward declarations)

### 19.24. Friend classes and friend member functions
- **A friend class** is a class that can access the private and protected members of another class.
- e.g. 
```cpp
#include <iostream>

class Storage
{
private:
    int m_nValue {};
    double m_dValue {};
public:
    Storage(int nValue, double dValue)
       : m_nValue { nValue }, m_dValue { dValue }
    { }

    // Make the Display class a friend of Storage
    friend class Display;
};

class Display
{
private:
    bool m_displayIntFirst {};

public:
    Display(bool displayIntFirst)
         : m_displayIntFirst { displayIntFirst }
    {
    }

    // Because Display is a friend of Storage, Display members can access the private members of Storage
    void displayStorage(const Storage& storage)
    {
        if (m_displayIntFirst)
            std::cout << storage.m_nValue << ' ' << storage.m_dValue << '\n';
        else // display double first
            std::cout << storage.m_dValue << ' ' << storage.m_nValue << '\n';
    }

    void setDisplayIntFirst(bool b)
    {
         m_displayIntFirst = b;
    }
};

int main()
{
    Storage storage { 5, 6.7 };
    Display display { false };

    display.displayStorage(storage);

    display.setDisplayIntFirst(true);
    display.displayStorage(storage);

    return 0;
}
    
```

- **A friend member function** is a specific member function of one class that is granted access to the private and protected members of another class.
- e.g. 
```cpp
#include <iostream>
class Storage; // forward declaration for class Storage
class Display
{
private:
	bool m_displayIntFirst {};

public:
	Display(bool displayIntFirst)
		: m_displayIntFirst { displayIntFirst }
	{
	}

	void displayStorage(const Storage& storage); // forward declaration for Storage needed for reference here
};

class Storage // full definition of Storage class
{
private:
	int m_nValue {};
	double m_dValue {};
public:
	Storage(int nValue, double dValue)
		: m_nValue { nValue }, m_dValue { dValue }
	{
	}

	// Make the Display::displayStorage member function a friend of the Storage class
	// Requires seeing the full definition of class Display (as displayStorage is a member)
	friend void Display::displayStorage(const Storage& storage);
};

// Now we can define Display::displayStorage
// Requires seeing the full definition of class Storage (as we access Storage members)
void Display::displayStorage(const Storage& storage)
{
	if (m_displayIntFirst)
		std::cout << storage.m_nValue << ' ' << storage.m_dValue << '\n';
	else // display double first
		std::cout << storage.m_dValue << ' ' << storage.m_nValue << '\n';
}

int main()
{
    Storage storage { 5, 6.7 };
    Display display { false };
    display.displayStorage(storage);

    return 0;
}
```

### 19.25. Ref qualifiers
- C++11 introduced a little known feature called a **ref-qualifier** that allows us to overload a member function based on whether it is being called on an lvalue or an rvalue implicit object. Using this feature, we can create two versions of getName() -- one for the case where our implicit object is an lvalue, and one for the case where our implicit object is an rvalue.
- e.g.
```cpp
#include <iostream>
#include <string>
#include <string_view>

class Employee
{
private:
	std::string m_name{};

public:
	Employee(std::string_view name): m_name { name } {}

	const std::string& getName() const &  { return m_name; } //  & qualifier overloads function to match only lvalue implicit objects
	std::string        getName() const && { return m_name; } // && qualifier overloads function to match only rvalue implicit objects
};

// createEmployee() returns an Employee by value (which means the returned value is an rvalue)
Employee createEmployee(std::string_view name)
{
	Employee e { name };
	return e;
}

int main()
{
	Employee joe { "Joe" };
	std::cout << joe.getName() << '\n'; // Joe is an lvalue, so this calls std::string& getName() & (returns a reference)

	std::cout << createEmployee("Frank").getName() << '\n'; // Frank is an rvalue, so this calls std::string getName() && (makes a copy)

	return 0;
}
```