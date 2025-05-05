---
title: BufferedOutputStream 252490 job4j
tags:
  - IO-NIO
  - Job4j
related_topics: 
created: 2025-01-20 10:48
modified: 2025-01-20T10:51:54+03:00
questions: 
notes: 
links: 
---



```java
package ru.job4j.io;

import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.PrintWriter;

public class ResultFile {
    public static void main(String[] args) {
        try (PrintWriter output = new PrintWriter(
                new BufferedOutputStream(
                        new FileOutputStream("data/dataresult.txt")
                ))) {
            output.println("Hello, world!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
![[Pasted image 20250120104914.png]]

![[Pasted image 20250120105143.png]]

