---
title: Отправка и получение запроса c помощью Socket и ServerSocket (TCP)
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:54
modified: 2024-09-13T18:12:06+03:00
questions: 
notes: 
links: 
---

### Отправка и получение запроса на собственный сервер c помощью Socket и ServerSocket (TCP)

Сначала необходимо запустить ServerSocket, потом Socket, чтобы установить соединение по порту. Иначе выбросится исключение, так как это TCP протокол:

```Java
Exception in thread "main" java.net.ConnectException: В соединении отказано
```

```Java
package ru.konovalov;

import java.io.DataOutputStream;
import java.io.DataInputStream;
import java.io.IOException;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

public class SocketRunner {
    public static void main(String[] args) throws IOException {
        var inetAddress = InetAddress.getByName("localhost");
        try (Socket socket = new Socket(inetAddress, 7777);
             var outputStream = new DataOutputStream(socket.getOutputStream());
             var inputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            while (scanner.hasNextLine()){
                String request = scanner.nextLine();
                outputStream.writeUTF(request);
                System.out.println("Response from server: " + inputStream.readUTF());
            }

        }
    }
}
```

![[Untitled 41.png]]

```Java
package ru.konovalov;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class SocketServerRunner {
    public static void main(String[] args) throws IOException {
        try (ServerSocket serverSocket = new ServerSocket(7777);
             Socket socket = serverSocket.accept();
             DataOutputStream dataOutputStream = new DataOutputStream(socket.getOutputStream());
             DataInputStream dataInputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            var request = dataInputStream.readUTF();
            while (!"stop".equals(request)) {
                System.out.println("Client request: " + request);
                var response = scanner.nextLine();
                dataOutputStream.writeUTF(response);
                request = dataInputStream.readUTF();
            }
        }
    }
}
```

![[Untitled 42.png]]

![[Untitled 43.png]]

Если в консоль Socket написать `"stop",` то получим исключение, так как у нас TCP соединение и мы не получим response от ServerSocket, а должны.

```Java
Exception in thread "main" java.io.EOFException
at java.base/java.io.DataInputStream.readUnsignedShort(DataInputStream.java:346)
at java.base/java.io.DataInputStream.readUTF(DataInputStream.java:595)
at java.base/java.io.DataInputStream.readUTF(DataInputStream.java:570)
at ru.konovalov.SocketRunner.main(SocketRunner.java:20)
```

Поэтому необходимо обработать случай закрытия соединения:

```Java
package ru.konovalov;

import java.io.DataOutputStream;
import java.io.DataInputStream;
import java.io.IOException;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

public class SocketRunner {
    public static void main(String[] args) throws IOException {
        var inetAddress = InetAddress.getByName("localhost");
        try (Socket socket = new Socket(inetAddress, 7777);
             var outputStream = new DataOutputStream(socket.getOutputStream());
             var inputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            do {

                String request = scanner.nextLine();
                outputStream.writeUTF(request);
                String repsonse = inputStream.readUTF();
                System.out.println("Response from server: " + repsonse);
                if (repsonse.equals("Close connection.")) {
                    break;
                }
            } while (scanner.hasNextLine());

        }
    }
}
```

```Java
package ru.konovalov;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class SocketServerRunner {
    public static void main(String[] args) throws IOException {
        try (ServerSocket serverSocket = new ServerSocket(7777);
             Socket socket = serverSocket.accept();
             DataOutputStream dataOutputStream = new DataOutputStream(socket.getOutputStream());
             DataInputStream dataInputStream = new DataInputStream(socket.getInputStream());
             Scanner scanner = new Scanner(System.in)) {
            var request = dataInputStream.readUTF();

                do {
                    System.out.println("Client request: " + request);
                    var response = scanner.nextLine();
                    dataOutputStream.writeUTF(response);
                    request = dataInputStream.readUTF();
                } while (!"stop".equals(request));
                    var response = "Close connection.";
                    dataOutputStream.writeUTF(response);
        }
    }
}
```

