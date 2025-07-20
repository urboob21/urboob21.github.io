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
## 3. Need to update ...