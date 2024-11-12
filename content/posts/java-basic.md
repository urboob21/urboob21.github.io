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
**References:** [Java Platform Standard Edition 8 Documentation](https://docs.oracle.com/javase/8/docs/)<br>

## 1. Introduction
Oracle has two products implement **Java SE**:
- **JDK** (Java SE Development Kit): is a superset of JRE.
- **JRE** (Java SE Runtime Environment): provides the libraries, the Java Virtual Machine (JVM) and other components.
![image](/images/java_concept_diagram.png)<br>

### 1.1. Java Programming Language
- Source code is first written in `.java` file.
- Those source files are then compiled into `.class` files by the `javac` compiler.
- A `.class` file contains `bytecodes`(the machine language of the Java VM).
- The `.class` file will be run by Java tool as an instance of the Java VM. 
![image](/images/overview_java_process.png)<br>

Because the Java VM is available on many different OS, the same `.class` files are capable of running on many OS too.
![image](/images/java_capable_running_os.png)<br>

### 1.2.  Comparison of the C and Java Compilation & Linking Processes
![image](/images/java_c_processes.png)<br>
![image](/images/java_processes.png)<br>

<br>

## 2. Trail: Learning the Java Language
This trail covers the fundamentals of programming in the Java programming language.
1. [Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/index.html) teaches you the core concepts behind object-oriented programming: objects, messages, classes, and inheritance. This lesson ends by showing you how these concepts translate into code. Feel free to skip this lesson if you are already familiar with object-oriented programming.
2. [Language Basics](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/index.html) describes the traditional features of the language, including variables, arrays, data types, operators, and control flow.
3. [Classes and Objects](https://docs.oracle.com/javase/tutorial/java/javaOO/index.html) describes how to write the classes from which objects are created, and how to create and use the objects.
4. [Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/index.html) are a form of metadata and provide information for the compiler. This lesson describes where and how to use annotations in a program effectively.
5. [Interfaces and Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/index.html) describes interfaces—what they are, why you would want to write one, and how to write one. This section also describes the way in which you can derive one class from another. That is, how a subclass can inherit fields and methods from a superclass. You will learn that all classes are derived from the Object class, and how to modify the methods that a subclass inherits from superclasses.
6. [Numbers and Strings](https://docs.oracle.com/javase/tutorial/java/data/index.html) This lesson describes how to use Number and String objects The lesson also shows you how to format data for output.
7. [Generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html) are a powerful feature of the Java programming language. They improve the type safety of your code, making more of your bugs detectable at compile time.
8. [Packages](https://docs.oracle.com/javase/tutorial/java/package/index.html) are a feature of the Java programming language that help you to organize and structure your classes and their relationships to one another.

<br>

9.  Others: 
- [JavaSE-8](https://docs.oracle.com/javase/8/docs/)
- [JavaSE-17](https://docs.oracle.com/en/java/javase/17/index.html)
- [JavaSE-21](https://docs.oracle.com/en/java/javase/21/index.html)