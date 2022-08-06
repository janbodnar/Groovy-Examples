# Groovy switch expressions

Also called pattern matching, switch expressions provide a concise and powerful  
way of creating conditions in multiple arms. In Groovy, switch expressions return  
values.  

## Matching types 

```groovy
def data = [1, 2.2, 'falcon', true, [1, 2, 3], 2g]

for (e in data) {

    def res = switch (e) {

        case Integer -> 'integer'
        case String -> 'string'
        case Boolean -> 'boolean'
        case List -> 'list'
        default -> 'other'
    }

    println res
}
```

## Multiple options 

```groovy
def grades = ['A', 'B', 'C', 'D', 'E', 'F', 'FX']

for (grade in grades) {

    switch (grade) {
        case 'A' , 'B' , 'C' , 'D' , 'E' , 'F' -> println('passed')
        case 'FX' -> println('failed')

    }
}
```

## Default option

```groovy
def factorial(n) {

    switch (n) {
        case 0, 1 -> 1
        default -> n * factorial(n - 1)
    }
}

for (i in 0g..22g) {
    def f = factorial(i)
    println("$i $f")
}
```

## Guards withing options

```groovy
def rnd = new Random()
def ri = rnd.nextInt(-5, 5)

def res = switch (ri) {
    case { ri < 0 } -> "${ri}: negative value"
    case { ri == 0 } -> "${ri}: zero"
    case { ri > 0 } -> "${ri}: positive value"

}

println res
```

## Matching enumerations

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

def days = [ Day.Monday, Day.Tuesday, Day.Wednesday, Day.Thursday, 
    Day.Friday, Day.Saturday, Day.Sunday ]

def res = []
def random = new Random()

(0..3).each {
    res << days[random.nextInt(days.size()) ]
}

for (e in res) {

    switch (e) {
        case Day.Monday ->
            println("monday")
        case Day.Tuesday ->
            println("tuesday")
        case Day.Wednesday ->
            println("wednesay")
        case Day.Thursday ->
            println("thursday")
        case Day.Friday ->
            println("friday")
        case Day.Saturday ->
            println("saturday")
        case Day.Sunday ->
            println("sunday")
    }
}
```

---

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


## Matching objects 

```groovy
record Cat(String name) {}
record Dog(String name) {}
record Person(String name) {}

def data = [new Cat('Missy'), new Dog('Jasper'), new Dog('Ace'), 
    new Person('Peter'), 'Jupiter']

for (e in data) {
 
    switch (e) {
        case Cat, Dog ->
            println("${e} is a pet")
        case Person ->
            println("${e} is a human")
        default ->
            println('unknown')
    }
}
```

## Matching ranges

```groovy
def rnd = new Random()
def ri = rnd.nextInt(0, 120)

switch (ri) {

    case 1..30 -> println('value is in the range from 1 to 30')
    case 31..60 -> println('value is in the range from 31 to 60')
    case 61..90 -> println('value is in the range from 61 to 90')
    case 91..120 -> println('value is in the range from 91 to 120')
}
```

## Matching regular expressions

```groovy 
# select all words starting with w or c

def words = ['week', 'bitcoin', 'cloud', 'copper', 'raw', 'war', 
    'cup', 'water']

def selected = []

for (word in words) {

    def res = switch (word) {

        case ~/^w.*/ -> word
        case ~/^c.*/ -> word
        default -> 'skip'
    }

    if (res != 'skip') {
        selected.add(res)
    }
}

println selected
```

## Matching lists 

Check if a list contains a value.  

```groovy
def users = [
    ['John', 'Doe', 'gardener'],
    ['Jane', 'Doe', 'teacher'],
    ['Roger', 'Roe', 'driver'],
    ['Martin', 'Molnar', 'programmer'],
    ['Robert', 'Kovac', 'shopkeeper'],
    ['Tomas', 'Novy', 'programmer']
]

def occupation = 'programmer'

for (user in users) {
    switch (occupation) {
        case user ->
            println("${user[0]} ${user[1]} is a programmer")
        default ->
            println("${user[0]} ${user[1]} is not a programmer")
    }
}
```


