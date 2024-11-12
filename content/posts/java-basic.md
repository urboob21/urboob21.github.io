---
author: "Phong Nguyen"
title: "Java Platform Standard Edition"
date: "2024-11-12"
description: "Basic Java Platform Standard Edition."
tags: ["java"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 4    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
## 1. Introduction
**References:** [Java Platform Standard Edition 8 Documentation](https://docs.oracle.com/javase/8/docs/)
Oracle has two products implement **Java SE**:
- JDK (a.k.a Java SE Development Kit): is a superset of JRE
- JRE (a.k.a Java SE Runtime Environment): provides the libs, the Java Virtual Machine (JVM) and other components.
![image](/images/java_concept_diagram.png)<br>

### 1.1. Java Programming Language
- Source code is first written in `.java` file.
- Those source files are then compiled into `.class` files by the `javac` compiler
- A `.class` file contains `bytecodes`(the machine language of the Java VM)
- The `.class` file will be run by Java tool as an instance of the Java VM. 
![image](/images/overview_java_process.png)<br>

Because the Java VM is available on many different OS, the same `.class` files are capable of running on many OS too.
![image](/images/java_capable_running_os.png)<br>

### 1.2.  Comparison of the C and Java Compilation & Linking Processes
![image](/images/java_c_processes.png)<br>
![image](/images/java_processes.png)<br>

## 2. 