# Sockets 

## HEAD request

```groovy
def s = new Socket("webcode.me", 80);

s.withStreams { sin, sout ->
  sout << "HEAD / HTTP/1.1\r\nConnection: close\r\nHost: webcode.me\r\nAccept: text/html\r\n\r\n"  // send request first
  def reader = sin.newReader()
  def lines = reader.readLines()
  
  for (line in lines) {
	  println line
  }
}

s.close();
```

## GET request 

```groovy
def host = "webcode.me"
int port = 80

def s = new Socket(host, port)

s.setSoTimeout(18000)

s.withStreams { sin, sout ->

	sout << "GET / HTTP/1.1\r\n"
	sout << "Connection: close\r\n"
	sout << "Host: www.webcode.me\r\n\r\n"

	def text = sin.text
	println text
}

s.close()
```

## Time client 

```groovy
def hostname = "3.se.pool.ntp.org"
int port = 13

def s = new Socket(hostname, port)
println s.getSoTimeout()

s.setSoTimeout(3000)

s.withStreams { sin, sout ->

	s << ""
	def reader = sin.newReader()
	def line = reader.readLine()
	println "$line"
}

s.close()
```

## Whois request

```groovy
package com.zetcode

def hostname = "3.se.pool.ntp.org"
int port = 13

def s = new Socket(hostname, port)
s.setSoTimeout(6000)

s.withStreams { sin, sout ->

	sout << ""
	
	def text = sin.text
	println text
}

s.close()
```
