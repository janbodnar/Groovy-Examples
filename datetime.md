# Date and time 


## Instant

```groovy
import java.time.Instant
import java.time.ZoneId
import java.time.temporal.ChronoUnit

def now = Instant.now()

println("The current now: " + now)
println("Unix time: ${now.toEpochMilli()}")

//Now minus five days
def minusFive = now.minus(5, ChronoUnit.DAYS)
println("Now minus five days: ${minusFive}")

def atZone = now.atZone(ZoneId.of("GMT"))
println("GMT: ${atZone}")

def yesterday = Instant.now().minus(24, ChronoUnit.HOURS)
println("Yesterday: ${yesterday}")
```
