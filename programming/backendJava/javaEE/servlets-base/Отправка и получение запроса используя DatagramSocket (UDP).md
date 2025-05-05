---
title: Отправка и получение запроса используя DatagramSocket (UDP)
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:55
modified: 2024-09-11T13:55:54+03:00
questions: 
notes: 
links: 
---
### Отправка и получение запроса используя DatagramSocket (UDP)

При отправке данных по протоколу UDP, необходимо регламентировать размер принимающего баффера, иначе есть возможность потерять часть отправленных данных.

```Java
package ru.konovalov;

import java.io.IOException;
import java.net.*;

public class DatagramRunner {
    public static void main(String[] args) throws IOException {
        InetAddress inetAddress = InetAddress.getByName("localhost");
        try(DatagramSocket socket  = new DatagramSocket()){
            var bytes = "Hello from UDP client".getBytes();
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length, inetAddress, 7777);
            socket.send(packet);
        }
    }
}
```

```Java
package ru.konovalov;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class DatagramServerRunner {
    public static void main(String[] args) throws IOException {
        try(DatagramSocket server = new DatagramSocket(7777)){
            byte[] buffer = new byte[512];
            DatagramPacket datagramPacket = new DatagramPacket(buffer, buffer.length);
            server.receive(datagramPacket);
            String data = new String(buffer);
            System.out.println(data);
        }
    }
}
```
