# ๐พ Zero Copy

## Zero Copy๋?
์ปดํจํฐ ์์คํ์์ CPU์ ๊ฐ์์ ๋ฐ์ง ์๊ณ  ํ ๋ฉ๋ชจ๋ฆฌ์ ์์ญ์์ ๋ค๋ฅธ ๋ฉ๋ชจ๋ฆฌ์ ์์ญ์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ์นดํผํ๋ ์์์ ๋งํ๋ค. ์ด๋ CPU ์ฌ์ดํด์ ์ ์ฝํ์ฌ์ ๋คํธ์ํฌ๋ SSD์ ๊ฐ์ด ๋น ๋ฅธ ๋ฐ์ดํฐ๊ฐ ํ์ํ ์์คํ์์ ์ ์ฉํ๊ฒ ์ด์ฉ๋๋ค.
  
## ๐ ์ผ๋ฐ์ ์ธ ํ์ผ ์ ์ก
- ํ์ผ ์๋ฒ๋ ์ ์  ํ์ผ์ ์๋น์คํ๋ ์น ์ ํ๋ฆฌ์ผ์ด์์ ๋์คํฌ์์ ํ์ผ ์ปจํ์ธ ๋ฅผ ์ฝ์ด ๋คํธ์ํฌ ์์ผ์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๋ ์ผ์ ๋ฐ๋ณต์ ์ผ๋ก ์ํํ๋ค.
- ์ด ๋์์ ์ด์์ฒด์  ๋ด๋ถ์์์ ๋ถํ์ํ ์ปจํ์คํธ ์ค์์นญ, ๋ฐ์ดํฐ ๋ณต์ฌ๊ฐ ์๋ฐ๋๋ค

์ผ๋ฐ์ ์ธ ์์ค์ฝ๋
```java
byte[] applicationBuffer = new byte[1024];
read(fildFd, applicationBuffer, len);
send(socketFd, applicationBuffer, len);

// ๋ฒํผ๋ฅผ ํ ๋น ๋ฐ๊ณ  ๋์คํฌ์์ ํ์ผ์ ๋ฒํผ๋ก ์ฝ์ด ๋ค์ฌ์ ๋ค์ ์์ผ์ผ๋ก ์ ๋ฌํ๋ ํํ 
// while ๋ฌธ์ ์ฌ์ฉํด ๋ ํฐ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ  ์ ์์ง๋ง ๊ธฐ๋ณธ์ ์ผ๋ก read() send() ์์คํ ์ฝ์ด ๋ฐ๋ณต๋๋ ํํ์ด๋ค.
```
![image](https://user-images.githubusercontent.com/71022555/164708479-0aa96e3e-c692-4078-a009-e5200ee0ba7b.png)  
์ผ๋ฐ์ ์ธ ์ํฉ์์์ ํ์ผ ์ ์ก(์ถ์ฒ : IBM ๋ฌธ์)

### ์คํ ๋ก์ง
1. ์ ์ ๊ฐ read() ์์คํ ์ฝ์ ํธ์ถํด ํ์ผ์ ์ฝ์ด๋ฌ๋ผ๊ณ  ์์ฒญํ๋ฉด, DMA ์์ง์ ์ํด์ ๋์คํฌ์ ์กด์ฌํ๋ ํ์ผ์ ๋ด์ฉ์ด ์ปค๋ ์ฃผ์๊ณต๊ฐ์ ์์นํ Read Buffer์ ๋ณต์ฌ๋๋ค.
2. ์ปค๋ ์ฃผ์๊ณต๊ฐ์ ์์นํ Read buffer๋ ์ฌ์ฉ์๊ฐ ์ ๊ทผํ  ์ ์๊ธฐ ๋๋ฌธ์ ์ฌ์ฉ์๊ฐ read() ํจ์ ํธ์ถ์ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ Application buffer์ Read buffer์ ๋ด์ฉ์ ๋ณต์ฌํด์ค๋ค. ๋ณต์ฌ ์๋ฃ ์ ํจ์ ํธ์ถ์์ return ํ๋ค.
3. ์ฌ์ฉ์๋ Application buffer๋ก ์ฝ์ด ๋ค์ธ ๋ฐ์ดํฐ๋ฅผ ์์ผ์ผ๋ก ์ ์กํ๊ธฐ ์ํด send() ํจ์๋ฅผ ํธ์ถํ๋ค. send() ํจ์ ํธ์ถ ์ Application buffer๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ๋ฉด ์ปค๋ ์์ญ์ ์์นํ Socket buffer๋ก ๋ฐ์ดํฐ๋ฅผ ๋ณต์ฌํ๋ค.
4. Socket buffer์ ์๋ ๋ฐ์ดํฐ๋ ์ค์  ๋คํธ์ํฌ๋ก ์ ์กํ๊ธฐ ์ํด ๋คํธ์ํฌ ์ฅ๋น(NIC)์ ์๋ buffer๋ก ๋ค์ ๋ณต์ฌ๋๋๋ค.
  
- ์ด๋ ๊ฒ 4๊ฐ์ ๋ฒํผ์ ๋์ผํ ๋ฐ์ดํฐ๊ฐ ๋ณต์ฌ๋๋ ์ด ๊ณผ์ ์ ์ ํ๋ฆฌ์ผ์ด์์ CPU ์์์ ์๋ชจํ๋ค. ์ด๋ฐ ๋ฒํผ๋ง์ ๋ฉ๋ชจ๋ฆฌ์ ๋์คํฌ, ๋ฉ๋ชจ๋ฆฌ์ ๋คํธ์ํฌ ์ฅ๋น ์ฌ์ด์ ์๋ ์ฐจ์ด๋ฅผ ๋งํํ๊ณ ์ ๋ง๋ค์ด์ง ์ฑ๋ฅ ๊ฐ์  ์ฅ์น์ด๋ค.  
- ์๋ฅผ ๋ค์ด 1KB ์ ๋์ ์์ ๋ฐ์ดํฐ๋ฅผ ํ์ผ์์ ์ฝ์ผ๋ฉด ์ปค๋์ 1KB ์ดํ ๋ฐ์ดํฐ๊น์ง ํ๋ฒ์ ์ฝ์ด์ ํ์ด์ง ์บ์์ ๋ก๋ํ๋ค. ์ด ํ, ๊ทธ ๋ค์ ๋ฐ์ดํฐ ์์ฒญ ์ ๋ฌผ๋ฆฌ์ ์ธ Input/Output์ ์ํํ์ง ์๊ณ  ํ์ด์ง ์บ์์์ ์ฝ์ด์ ์ฌ์ฉ์์๊ฒ ์ค๋ค. ๋ฐ์ดํฐ๋ฅผ ๋ฏธ๋ฆฌ ์ฝ์ด์ (Read Ahead) ์ฑ๋ฅ ๊ฐ์ ์ ๋๋ชจํ ๊ฒ  
- ์ด๋ฐ ๋ฒํผ๋ง์ด ์ฑ๋ฅ ์ ํ๋ฅผ ์ ๋ฐํ๋ ๋ณ๋ชฉ์ผ๋ก ์์ฉํ  ์ ์๋ค. ์ฌ์ฉ์๊ฐ ์์ฒญํ๋ ๋ฐ์ดํฐ์ ํฌ๊ธฐ๊ฐ ์ปค๋์ด ์ ์งํ๋ ๋ฒํผ์ ์ฌ์ด์ฆ๋ณด๋ค ํฐ ๊ฒฝ์ฐ ๋ฏธ๋ฆฌ ์ฝ๋(Read Ahead) ์ฑ๋ฅ ๊ฐ์  ํจ๊ณผ๋ณด๋ค ์ฌ๋ฌ ๋จ๊ณ์ ๊ฑธ์ณ ๋ฐ์ดํฐ๋ฅผ ๋ณต์ฌํ๋ ๋นํจ์จ์ด ๋ ์ปค์ง๊ฒ ๋๋ค.
  
## ๐ Zero Copy
๋ฆฌ๋์ค 2.2 ๋ฒ์ 
```c
#include  <sys/sendfile.h>
ssize_t endfile(int out_fd, int in_fd, off_t * offset ",size_t" " count" );
```
Java (nio ํจํค์ง์ transferTo(), transferFrom() ๋ฉ์๋๋ก ๊ตฌํ๋์ด์๋ค.)
```java
public void transferTo(long position, long count, WritableByteChannel target);

// transferTo(), transferFrom() ๋ฉ์๋ ์ญ์ sendfile() ์์คํ ์ฝ์ ์ด์ฉํด ๊ตฌํ๋์๋ค. 

toChannel.transferTo(position, to, toChannel);
```
![image](https://user-images.githubusercontent.com/71022555/164710687-697c868f-bb7b-41cc-a6b1-44a9a244ac0e.png)  
transferTo() ๋ฉ์๋๋ฅผ ์ด์ฉํ ํ์ผ์ ์ก(์ถ์ฒ : IBM ๋ฌธ์)
  
### ์คํ ๋ก์ง
1. ์ฌ์ฉ์๊ฐ transferTo() ๋ฉ์๋๋ฅผ ์ด์ฉํด ํ์ผ ์ ์ก์ ์์ฒญํ๋ค. read()์ send() ํจ์๊ฐ ํ๋๋ก ํฉ์ณ์ง ํํ์ ์์คํ ์ฝ์ด๋ค. read() ์์คํ ์ฝ๊ณผ ๋ง์ฐฌ๊ฐ์ง๋ก DMA ์์ง์ด ๋์คํฌ์์ ํ์ผ์ ์ฝ์ด ์ปค๋ ์ฃผ์ ๊ณต๊ฐ์ ์์นํ Read buffer๋ก ๋ฐ์ดํฐ๋ฅผ ๋ณต์ฌํ๋ค. 
2. ์ปค๋ ๋ชจ๋์์ ์ ์  ๋ชจ๋๋ก ์ปจํ์คํธ ์ค์์นญํ์ง ์๊ณ  ๋ฐ๋ก Socket buffer๋ก ๋ฐ์ดํฐ๋ฅผ ๋ณต์ฌํ๋ค. 
3. Socket buffer์ ๋ณต์ฌ๋ ๋ฐ์ดํฐ๋ฅผ DMA ์์ง์ ํตํด NIC buffer๋ก ๋ณต์ฌ๋์ด ์ง๋ค. 
4. ๋ฐ์ดํฐ๊ฐ ์ ์ก๋๊ณ  transferTo() ๋ฉ์๋์์ ๋ฆฌํดํ๋ค. 
  
์ปจํ์คํธ ์ค์์นญ์ด 4๋ฒ์์ 2๋ฒ์ผ๋ก ์ค์๋ค. transferTo() ๋ฉ์๋ ํธ์ถ์ ์ปค๋ ๋ชจ๋๋ก ํ๋ฒ, ์ข๋ฃ์ ์ ์ ๋ชจ๋๋ก ํ๋ฒ ์ด 2๋ฒ์ ์ปจํ์คํธ ์ค์์นญ์ด ๋ฐ์ํ๋ค.  
๋์๋ค์ด ์ ์  ์ฃผ์๊ณต๊ฐ์ ์๋ Application Buffer๋ก ๋ณต์ฌ๋์ด์ง์ง ์๊ธฐ ๋๋ฌธ์ ๋ฐ์ดํฐ์ ๋ณต์ฌ๋ณธ์ด 4๊ตฐ๋ฐ์์ 3๊ตฐ๋ฐ๋ก ์ค์ด๋ค์ด๋ค. ๋ฐ๋ผ์ ๋ฐ์ดํฐ ๋ณต์ฌ ๋์๋ ํ ๋ฒ ์ค์ด๋ค์๋ค.
  
## ๐ ๋ ๊ฐ์ ๋ Zero Copy
๋ฆฌ๋์ค ์ปค๋ 2.4 ์ดํ ๋ฒ์ ์์๋ NIC ์ฅ๋น๊ฐ "Gather Operation"์ ์ง์ํ  ๊ฒฝ์ฐ ๋ณต์ฌ๋ณธ์ ๋ ์ค์ผ ์ ์๊ฒ ๋์๋ค.  
![image](https://user-images.githubusercontent.com/71022555/164711166-90c403ed-82a3-4199-b5bd-8ca58213be7f.png)  
NIC์ ๋์์ ๋ฐ์ ์ ๋ก์นดํผ๊ฐ ์ ์ฉ๋ transferTo() ๋ฉ์๋(์ถ์ฒ : IBM ๋ฌธ์)
  
### ์คํ ๋ก์ง
์ปค๋ ๋ด๋ถ ๋์์ด ์ข ๋ ๊ฐ์ ๋ ๊ฒ
1. ์ฌ์ฉ์๊ฐ transferTo() ๋ฉ์๋๋ฅผ ํธ์ถํ๋ค. DMA ์์ง์ด ๋์คํฌ์์ ํ์ผ์ ์ฝ์ด ์ปค๋์ ์์นํ Read buffer๋ก ๋ฐ์ดํฐ๋ฅผ ๋ณต์ฌํ๋ค. ์ฌ๊ธฐ๊น์ง๋ ๋์ผํ๋ค. 
2. ๋ฐ์ดํฐ๊ฐ ์์ผ ๋ฒํผ๋ก ๋ณต์ฌ๋์ด์ง์ง๋ ์๋๋ค. ๋์  ๋ฐ์ดํฐ๊ฐ ์ ์ฅ๋ ์์น์ ๋ฐ์ดํฐ ์ฌ์ด์ฆ์ ๋ํ ์ ๋ณด์ ํจ๊ป ๋์คํฌ๋ฆฝํฐ(descriptor)๊ฐ ์์ผ ๋ฒํผ์ ์ถ๊ฐ๋๋ค. DMA ์์ง์ ์ด ์ ๋ณด๋ฅผ ์ด์ฉํด Read buffer์ ์๋ ๋ฐ์ดํฐ๋ฅผ NIC ๋ฒํผ์ ๋ฐ๋ก ๋ณต์ฌํ๊ณ , ๋คํธ์ํฌ๋ก ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๋ค.  

#### ์ด๋ฐ ์ถ๊ฐ์ ์ธ ์ต์ ํ๋ NIC์ gather operation๊ณผ ํ๋กํ ์ฝ์ checksum ๊ธฐ๋ฅ์ด ํ์ํ๋ค. TCP์ UDP์ ๊ฒฝ์ฐ Checksum ๊ธฐ๋ฅ์ด ๊ตฌํ๋์ด ์๋ค. NIC ์ฅ๋น์ "Gather operation" ์ง์์ ์ด์ ๋ stackoverflow์์ ํํธ๋ฅผ ์ป์ ์ ์๋ค. 
  
## ๐ ์ฑ๋ฅ์ ์ด์ 
|File size|Normal file transfer(ms)|tarnsferTo(ms)|
|:---:|:---:|:---:|
|7MB|156|45|
|21MB|337|128|
|63MB|843|387|
|98MB|1320|617|
|200MB|2124|1150|
|350MB|3631|1762|
|700MB|13498|4422|
|1GB|18399|8537|

## ๐ ์์ฝ
#### ๋์คํฌ์์ ํ์ผ์ ์ฝ์ ํ ๋ณ๋ค๋ฅธ ์ฒ๋ฆฌ ์์ด ๋ฐ๋ก ๋คํธ์ํฌ๋ก ์ ์กํ๋ ํ์ผ ์๋ฒ๋ ์ ์  ํ์ผ์ ์ ์กํ๋ ์น ์๋ฒ์ ๊ฒฝ์ฐ ์ ๋ก ์นดํผ๋ฅผ ์ฌ์ฉํ๋ฉด ์ฑ๋ฅ ๊ฐ์  ํจ๊ณผ๋ฅผ ์ป์ ์ ์๋ค. ํนํ ๋คํธ์ํฌ ์๋๊ฐ ๋งค์ฐ ๋นจ๋ผ์ ์๋ฒ์ ์ฑ๋ฅ์ ๋ณ๋ชฉ์ (bottleneck)์ด CPU๋ก ๋ชฐ๋ฆด ์๋ก ๋ถํ์ํ ๋ฐ์ดํฐ ๋ณต์ฌ๋ฅผ ์ ๊ฑฐํด ์ฑ๋ฅ ๊ฐ์ ์ ์ป์ ์ ์๋ค. 
  
## ์์ค
### ์ผ๋ฐ์ ์ธ ํ์ผ ์ ์ก
ํด๋ผ์ด์ธํธ
```java
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.Socket;
import java.net.UnknownHostException;

public class TraditionalClient {
    
    
    
    public static void main(String[] args) {

	int port = 2000;
	String server = "localhost";
	Socket socket = null;
	String lineToBeSent;
	
	DataOutputStream output = null;
	FileInputStream inputStream = null;
	int ERROR = 1;
	
	
	// connect to server
	try {
	    socket = new Socket(server, port);
	    System.out.println("Connected with server " +
				   socket.getInetAddress() +
				   ":" + socket.getPort());
	}
	catch (UnknownHostException e) {
	    System.out.println(e);
	    System.exit(ERROR);
	}
	catch (IOException e) {
	    System.out.println(e);
	    System.exit(ERROR);
	}
	
	try {
		String fname = "sendfile/NetworkInterfaces.c";
		inputStream = new FileInputStream(fname);
		
	    output = new DataOutputStream(socket.getOutputStream());
	    long start = System.currentTimeMillis();	    
	    byte[] b = new byte[4096];
	    long read = 0, total = 0;
	    while((read = inputStream.read(b))>=0) {
		total = total + read;
	    	output.write(b);
	    }
	    System.out.println("bytes send--"+total+" and totaltime--"+(System.currentTimeMillis() - start));
	}
	catch (IOException e) {
	    System.out.println(e);
	}

	try {
		output.close();
	    socket.close();
	    inputStream.close();
	}
	catch (IOException e) {
	    System.out.println(e);
	}
    }    
}
```
์๋ฒ
```java
import java.net.*;
import java.io.*;

public class TraditionalServer {
    
    public static void main(String args[]) {
	
	int port = 2000;
	ServerSocket server_socket;
	DataInputStream input;
	
	try {
	    
	    server_socket = new ServerSocket(port);
	    System.out.println("Server waiting for client on port " + 
			       server_socket.getLocalPort());
	    
	    // server infinite loop
	    while(true) {
		Socket socket = server_socket.accept();
		System.out.println("New connection accepted " +
				   socket.getInetAddress() +
				   ":" + socket.getPort());
		input = new DataInputStream(socket.getInputStream()); 
		// print received data 
		try {
			byte[] byteArray = new byte[4096];
		    while(true) {
		    	int nread = input.read(byteArray , 0, 4096);
		    	if (0==nread) 
		    		break;
		    }
		}
		catch (IOException e) {
		    System.out.println(e);
		}
		
		// connection closed by client
		try {
		    socket.close();
		    System.out.println("Connection closed by client");
		}
		catch (IOException e) {
		    System.out.println(e);
		}
		
	    }
	    
	    
	}
	
	catch (IOException e) {
	    System.out.println(e);
	}
    }
}
```
### Zero Copy
ํด๋ผ์ด์ธํธ
```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.SocketAddress;
import java.nio.channels.FileChannel;
import java.nio.channels.SocketChannel;

public class TransferToClient {
	
	public static void main(String[] args) throws IOException{
		TransferToClient sfc = new TransferToClient();
		sfc.testSendfile();
	}
	public void testSendfile() throws IOException {
	    String host = "localhost";
	    int port = 9026;
	    SocketAddress sad = new InetSocketAddress(host, port);
	    SocketChannel sc = SocketChannel.open();
	    sc.connect(sad);
	    sc.configureBlocking(true);

	    String fname = "sendfile/NetworkInterfaces.c";
	    long fsize = 183678375L, sendzise = 4094;
	    
	    // FileProposerExample.stuffFile(fname, fsize);
	    FileChannel fc = new FileInputStream(fname).getChannel();
            long start = System.currentTimeMillis();
	    long nsent = 0, curnset = 0;
	    curnset =  fc.transferTo(0, fsize, sc);
	    System.out.println("total bytes transferred--"+curnset+" and time taken in MS--"+(System.currentTimeMillis() - start));
	    //fc.close();
	  }
}
```
์๋ฒ
```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ServerSocket;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;


public class TransferToServer  {
  ServerSocketChannel listener = null;
  protected void mySetup()
  {
    InetSocketAddress listenAddr =  new InetSocketAddress(9026);

    try {
      listener = ServerSocketChannel.open();
      ServerSocket ss = listener.socket();
      ss.setReuseAddress(true);
      ss.bind(listenAddr);
      System.out.println("Listening on port : "+ listenAddr.toString());
    } catch (IOException e) {
      System.out.println("Failed to bind, is port : "+ listenAddr.toString()
          + " already in use ? Error Msg : "+e.getMessage());
      e.printStackTrace();
    }

  }

  public static void main(String[] args)
  {
    TransferToServer dns = new TransferToServer();
    dns.mySetup();
    dns.readData();
  }

  private void readData()  {
	  ByteBuffer dst = ByteBuffer.allocate(4096);
	  try {
		  while(true) {
			  SocketChannel conn = listener.accept();
			  System.out.println("Accepted : "+conn);
			  conn.configureBlocking(true);
			  int nread = 0;
			  while (nread != -1)  {
				  try {
					  nread = conn.read(dst);
				  } catch (IOException e) {
					  e.printStackTrace();
					  nread = -1;
				  }
				  dst.rewind();
			  }
		  }
	  } catch (IOException e) {
		  e.printStackTrace();
	  }
  }
}
```
---
์ฐธ๊ณ  ์๋ฃ  
https://free-strings.blogspot.com/2016/04/zero-copy.html
https://soft.plusblog.co.kr/7
https://gist.github.com/freestrings