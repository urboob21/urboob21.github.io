---
author: "Phong Nguyen"
title: "DSF"
date: "2025-05-13"
description: "Java: Program with DSF"
tags: ["java","eclipse"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 12    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
Explain how to use the DSF.
[Refer1](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.cdt.doc.isv%2Fguide%2Fdsf%2Fintro%2Fdsf_programming_intro.html)
<br>

![image](/images/dsf_overview.png)

## 1. Introduction
- Scenario: **Debugger View Fetching Data**
Context: We want to show variable values in the Variables View as the user steps through the code.

### 1.1. **Asynchronous**
  - Req: When the user steps through the program, the debugger needs to **fetch variable values** from the target (like a remote system). This operation might **take a few milliseconds or more**, depending on the connection. If we **block the UI thread, the UI freezes**.
  - Solution: Use an asynchronous method to fetch variable data

```java
void getVariableValue(String varName, DataRequestMonitor<String> rm) {
    // Imagine this takes time (e.g., contacting GDB or a remote debugger)
    // Simulate async call
    executor.execute(() -> {
        String value = remoteFetchValue(varName);  // Slow operation
        rm.setData(value);
        rm.done();
    });
}
```
  - Why Async?
    - Prevents blocking the UI.
    - Allows the Eclipse debug framework to continue updating other views.

### 1.2. **Synchronous** 
  - We want to implement a feature like **evaluate expression** that requires the value immediately. We don’t want to rewrite your entire logic using async callbacks just to get the result of getVariableValue().

  - Solution: Use a DSF Query to **wrap the async call in a synchronous interface**:

```java
String getVariableValueSync(String varName) {
    Query<String> query = new Query<>() {
        @Override
        protected void execute(DataRequestMonitor<String> rm) {
            getVariableValue(varName, rm);  // Wrap Asynchronous method
        }
    };

    executor.execute(query);

    try {
        return query.get();  // Blocks until result is ready (asynchronous method done  rm.done();)
    } catch (Exception e) {
        return "Error";
    }
}
```

  - Why Sync?
    - Some contexts expect a result immediately.
    - Simplifies logic when integrating with legacy synchronous systems.

### 1.3. Summary Table:

|Use Case | Method Type |	Reason |
|---------|-------------|----------|
|Fetching variable values in UI|	**Asynchronous**|	Avoid blocking UI thread|
|Nested UI event (e.g., button click)|	**Asynchronous**|	Keeps UI responsive|
|Performing batch operations|	**Asynchronous**|	Can parallelize or chain operations|
|Calling legacy blocking logic|	Synchronous|	Synchronous integration is easier|
|Evaluating expressions in scripts|	Synchronous|	Scripts expect immediate result|

## 2. Asynchronous Methods: Request Monitor , Data Request Monitor,  Multi-Request Monitor
- `Async methods is a way that use a callback object to indicate their completion`.

## 2.1. Request Monitor
- There is an standard **callback object** used in DSF, the request monitor, has following features:
  - **Executor**: executor to invoke the callback method
  - **Status**: indicating the success or failure of the callback method
  - **Callback methods**: methods which are invoked when the callback is invoked: handleCompleted(), handleOK(), handleError(),... -> overwrite these if need to perform additional process after async method completion.
  - **Parent request monitor**: 
  - Returning values to the caller. ( Data Request Monitor)
  - Done count. (Multi-Request Monitor)

## 2.2. Data Request Monitor
- The **Request Monitor** can return status of the async method but do not return a value to caller. **Data Request Monitor** can be used for that purpose.

## 2.3. Counting Request Monitor
- When we need to manage the completion of several request monitor. It is configured such that it's done() method needs to be called a count number of times before the callback method is invoked. 

## 2.4 Examples:
```java
import java.util.concurrent.Executor;

import org.eclipse.cdt.dsf.concurrent.DataRequestMonitor;
import org.eclipse.cdt.dsf.concurrent.ImmediateExecutor;
import org.eclipse.cdt.dsf.concurrent.RequestMonitor;

public class Test {

	public static void main(String[] args) {
		Executor executor = ImmediateExecutor.getInstance(); // Get the Executor
		RequestMonitor rm1 = new RequestMonitor(executor, null) {
			@Override
			protected void handleCompleted() {   // callback function 
				System.out.println("Fcn1 has done");
			}
		};
		asyncFcn1(rm1);

		DataRequestMonitor<Integer> rm2 = new DataRequestMonitor<>(executor, null) {
			@Override
			protected void handleCompleted() {  // callback function
				Integer data = getData();
				System.out.println("Fcn2 has done");
				System.out.println("Returned data: " + data);
			}
		};
		asyncFcn2(2, rm2);
	}

	static void asyncFcn1(RequestMonitor rm) {
		System.out.println("Run Fcn1");
		rm.done(); // notify that the async method done -> will run the callback function 
	}

	static void asyncFcn2(int value, DataRequestMonitor<Integer> rm) {
		System.out.println("Run Fcn2");
		rm.setData(value);
		rm.done();
	}
}
```

- Output:
```powershell
Run Fcn1
Fcn1 has done
Run Fcn2
Fcn2 has done
Returned data: 2
```

## 3. Concurrency
### 3.1. Query ( implements Runnable)
- Using a Query can use a execute() (overwrite) implementation in order **to call other asynchronous method** from a **sync method**
```java

import java.util.concurrent.Executor;

import org.eclipse.cdt.dsf.concurrent.DataRequestMonitor;
import org.eclipse.cdt.dsf.concurrent.ImmediateExecutor;
import org.eclipse.cdt.dsf.concurrent.Query;

public class TestQuery {

	public static void main(String[] args) {
		Executor executor = ImmediateExecutor.getInstance();
		int result = syncAdd(2, 3, executor);
		System.out.println("Result: " + result); // Expected output: Result: 5
	}

	// Asynchronous method ( fetch value etc, take more time here)
	static void asyncAdd(int a, int b, DataRequestMonitor<Integer> rm) {
		rm.setData(a + b);
		rm.done();
	}

	// Synchronous method using DSF Query to call a async method
	static int syncAdd(final int a, final int b, final Executor executor) {
		Query<Integer> query = new Query<>() {
			@Override
			protected void execute(final DataRequestMonitor<Integer> rm) {
				asyncAdd(a, b, rm); // Call async method inside query
			}
		};

		executor.execute(query); // Submit the query to the executor

		try {
			return query.get(); // Blocks and waits for result
		} catch (Exception e) {
			e.printStackTrace();
			return -1; // Return error code on failure
		}
	}

}
```

### 3.2. Synchronization
- DSF uses a single-threaded executor (DSF Executor thread) as the primary mechanism for safe access to date. (race conditions + deadlocks)
 
### 3.3. Annotations
- DSF defines a number of annotations that can be used to determine what are the rules governing access to the various data objects.

## 4. Services
### 4.1. OSGi

