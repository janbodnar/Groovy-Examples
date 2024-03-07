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

## Planets

```groovy
double earthWeight = 63
double mass = earthWeight / Planet.EARTH.surfaceGravity()

for (Planet p : Planet.values()) {

    println("Your weight on ${p} is ${p.surfaceWeight(mass)}")
}

enum Planet {

    MERCURY (3.303e+23, 2.4397e6),
    VENUS   (4.869e+24, 6.0518e6),
    EARTH   (5.976e+24, 6.37814e6),
    MARS    (6.421e+23, 3.3972e6),
    JUPITER (1.9e+27,   7.1492e7),
    SATURN  (5.688e+26, 6.0268e7),
    URANUS  (8.686e+25, 2.5559e7),
    NEPTUNE (1.024e+26, 2.4746e7);

    private final double mass   // in kilograms
    private final double radius // in meters

    Planet(double mass, double radius) {
        this.mass = mass
        this.radius = radius
    }

    private double mass() { return mass }
    private double radius() { return radius }

    // universal gravitational constant  (m3 kg-1 s-2)
    final double G = 6.67300E-11

    double surfaceGravity() {
        return G * mass / (radius * radius)
    }

    double surfaceWeight(double otherMass) {        
        return otherMass * surfaceGravity()
    }

    String toString() {
        name().toLowerCase().capitalize()
        // "${name().charAt(0)}${name().substring(1).toLowerCase()}"
    }
}
```




