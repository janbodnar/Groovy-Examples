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

## Parsing datetime as strings

```groovy
import java.time.LocalDate
import java.time.LocalDateTime
import java.time.format.DateTimeFormatter

String sd1 = '2023-01-04'
LocalDate ld1 = LocalDate.parse(sd1)
println("Date: ${ld1}")

String sdatetime = '2022-08-16T16:34:10'
LocalDateTime ldt = LocalDateTime.parse(sdatetime)
println("Datetime: ${ldt}")

// If we do not specify the Locale.ENGLISH, the example fails
DateTimeFormatter dtfmt = DateTimeFormatter.ofPattern('dd MMM uuuu', Locale.ENGLISH)
String anotherDate = '14 Aug 2017'
LocalDate lds = LocalDate.parse(anotherDate, dtfmt)
println("${anotherDate} parses to ${lds}")
```

It may be necessary to specify the locale.  

