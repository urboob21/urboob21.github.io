---
author: "Phong Nguyen"
title: "Eclipse Project"
date: "2025-02-05"
description: "Eclipse Project."
tags: ["eclipse"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 10    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
Explain how to create the eclipse project.    
**References:** 
[Wizards and Dialogs](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.pde.doc.user%2Fguide%2Ftools%2Fproject_wizards%2Fnew_fragment_project.htm)
[Eclipse fragment projects - Tutorial](https://www.vogella.com/tutorials/EclipseFragmentProject/article.html#example-manifest-for-a-fragment)<br>
## 1. New Project Creation Wizards
- Every `plugin, fragment, feature` and update site is represented by a single project in the workspace and allow PDE(Plugin Development Environment) to validate their manifest file(s). 
- `File > New > Project... > Plug-in Development`

- Use a Plugin Project if we're building new functionality.
- Use a Fragment Project if we need to modify an existing plugin without changing its core code.
## 1.1. Plugin project
- **A plugin project** (or "bundle") is the fundamental building block in an Eclipse-based application.

- **Use-cases:**
  - We are developing a primary feature of an application.
  - We need to define new APIs, extension points, or contribute UI components.

- **Manifest for a plugin project (OSGi bundle)**
```xml
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Name: Plugin Name
Bundle-SymbolicName: test.plugin.project;singleton:=true       # unique identifier for an plugin, The singleton:=true means only one instance of this plugin can be active in the runtime environment.
Bundle-Version: 1.0.0.qualifier                                # is mandatory and must be of the form major.minor.micro.qualifier
Bundle-Activator: test.plugin.Activator                        # used to control the bundle's life cycle
Bundle-Vendor: Vendor Name
Require-Bundle: org.eclipse.swt,                               # lists dependencies on other plugins, this means the current plugin depends on org.eclipse.swt, ... and can use all exported packages from it.
 org.eclipse.jface;bundle-version="[3.205.0,4.0.0)",
 org.eclipse.jdt.annotation;bundle-version="[2.2.0,3.0.0)";resolution:=optional # optional, if the bundle is missing, the plugin must still work without failing.
Import-Package: com.example.utils                              # this means the plugin only depends on specific packages, not the whole bundle.
Automatic-Module-Name: test.plugin.project
Bundle-ActivationPolicy: lazy                                  # Means the plugin will not be activated until needed
Export-Package: com.example.utils                              # Other plugins can use the classes in com.example.utils
Bundle-RequiredExecutionEnvironment: JavaSE-21                 # the Java version required to run the plugin. 
```

- **Versioning Rule** (Semantic Versioning - SemVer)
MAJOR version (X.0.0): Incompatible API changes (breaking changes).
MINOR version (X.Y.0): Backward-compatible feature additions.
PATCH version (X.Y.Z): Backward-compatible bug fixes.
<br>


## 1.2. Fragment project
- **A fragment** is an optional attachment to another plug-in. This other plug-in is called the host plug-in. At runtime the fragment is merged with its host plug-in.

- **Use-cases:**
  - Contain test classes: This way, tests can access the internal API of the plugin classes and test it. (Tests can be contained in own their `plugin project`, but they can only test the external API of the other plugin)
  - Contribute property files for additional translations.
  - Provide native code which is specific to certain operating systems (OS)
  - Contain resources like icon sets or other images

- **Manifest for a fragment project**

```xml
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Name: %fragmentName
Bundle-SymbolicName: org.eclipse.ui.win32
Bundle-Version: 3.4.300.qualifier # is mandatory and must be of the form major.minor.micro.qualifier
Bundle-ClassPath: .
Bundle-Vendor: %providerName
Fragment-Host: org.eclipse.ui.ide;bundle-version="[3.2.0,4.0.0)"
Bundle-Localization: fragment-win32
Export-Package: org.eclipse.ui.internal.editorsupport.win32;x-internal:=true
Eclipse-PlatformFilter: (osgi.ws=win32)
Bundle-RequiredExecutionEnvironment: JavaSE-21
Automatic-Module-Name: org.eclipse.ui.win32
```


