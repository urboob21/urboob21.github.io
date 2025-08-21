---
author: "Phong Nguyen"
title: "CMake Guide"
date: "2025-08-21"
description: "CMake Learning."
tags: ["cmake"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 2    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
## 1. Introduction
- It's a meta-build system generator.
- How it works:
    - We write high-level instructions in a CMakeLists.txt file (platform-agnostic).Then CMake generates build system files for the platform we choose:
    - On Linux/Unix → generates Makefile (for make) or build.ninja (for ninja).
    - On Windows → generates Visual Studio solutions (.sln).
    - On macOS → can generate Xcode projects.

## 2. Setup
- Linux: Install: `sudo apt install cmake` , Verify: `cmake --version` 

## 3. How to work with CMake

1. Create CMakeLists.txt and resources file
2. Run `cmake <dir-contain-CMakeLists.txt>` to set up & generate build system.
3. Run `cmake --build <dir-contain-buildsystem>` to actually build/compile the project.
4. Run the target build (e.g. `./targetName`)


### 3.1. 
> CMakeLists.txt
```makefile
cmake_minimum_required(VERSION 3.0)	// specifying a minimum CMake version
project(CmakeprojectName)	// set the project name
add_executeable(targetName "headerfile.h" "sourcefile1.cpp" "sourcefile2.cpp") // specified source code files
```
