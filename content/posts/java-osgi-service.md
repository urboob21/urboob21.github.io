---
author: "Phong Nguyen"
title: "Java OSGi Service"
date: "2025-02-10"
description: "Java: OSGi Service."
tags: ["java"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 3    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
Explain how to use the OSGi Service
[Refer1](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/different-between-osgi-services-and-osgi-component/m-p/621761)<br>
[Refer2](https://www.vogella.com/tutorials/OSGi/article.html#osgi-services)<br>
[Refer3](https://vogella.com/blog/getting-started-with-osgi-declarative-services-2024/)<br>
## 1. Introduction
- **OSGi Services** are essentially Java objects that provide a specific functionality or interface, and other components,plugins can dynamically discover and use these services.
- Multiple plugins can provide a service implementation for the service interface. Plugins can access the service implementation via the service interface.
- **E.g.** When you want to create modular that can be added or removed at runtime, you can use the OSGi service.

## 2. Using OSGi
- There are several ways of defining, providing and consuming service.
- Let's start with create the API project to provide the external APIs for other components.
  
## 2.1. Create API project

## 2.2. Specific the API
- Create an interface for the service definition.

```java
package com.test.internal;

/**
 * Service for doing something
 *
 * @noimplement This interface is not intended to be implemented by clients.
 * @noextend This interface is not intended to be extended by clients.
 */
public interface IDoingSomethingService {
	public void doSomething();
}

```
- Create the service implementation that implements service interface.
```java
@Component(service = { DoingSomethingService.class, IDoingSomethingService.class }, immediate = false)
public class DoingSomethingService implements IDoingSomethingService{

    @Override
    public void doSomething(){
        syserr("DoingSomethingService did something");
    }

}

```


- Testing: [ServiceCaller](https://help.eclipse.org/latest/nftopic/org.eclipse.platform.doc.isv/reference/api/org/eclipse/core/runtime/ServiceCaller.html)
```java
 ServiceCaller.callOnce(MyClass.class, IDoingSomethingService.class, (myService) -> myService.doSomething());

```

## 3 DS Annotations
### 3.1. @Component
- It i to identify the class as a Service Component and is used to generate the Component Description.
- **Default**:
>    Its name is the full qualified class name
    It registers all of the class’s directly implemented interfaces as services
    The instance will be shared by all bundles
    It is enabled
    It is immediate if it has no services, otherwise it is delayed
    It has an optional configuration policy
    The configuration PID is the full qualified class name

- The defaults can be changed via **annotation type elements**:
  - `service`	: The name(s) of the interface or class this component is registered under as a service. Needs to be a full qualified class name.
  - `immediate`:	Control whether a component configuration should be immediately activated after becoming satisfied or if the activation should be delayed. Needs to be false in case the factory attribute is set also, needs to be true if no service is provided.
  - ...

### 3.2. @Activate, @Deactivate, @Modified
- Used to specify methods that should be called when a life cycle event happens.
  - `@Activate`:	The method that should be called on component activation.
  - `@Modified`:	The method that should be called if a configuration is updated using the ConfigurationAdmin.
  - `@Deactivate`:	The method that should be called on component deactivation.

- **e.g.**
```java
@Activate
private void activate() {

    //do some initialization stuff
}
```
 
### 3.3. @Reference
- Used to specify the dependency on other services.
- When the annotation is applied to a **method**, the method is the bind method of the reference.
- When the annotation is applied to a **field**, the field will contain the bound service(s) of the reference.

- Field Injection
```java
@Component
public class MyComponent {
    
    @Reference
    private MyService myService;

    public void doSomething() {
        myService.execute();
    }
}
✅ Khi MyService có trong OSGi Service Registry, nó sẽ tự động được inject vào myService.
❌ Nếu không có MyService, myService sẽ là null.
```

- Method Injection
```java
@Component
public class MyComponent {
    private MyService myService;

    @Reference
    protected void bindMyService(MyService myService) {
        this.myService = myService;
    }

    protected void unbindMyService(MyService myService) {
        if (this.myService == myService) {
            this.myService = null;
        }
    }
}
✅ Phương thức bind (bindMyService): Được gọi khi MyService có sẵn trong Service Registry.
✅ Phương thức unbind (unbindMyService): Được gọi khi service bị gỡ khỏi Registry.
❗ Đây là cách tốt để xử lý service lifecycle (khi service bị remove).
```