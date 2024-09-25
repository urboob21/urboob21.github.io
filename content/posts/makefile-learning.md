---
author: "Phong Nguyen"
title: "Makefile Guide"
date: "2024-08-09"
description: "Makefile Learning."
tags: ["makefile"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 2    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
## Introduction
GNU Make is a tool which controls the generation of executables and other non-source files of a program from the program's source files.
Make get its knowledge of how to build your program from a file called the `makefile`, which lists each of the non-source files and how to compute it from the other files.
When you write a program, you should write a makefile for it, so that it is possible to use Make to build and install the program.

Makefiles are used to help decide which parts of a large program need to be recompiled.If any file's dependencies changes, then the file will get recompiled.

C/C++ alternatives build systems are: CMake, Ninja,...
Java alternatives build systems are: Maven, Gradle, ...

This version for GNU Make. The examples work for Make versions 3 and 4.
*Make script smart and optimize building - build system*
## Requirement:

1. Install the Make:
2. Add to the environment:

## Running the first example:
**Note Makefiles must be indented using TABs and not spaces or `make` will fail.**

1. Create makefile.mk
```makefile
all:
	echo "Hello Word!"
option1:
	echo "Option1!"

```

1. Open cmd, cd to dir
2. run `make -f makefilename.mk`
3. Observe the output (should be as bellow)
```
C:\Users\phong.nguyen-van\Desktop\datasets\learning\make>make -f makefile.mk
echo "Hello Word!"
"Hello Word!"

C:\Users\phong.nguyen-van\Desktop\datasets\learning\make>make option1 -f makefile.mk
echo "Option1!"
"Option1!"
```

## 1. Makefile Syntax
### 1.1. Basic:
```makefile
targets: prerequisites
    command
    command
    command
```
- `targets` are file names, separated by spaces.
- `commands` are a series of steps used to make the target(s). Start with **a tab character**.
- `prerequisites` are also file names, separated by spaces. Need to exist before commands run. (dependencies)

#### Example1:
```makefile
hello:
	echo "Hello, World"
	echo "This line will print if the file hello does not exist."
```
-   Target: hello
-   2 commands
-   0 prerequisites

We'll then run `make hello`. As long as the `hello` file does not exist, the commands will run.If `hello` file exist, no commands will run.
*target is run -> commands are run -> commands will create a file with the same name as the target*

#### Example2:
```makefile
some_file: other_file
	echo "This will always run, and runs second"
other_file:
	echo "This will always run, and runs first"
```
We'll then run `make`
1. Make select fist target default: `some_file`
2. `some_file` require `other_file`, so make searches for the `other_file` target
3. `other_file` has no dependencies, so its command run. (We have `other_file` file)
4. `some_file` have dependencies `other_file`, so its command run and create the `some_file` file.

### 1.2 Make clean
`clean` is often used as a target that removes the output of other target.
Run explicitly : `make clean`
#### Example3:
```makefile
some_file: other_file
	echo "This will always run, and runs second"
other_file:
	echo "This will always run, and runs first"
...
clean:
    rm -f some_file
```
### 1.3 Variables
Variables can only be strings. Use `:=` or `=`.
#### Example4:
```makefile
file := file1 file2
some_file: $(files) # some_file: file1 file2
...
```

## 2. Targets
### 2.1 All target
This is the first rule listed and run by default.
#### Example:
```makefile
all: one two three

one:
    #todo st
two:
    #todo st
three:
    #todo st
clean:
    rm -f one two three
...
```
### 2.2 Multi targets
When there are multiple targets for a rule, the commands will be run for each target.
```makefile
all: f1 f2

f1 f2:
    echo $@
# Equivalent to:
# f1:
#	 echo f1
# f2:
#	 echo f2
...
```
**Note**: `$@` is an automatic variable that contains the target name. `echo` is used in command in command-line to display output.

## 3. Automatic Variables and Wildcards
### 3.1 Wildcards '*' : [ˈæs.tɚ.ɪsk]
`*` searches your filesystem for matching file name.
#### Example1:
```makefile
# Print out every .c file name
print: $(wildcard *.c) 
    ls -la $?
...
```
- `$(wildcard *.c)`: return a list of all the .c files in the current directory.
- `$?`: automatic variable represents the names of all prerequisites.
- `ls -la $?`: list all files.

#### Example2:
```makefile
thing_wrong := *.o # Don't do this! '*' will not get expanded
thing_right := $(wildcard *.o)

all: one two three four

# Fails, because $(thing_wrong) is the string "*.o"
one: $(thing_wrong)

# Stays as *.o if there are no files that match this pattern :(
two: *.o 

# Works as you would expect! In this case, it does nothing.
three: $(thing_right)

# Same as rule three
four: $(wildcard *.o)
```
### 3.2 Wildcards '%' :
...

### 3.3 Automatic Variables :
```makefile
hey: one two
	echo $@    # output target name 'hey'
	echo $?    # Outputs all prerequisites newer than the target. 'one' 'two' 
	echo $^    # # Outputs all prerequisites ??
	echo $<    # Outputs the first prerequisite. 'one'
one:

two:

```
```bash
output ===========
make -f .\Makefile.mk
echo hey
hey
echo one two
one two
echo one two
one two
echo one
one
touch hey
...
```

## 4. Fancy Rules
### 4.1 Implicit Rules:
Here's a list of implicit rules:
- Compiling a C program: `n.o` is made automatically from `n.c` with a command of the form `$(CC) -c $(CPPFLAGS) $(CFLAGS) $^ -o $@`
Compiling a C++ program: `n.o` is made automatically from `n.cc` or `n.cpp` with a command of the form `$(CXX) -c $(CPPFLAGS) $(CXXFLAGS) $^ -o $@`
Linking a single object file: `n` is made automatically from `n.o` by running the command `$(CC) $(LDFLAGS) $^ $(LOADLIBES) $(LDLIBS) -o $@`

The important variables used by implicit rules are:
- `CC`: Program for compiling C programs; default cc
- `CXX`: Program for compiling C++ programs; default g++
- `CFLAGS`: Extra flags to give to the C compiler
- `CXXFLAGS`: Extra flags to give to the C++ compiler
- `CPPFLAGS`: Extra flags to give to the C preprocessor
- `LDFLAGS`: Extra flags to give to compilers when they are supposed to invoke the linker
#### Example
```makefile
CC = gcc # select gcc toolchain for compiling C program
CFLAGS = -g #turn on debug info

# Implicit rule #1: blah is built via the C linker implicit rule
# Implicit rule #2: blah.o is built via the C compilation implicit rule, because blah.c exists
# Implicit : $(CC) $(CFLAGS) -o blah blah.o
blah: blah.o
    # $(CC) $(CFLAGS) -o blah blah.o
blah.c:
	echo "int main() { return 0; }" > blah.c

clean:
	rm -f blah*
...
    
```

### 4.2 Static Pattern Rules:
The way to write less in Makefile.
```makefile
targets...: target-pattern: prereq-patterns...
    command
```
The essence is that given target is matched by the target-pattern via `%` wildcard.
 

