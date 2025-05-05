---
title: Что такое Socket? 4850 job4j
tags:
  - IO-NIO
  - Job4j
related_topics: 
created: 2025-01-22 16:20
modified: 2025-01-22T16:30:01+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20250122162111.png]]



#### СЕРВЕР

```java
package ru.job4j.io;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class EchoServer {
    public static void main(String[] args) throws IOException {
        try (ServerSocket server = new ServerSocket(9000)) {
            while (!server.isClosed()) {
                Socket socket = server.accept();
                try (OutputStream output = socket.getOutputStream();
                     BufferedReader input = new BufferedReader(
                             new InputStreamReader(socket.getInputStream()))) {
                    output.write("HTTP/1.1 200 OK\r\n\r\n".getBytes());
                    for (String string = input.readLine(); string != null && !string.isEmpty(); string = input.readLine()) {
                        System.out.println(string);
                    }  
                    output.flush();
                }
            }
        }
    }
}
```

![[Pasted image 20250122162319.png]]

![[Pasted image 20250122162331.png]]
![[Pasted image 20250122162942.png]]


----
