---
author: "Phong Nguyen"
title: "Java Basic IO"
date: "2024-11-14"
description: "Java: Basic I/O."
tags: ["java"]   #tags search
FAcategories: ["syntax"]    #The category of the post, similar to tags but usually for broader classification.
FAseries: ["Themes Guide"]    #indicates that this post is part of a series of related posts
aliases: ["migrate-from-jekyl"]    #Alternative URLs or paths that can be used to access this post, useful for redirects from old posts or similar content.
ShowToc: true    # Determines whether to display the Table of Contents (TOC) for the post.
TocOpen: true    # Controls whether the TOC is expanded when the post is loaded. 
weight: 4    # The order in which the post appears in a list of posts. Lower numbers make the post appear earlier.
---
Explain how to use basic I/O.
**References:** 
[Basic I/O](https://docs.oracle.com/javase/tutorial/essential/io/index.html)<br>
[Reading and writing files in Java (Input/Output) - Tutorial](https://www.vogella.com/tutorials/JavaIO/article.html)<br>
## 1. I/O Streams
- An I/O Stream represents and input source or an output destination.
- A stream is a sequence of data.
- `input stream`: is used to **read** data from source, once item at a time.
- `outputs stream`: is used to **write** data to a destination, once item |at a time.
```
--stream-->
0101010101...
---------->
```
- **java.io** package

### 1.1. Bytes Streams
- Bytes Streams is used to perform input and output of 8-bit bytes.
- All other stream types are built on byte streams.
- Examples:
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyBytes {
    public static void main(String[] args) throws IOException {

        FileInputStream in = null;
        FileOutputStream out = null;

        try {
            in = new FileInputStream("xanadu.txt");
            out = new FileOutputStream("outagain.txt");
            int c;

            while ((c = in.read()) != -1) {
                out.write(c);
            }
        } finally {
            if (in != null) {
                in.close();
            }
            if (out != null) {
                out.close();
            }
        }
    }
}

```

### 1.2. Character Streams
- Character values is stored using Unicode conventions.
- Character stream I/O **automatically** translates this internal format to and from the local character set.( E.g: Western: ASCII, Japan:Shift-JIS)
- Example:
```java
	private boolean writeXmlFile(Map<String, String> object, IPath filePath) {
		try (FileOutputStream outputStream = new FileOutputStream(filePath.toFile())) {
			XMLStreamWriter writer = XMLOutputFactory.newFactory().createXMLStreamWriter(outputStream,
					StandardCharsets.UTF_8.name());
			writer.writeStartDocument(StandardCharsets.UTF_8.name(), "1.0"); //$NON-NLS-1$
			writer.writeStartElement("A");
			writer.writeAttribute("xmlns:i", "http://www.w3.org/2001/XMLSchema-instance");
			writer.writeAttribute("xmlns", "http://schemas.microsoft.com/2003/10/Serialization/Arrays");
			for (Map.Entry<String, String> entry : object.entrySet()) {
				writer.writeStartElement("A");
				writer.writeStartElement("A");
				writer.writeCharacters("A");
				writer.writeEndElement();
				writer.writeStartElement("A");
				writer.writeCharacters("A");
				writer.writeEndElement();
				writer.writeEndElement();
			}
			writer.writeEndElement();
			writer.writeEndDocument();
			writer.flush();
			return true;
		} catch (IOException | XMLStreamException e) {
			System.out("Error writing XML file", e); //$NON-NLS-1$
		}
		return false;
	}
```
