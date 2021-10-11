# Groovy-Examples
Groovy code examples

`println System.getProperty("user.dir")` print current working directory  
`println "ls -la".execute().text` execute shell command  

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
