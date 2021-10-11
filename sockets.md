# Sockets 

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

## Whois

```groovy
def domainName = "example.me"
def whoisServer = "whois.nic.me"
int port = 43

def s = new Socket(whoisServer, port)

s.setSoTimeout(18000)

s.withStreams { sin, sout ->

	sout << domainName << "\r\n"
	
	def text = sin.text
	println text
}

s.close()
```
