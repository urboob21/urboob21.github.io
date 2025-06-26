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

## 1. Introduction
- Scenario: Debugger View Fetching Data
Context: We want to show variable values in the Variables View as the user steps through the code.

- **Asynchronous**
  - When the user steps through the program, the debugger needs to **fetch variable values** from the target (like a remote system). This operation might **take a few milliseconds or more**, depending on the connection. If we **block the UI thread, the UI freezes**.
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

- **Synchronous** 
  - We want to implement a feature like “evaluate expression” that requires the value immediately. We don’t want to rewrite your entire logic using async callbacks just to get the result of getVariableValue().

  - Solution: Use a DSF Query to wrap the async call in a synchronous interface:

```java
String getVariableValueSync(String varName) {
    Query<String> query = new Query<>() {
        @Override
        protected void execute(DataRequestMonitor<String> rm) {
            getVariableValue(varName, rm);  // Asynchronous method
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

- Summary Table:
|Use Case | Method Type |	Reason |
|---------|-------------|----------|
|Fetching variable values in UI|	Asynchronous|	Avoid blocking UI thread|
|Evaluating expressions in scripts|	Synchronous|	Scripts expect immediate result|
|Performing batch operations|	Asynchronous|	Can parallelize or chain operations|
|Calling legacy blocking logic|	Synchronous|	Synchronous integration is easier|
|Nested UI event (e.g., button click)|	Asynchronous|	Keeps UI responsive|

## 2. Asynchronous Methods: Request Monitor , Data Request Monitor,  Multi-Request Monitor
- There is an standard callback object used in DSF, the request monitor, has following features:
  - Executor
  - Status
  - Callback methods: handleCompleted(), handleOK(), handleError(),...
  - Parent request monitor
  - Returning values to the caller. ( Data Request Monitor)
  - Done count. (Multi-Request Monitor)

- Examples: 
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
			protected void handleCompleted() {
				System.out.println("Fcn1 has done");
			}
		};
		asyncFcn1(rm1);

		DataRequestMonitor<Integer> rm2 = new DataRequestMonitor<>(executor, null) {
			@Override
			protected void handleCompleted() {
				Integer data = getData();
				System.out.println("Fcn2 has done");
				System.out.println("Returned data: " + data);
			}
		};
		asyncFcn2(2, rm2);
	}

	static void asyncFcn1(RequestMonitor rm) {
		System.out.println("Run Fcn1");
		rm.done();
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

## 3. Concurrent
### 3.1. Query ( implements Runnable)
- Using a query can use a execute() implementation in order to call other asynchronous method.
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

	// Asynchronous addition method ( fetch value etc, take more time here)
	static void asyncAdd(int a, int b, DataRequestMonitor<Integer> rm) {
		rm.setData(a + b);
		rm.done();
	}

	// Synchronous method using DSF Query
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

