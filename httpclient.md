# Groovy HttpClient examples 

## Status 

```groovy
import java.net.URI
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse

def client = HttpClient.newHttpClient()
def request = HttpRequest.newBuilder()
        .uri(URI.create("http://webcode.me"))
        .GET() // GET is default
        .build()

HttpResponse<Void> res = client.send(request,
        HttpResponse.BodyHandlers.discarding())

println res.statusCode()
```

## GET request

```groovy
import java.net.URI
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse

def client = HttpClient.newHttpClient()
def req = HttpRequest.newBuilder()
        .uri(URI.create("http://webcode.me"))
        .build()

def res = client.send(req,
        HttpResponse.BodyHandlers.ofString())

println res.body()
```

## POST request

```groovy
import java.net.URI
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse
import groovy.json.JsonOutput

def vals = ["name": "John Doe", "occupation": "gardener"]
def body = JsonOutput.toJson(vals)

def client = HttpClient.newHttpClient()
def request = HttpRequest.newBuilder()
        .uri(URI.create("https://httpbin.org/post"))
        .POST(HttpRequest.BodyPublishers.ofString(body))
        .build()

def res = client.send(request, HttpResponse.BodyHandlers.ofString())

println res.body()
```

## HEAD request

```groovy
import java.net.URI
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse

def client = HttpClient.newHttpClient()

var request = HttpRequest.newBuilder(URI.create("http://webcode.me"))
        .method("HEAD", HttpRequest.BodyPublishers.noBody())
        .build()

def res = client.send(request,
        HttpResponse.BodyHandlers.discarding())

def headers = res.headers()

println headers 

headers.map().each { k, v  -> 

   println "$k: $v"
}
```

## Process JSON

```groovy
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse

import groovy.json.JsonSlurper
import groovy.json.JsonParserType

def url = "http://webcode.me/users.json"

def client = HttpClient.newHttpClient()
def request = HttpRequest.newBuilder()
        .uri(URI.create(url))
        .build()

HttpResponse<String> res = client.send(request,
        HttpResponse.BodyHandlers.ofString())


def body = res.body()

def data = new JsonSlurper().parseText(body)
def users = data["users"]

for (user: users)
{
    println "${user.id} ${user.first_name} ${user.last_name} ${user.email}"
}
```

Get and process JSON resource

## Proxy

```groovy
import java.net.URI
import java.net.ProxySelector
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse

def port = 7878
def url = "http://webcode.me"
def proxy = '143.208.200.26'

def client = HttpClient.newBuilder()
        .version(HttpClient.Version.HTTP_2)
        .proxy(ProxySelector.of(new InetSocketAddress(proxy, port)))
        .build()

def req = HttpRequest.newBuilder()
        .uri(URI.create(url))
        .build()

def res = client.send(req, HttpResponse.BodyHandlers.ofString())
println res.body()
```

## Multiple async requests

```groovy
import java.util.concurrent.Executors
import java.util.concurrent.TimeUnit

import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpResponse

int nThreads = 30

def executor = Executors.newFixedThreadPool(nThreads)

def urls = [
    "https://crunchify.com",
    "https://yahoo.com",
    "https://www.ebay.com",
    "https://google.com",
    "https://www.example.co",
    "https://paypal.com",
    "http://bing.com/",
    "https://techcrunch.com/",
    "http://mashable.com/",
    "https://pro.crunchify.com/",
    "https://wordpress.com/",
    "https://wordpress.org/",
    "https://example.com/",
    "https://sjsu.edu/",
    "https://ask.crunchify.com/",
    "https://test.com.au/",
    "https://www.wikipedia.org/",
    "https://en.wikipedia.org"
]

for (String url in urls ) {

    executor.execute(() -> {

        worker(url)

        // try {
        //     worker(url)
        // } catch (Exception e) {
        //     e.printStackTrace()
        // }

    })
}

executor.shutdown()

executor.awaitTermination(30, TimeUnit.SECONDS)
println("finished")

def worker(url) {

    def client = HttpClient.newHttpClient()
    def request = HttpRequest.newBuilder()
        .uri(URI.create(url))
        .build()

    HttpResponse<Void> res = client.send(request,
            HttpResponse.BodyHandlers.discarding())

    println "${url}: ${res.statusCode()}"
}
```

