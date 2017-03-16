**MulticastReceiver**
```java
import java.io.*;
import java.net.*;

public class MulticastReceiver {
  public static void main(String[] args) {
    DatagramPacket inPacket = null;
    byte[] inBuf = new byte[256];
    try {
      //Prepare to join multicast group
      MulticastSocket socket = new MulticastSocket(8888);
      InetAddress address = InetAddress.getByName("224.2.2.3");
      socket.joinGroup(address);
 
      while (true) {
        inPacket = new DatagramPacket(inBuf, inBuf.length);
        socket.receive(inPacket);
        String msg = new String(inBuf, 0, inPacket.getLength());
        System.out.println("From " + inPacket.getAddress() + " Msg : " + msg);
      }
    } catch (IOException ioe) {
      System.out.println(ioe);
    }
  }
}
```

**MulticastSender**
```java
import java.io.*;
import java.net.*;

public class MulticastSender {
  public static void main(String[] args) {
    byte[] outBuf;
    final int PORT = 8888;
 
    try {
      DatagramSocket socket = new DatagramSocket();
      long counter = 0;
      String msg;
 
      while (true) {
        msg = "This is multicast! " + counter;
        counter++;
        outBuf = msg.getBytes();
 
        //Send to multicast IP address and port
        InetAddress address = InetAddress.getByName("224.2.2.3");
        DatagramPacket outPacket = new DatagramPacket(outBuf, outBuf.length, address, PORT);
 
        socket.send(outPacket);
 
        System.out.println("Server sends : " + msg);
        try {
          Thread.sleep(500);
        } catch (InterruptedException ie) {
        }
      }
    } catch (IOException ioe) {
      System.out.println(ioe);
    }
  }
}
```