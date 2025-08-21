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
>It's a build tool (specifically, make is a build automation utility).
How it works: Reads a file called Makefile that describes rules for how to build source files into targets (executables, libraries, etc.).

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
## Basic Command-line Commands
### Unix/Linux Commands

| Command | Description                                      | Example                       |
|---------|--------------------------------------------------|-------------------------------|
| `ls`    | List files and directories in the current directory. | `ls`                          |
| `cd`    | Change the current directory.                    | `cd /path/to/directory`      |
| `pwd`   | Print the current working directory.             | `pwd`                         |
| `mkdir` | Create a new directory.                          | `mkdir new_directory`        |
| `touch` | Create a new file or update the timestamp of an existing file. | `touch newfile.txt`          |
| `cp`    | Copy files or directories.                       | `cp source.txt destination.txt` |
| `mv`    | Move or rename files or directories.            | `mv oldname.txt newname.txt` |
| `rm`    | Remove files or directories.                     | `rm file.txt`                |
| `cat`   | Display the contents of a file.                  | `cat file.txt`               |
| `man`   | Display the manual for a command.                | `man ls`  

### Windows Commands

| Command | Description                                      | Example                       |
|---------|--------------------------------------------------|-------------------------------|
| `dir`   | List files and directories in the current directory. | `dir`                         |
| `cd`    | Change the current directory (same as Unix).    | `cd \path\to\directory`      |
| `mkdir` | Create a new directory.                          | `mkdir new_directory`        |
| `echo`  | Display a message or value.                      | `echo Hello, World!`         |
| `copy`  | Copy files.                                     | `copy source.txt destination.txt` |
| `move`  | Move or rename files or directories.            | `move oldname.txt newname.txt` |
| `del`   | Delete files.                                   | `del file.txt`               |
| `type`  | Display the contents of a file.                 | `type file.txt`              |
| `help`  | Display a list of commands and their descriptions. | `help`                       |
| `cls`   | Clear the screen.  

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
#### Example:
```makefile
file := file1 file2
some_file: $(files) # some_file: file1 file2
```
#### 1.3.1. Flavors
There are two flavors of variables:
- `=`: Only looks for the variables when the command is used, not when it's defined.
- `=:`: Like const
##### Example:
```makefile
# Recursive variable. 
one = one ${later_variable}
# Simply expanded variable. 
two := two ${later_variable}
later_variable = later
all: 
	echo $(one)    # This will print "later" below
	echo $(two)    # This will not print "later"
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
### 4.1. Pattern Rules:
Pattern rules contain a `%` in the target. We can look at them as two ways:
- A way to define your own implicit rules.
- A simple form of static pattern rules
#### Example 1: Define a pattern rule that compiles every .c file into a .o file
```makefile
%.o : %.c
		$(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@
# gcc -c -Wall -O2 -I./include main.c -o main.o 
```
#### Explain:
- `%.o`: This is target matching any `.o` object file.
- `%.c`: This is prerequisite matching any `.c` source file.
=> If target is *main.o*, Make will understand that is needs the main.c compile.
- `$(CC)`: This is the variable represents the C/C++ compiler. `(CC = gcc)`
- `-c`: This option tells the compiler to compile the source file in to an object file without linking.
- `$(CFLAGS)`: Holds the flags for the `C Compiler`. Used for optimization or specific compiler options. `(CFLAGS = -Wall -O2)`. Like `-Wall` to show all warning.
- `$(CPPFLAFS)`: Hold the flags for the `Preprocessor`. `(CPPGLAGS = -I./include)`. Like `-I` to specific the directory of header files.
-  `$<`: List prerequisites. In this case, it refers to the corresponding .c files.
-  `-o`: Specifics the output file's name.
-  `$@`: Target name

#### Example 2: Define a pattern rule that creates empty .c files when needed
```makefile
%.c:
   touch $@ #on Linux
    # mkdir $@ #on Window
```
#### Explain:
- `$@`: Refers to target name.
- `touch`: create a file with name is target.
### 4.2 Implicit Rules:
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
### 4.3 Static Pattern Rules:
The way to write less in Makefile.
```makefile
targets...: target-pattern: prereq-patterns...
    command
```
The essence is that given target is matched by the target-pattern via `%` wildcard.
**Q: Write make file compile .c files into .o file**
#### Example1: Manual way
```makefile
objects = foo.o bar.o all.o
all: $(objects)
	$(CC) $^ -o all
foo.o: foo.c
	$(CC) -c foo.c -o foo.o
bar.o: bar.c
	$(CC) -c bar.c -o bar.o
all.o: all.c
	$(CC) -c all.c -o all.o
all.c:
	echo "int main() { return 0; }" > all.c
%.c:
	touch $@
clean:
	rm -f *.c *.o all
```
#### Example2: Efficient way  - Using a static pattern rule
```makefile
objects = foo.o bar.o all.o
all: $(objects)
	$(CC) $^ -o all

# Static pattern rule : targets ...: target-pattern: prereq-patterns ...
$(objects): %.o: %.c        
	$(CC) -c $^ -o $@
all.c:
	echo "int main() { return 0; }" > all.c
%.c:
	touch $@
clean:
	rm -f *.c *.o all
```

### 4.4 Static Pattern Rules and *Filter*:
We can use the `filter` functions in Static pattern rules to math the correct files. Introduce the filter function [here]().
#### Example:
```makefile
obj_files = foo.result bar.o lose.o
src_files = foo.raw bar.c lose.c

all: $(obj_files)
.PHONY: all 
# Ex 1: .o files depend on .c files. Though we don't actually make the .o file.
$(filter %.o,$(obj_files)): %.o: %.c
	echo "target: $@ prereq: $<"
# Ex 2: .result files depend on .raw files. Though we don't actually make the .result file.
$(filter %.result,$(obj_files)): %.result: %.raw
	echo "target: $@ prereq: $<" 
%.c %.raw:
	touch $@
clean:
	rm -f $(src_files)
```
> *Why is .PHONY important?
If you have a target called all, but there happens to be a file named all in the directory, running make all will make Make check for the file all instead of executing the commands associated with the target. If the file exists and is up-to-date, Make won't execute the commands. By marking all as .PHONY, Make will always execute the target, regardless of whether a file with the same name exists.*

### 4.5 Double-Colon Rules:
This type rules ae rarely used, but allow multiple rules to be defines for the same target.
#### Example:
```makefile
all::
    echo "Hello-option1"
all::
    echo "Hello-option2"
```

## 5 Commands and execution
 If you `Ctrl+c` make, it will delete the newer targets it just made.
### 5.1 Command Echoing/ Silencing
We can add `@` before a command to stop it from being printed.
```makefile
all: 
	@echo "This make line will not be printed"
	echo "But this will"
```

### 5.2 Command Execution
Each command is run in a new shell (or at least the effect is as such.)
```makefile
all: 
	cd ..
	# The cd above does not affect this line, because each command is effectively run in a new shell
	echo `pwd`
	# This cd command affects the next because they are on the same line
	cd ..;echo `pwd`
	# Same as above '\'
	cd ..; \
	echo `pwd`
```

### 5.3 Default Shell
The default shell is `/bin/sh`. We can change it by changing the variable SHELL.
```makefile
SHELL=/bin/bash
...
```

### 5.4 Double dollar sign
`$$`: String to have a dollar sign.

### 5.5 Error handling
- Add `-k` when running make to continue running even in the face on the errors. Useful if we want to see all the errors at once.
- Add `-` before a command to suppress the error.
- Add `-i` to make to have this happen for every command.
```makefile
one:
	# This error will be printed but ignored, and make will continue to run
	-false
	touch one
```

### 5.6 Recursive use of make
To recursively call a makefile, use the special $(MAKE) instead of make because it will pass the make flags for you and won't itself be affected by them.
```makefile
new_contents = "hello:\n\ttouch inside_file"
all:
	mkdir -p subdir
	printf $(new_contents) | sed -e 's/^ //' > subdir/makefile
	cd subdir && $(MAKE)

clean:
	rm -rf subdir
```

### 5.7 Export, Environment
```makefile
all:
	# Print out the Shell variable
	echo $$shell_env_var

	# Print out the Make variable
	echo $(shell_env_var)
```
The export directive takes a variable and sets it the environment for all shell commands in all the recipes
```makefile
shell_env_var=Shell env var, created inside of Make
export shell_env_var
all:
	echo $(shell_env_var)
	echo $$shell_env_var
```
continue... [here](https://makefiletutorial.com/#export-environments-and-recursive-make) 

### 5.8 Arguments
There's a [list of options](https://www.gnu.org/software/make/manual/make.html#Options-Summary) that can be run from make.

