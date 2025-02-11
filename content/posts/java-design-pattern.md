---
author: "Phong Nguyen"
title: "Design Patterns"
date: "2025-02-11"
description: "Design Patterns."
tags: ["java"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 11    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
Explain how to use the design patterns.
[Refer1](https://github.com/kamranahmedse/design-patterns-for-humans?tab=readme-ov-file#-simple-factory)
[Refer2](https://java-design-patterns.com/)
[Refer3](https://www.tutorialspoint.com/design_pattern/factory_pattern.htm)<br>
## 1. 🏠 Simple Factory
- Factory is an object for creating other objects 
- **Use cases:**
  - when the class does not know beforehand the exact types and dependencies of the objects it needs to create.
  - When a method returns one of several possible classes that share a common super class and wants to encapsulate the logic of which object to create.
  - when designing frameworks or libraries to give the best flexibility and isolation from concrete class types

- **Examples:** You want to manufacture the products. You must be able to create both A and B (C,D) products and switch between them without modify the existing source codes.
  -  Design UML:
![image](/images/factory_pattern.png)
 
  - Implementation codes:

```java
package design.patterns.factory;

public interface IProduct {
	public void getType();
}

package design.patterns.factory;

public class ProductA implements IProduct {
	private static String TYPE_NAME = "ProductA";
	@Override
	public void getType() {
		System.err.println(TYPE_NAME);
	}

}

package design.patterns.factory;

public class ProductB implements IProduct {
	private static String TYPE_NAME = "ProductB";
	@Override
	public void getType() {
		System.err.println(TYPE_NAME);
	}
}

package design.patterns.factory;

public class ProductC implements IProduct {
	private static String TYPE_NAME = "ProductC";
	@Override
	public void getType() {
		System.err.println(TYPE_NAME);
	}

}

package design.patterns.factory;

public enum ProductType {
	TYPEA,
	TYPEB,
	TYPEC;
}

package design.patterns.factory;

public class ProductFactory {
	public static IProduct createProduct(ProductType type) {
		if (type == null) {
			return null;
		}

		switch (type) {
		case TYPEA: {
			return new ProductA();
		}
		case TYPEB: {
			return new ProductB();
		}
		case TYPEC: {
			return new ProductC();
		}
		}
		return null;
	}
}

package design.patterns.factory;

public class MainTestFactory {

	public static void main(String[] args) {
		ProductFactory productFactory = new ProductFactory();
		
		IProduct product1 = productFactory.createProduct(ProductType.TYPEA);
		product1.getType();
		
		IProduct product2 = productFactory.createProduct(ProductType.TYPEB);
		product2.getType();
		
		IProduct product3 = productFactory.createProduct(ProductType.TYPEC);
		product3.getType();
				
	}

}


```
