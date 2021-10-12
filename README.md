# Groovy-Examples
Groovy code examples

`println System.getProperty("user.dir")` print current working directory  
`println "ls -la".execute().text` execute shell command  

```groovy
def isOdd = { int i -> (i & 1) as boolean }
def isEven = {int i -> ! isOdd(i) }
```
even/odd  

`def evens = [1, 2, 3, 4, 5].findAll{ it % 2 == 0 }` - filter out even values  

grepping/filtering  

```groovy
def res = [1, 15, 16, 30, 12].grep(12..18)
println res

def res2 = [1, 2, -1, 0, 2, 3, -3, 4].grep({it > 0})
println res2

def words = ['sky', 'cloud', 'war', 'wasp', 'water', 'coin']
def res3 = words.grep(~/.../)
println res3
```

Web scraping with JSoup  

```groovy
@Grab(group='org.jsoup', module='jsoup', version='1.10.1')
import org.jsoup.Jsoup

def doc = Jsoup.connect("http://webcode.me").get()
def title = doc.select('title')
def elements = doc.select('*')

println doc
println title

elements.each { e ->
    println e
}
```

Read file line by line  

```groovy
def fname = new File("words.txt")

fname.eachLine { ln, n ->
    println "line $n: $ln"
}
```

Fetch all rows from db  

```groovy
@Grab(group='org.postgresql', module='postgresql', version='42.2.24')
@GrabConfig(systemClassLoader=true)

import groovy.sql.Sql

def url = 'jdbc:postgresql://localhost:5432/testdb'
def user = 'postgres'
def password = ''
def driver = 'org.postgresql.Driver'

Sql.withInstance(url, user, password, driver) { sql ->

    def res = sql.rows('SELECT * FROM cars')
    println res
}
```
# Find all text files  

```groovy
import static groovy.io.FileType.FILES

def f = new File('/home/jano7/Documents')

f.eachFileRecurse(FILES) {
    if(it.name.endsWith('.txt')) {
        println it
    }
}
```

```groovy
import static groovy.io.FileType.FILES

def f = new File('/home/jano7/Documents')

f.traverse(type: FILES, nameFilter: ~/.*.txt/) { it ->
	println it
}
```

The *nameFilter* is an exact match   

## Iterate over range of datetime values

```groovy
@Grab('org.codehaus.groovy:groovy-datetime:3.0.9')

import java.time.Instant
import java.time.temporal.ChronoUnit

def now = Instant.now()
println now

def d2 = now.plus(3, ChronoUnit.DAYS)

println "--------------"

now.upto(d2, ChronoUnit.DAYS) {
	
	println it
}
```
