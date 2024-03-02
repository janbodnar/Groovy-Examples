# Date and time 

## Current datetime 

```groovy
import java.time.LocalDate
import java.time.Month
import java.time.MonthDay
import java.time.YearMonth


def date = LocalDate.now()
println("Current Date: ${date}")

def date2 = LocalDate.of(2017, Month.NOVEMBER, 13)
println("Date from specified date: ${date2}")

// current year and month
def yearMo = YearMonth.now()
println("Current Year and month: ${yearMo}")

def specifiedDate = YearMonth.of(2000, Month.NOVEMBER)
println("Specified Year-Month: ${specifiedDate}")

def monthDay = MonthDay.now()
println("Current month and day: ${monthDay}")

def specifiedDate2 = MonthDay.of(Month.NOVEMBER, 11)
println("Specified Month-Day: ${specifiedDate2}")
```


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

## Get age

We can use `Period.between` to get the person's age from the date of birth.  

```groovy
import java.time.LocalDate
import java.time.Period

def dobs = [
    LocalDate.parse('1973-09-07', 'yyyy-MM-dd'),
    LocalDate.parse('1963-03-30', 'yyyy-MM-dd'),
    LocalDate.parse('1980-05-12', 'yyyy-MM-dd'),
    LocalDate.parse('1983-03-30', 'yyyy-MM-dd'),
    LocalDate.parse('2009-03-06', 'yyyy-MM-dd'),
    LocalDate.parse('1978-11-16', 'yyyy-MM-dd'),
    LocalDate.parse('1986-10-23', 'yyyy-MM-dd')
]

def now = LocalDate.now()

for (def dob in dobs) {
    
    int age = Period.between(dob, now).getYears()
    println("age: ${age}")
}
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

## ZonedDateTime

The example prints the current datetime in Bratislava and Moscow, and the UTC datetime.

```groovy
import java.time.ZoneId
import java.time.ZoneOffset
import java.time.ZonedDateTime

def zbrat = ZonedDateTime.now(ZoneId.of("Europe/Bratislava"))
def zmosc = ZonedDateTime.now(ZoneId.of("Europe/Moscow"))

println(zbrat)
println(zmosc)

def utc = ZonedDateTime.now(ZoneOffset.UTC)
println(utc)
```


## Temporal adjusters

Temporal adjusters are used for modifying temporal objects (LodalDate,  
LodalDateTime, HijrahDate ) They allow to easily calculate calculations such  
as finding first day of week, moth, year etc.  

```groovy
import java.time.DayOfWeek
import java.time.LocalDate
import java.time.temporal.TemporalAdjusters

def ld = LocalDate.now()
println("today: ${ld}")

def date1 = ld.with(TemporalAdjusters.firstDayOfMonth())
println("first day of month: ${date1}")

def date2 = ld.with(TemporalAdjusters.lastDayOfMonth())
println("last day of month: ${date2}")

def date3 = ld.with(TemporalAdjusters.next(DayOfWeek.MONDAY))
println("next Monday: ${date3}")

def date4 = ld.with(TemporalAdjusters.firstDayOfNextMonth())
println("first day of next month: ${date4}")

def date5 = ld.with(TemporalAdjusters.lastDayOfYear())
println("last day of year: ${date5}")

def date6 = ld.with(TemporalAdjusters.firstDayOfYear())
println("first day of year: ${date6}")

def date7 = ld.with(TemporalAdjusters.lastInMonth(DayOfWeek.SUNDAY))
println("last Sunday of month: ${date7}")
```


