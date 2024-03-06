# Enum type

## ordinal, values

```groovy
enum Size {
    SMALL, MEDIUM, LARGE
}

println Size.SMALL

Size.each { println it }
Size.each { println "${it} ${it.ordinal()}" }

println Size.values()
```

## String coercion

```groovy

enum State {
    up,
    down
}

println State.up == 'up' as State
println State.down == 'down' as State

State s1 = 'up'
State s2 = 'down'

println State.up == s1
println State.down == s2
```

## Custom method

```groovy
import java.util.Random

def season = Season.randomSeason()

String msg = switch (season) {

    case Season.SPRING -> "Spring"
    case Season.SUMMER -> "Summer"
    case Season.AUTUMN -> "Autumn"
    case Season.WINTER -> "Winter"
}

println(msg)


enum Season {
    SPRING,
    SUMMER,
    AUTUMN,
    WINTER;

    static Season randomSeason() {

        def random = new Random()
        int ridx = random.nextInt(Season.values().length)
        Season.values()[ridx]
    }
}
```


## Switch expression ranges

```groovy
enum Day {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}

def isWeekend(Day d) {
    
    switch (d) {
        case Day.Monday..Day.Friday -> false 
        case Day.Saturday, Day.Sunday -> true
    }
}

def days = [ Day.Monday, Day.Tuesday, Day.Wednesday, Day.Thursday, 
    Day.Friday, Day.Saturday, Day.Sunday ]

for (e in days) {

    if (isWeekend(e)) {
        println('weekend')
    } else {
        println('weeday')
    }
}
```
