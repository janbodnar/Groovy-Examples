# Groovy switch expressions

Switch expressions, also known as pattern matching, are a powerful and  
expressive feature in Groovy that provides a concise way to handle multiple  
conditional branches. Unlike traditional switch statements that execute code  
blocks, switch expressions return values directly, making them ideal for  
functional programming patterns and clean, readable code.  

Groovy switch expressions support advanced pattern matching capabilities,  
including type matching, range matching, regular expressions, collections,  
and custom predicates. They offer a more elegant alternative to long  
if-else chains and provide compile-time safety when used with enums. The  
arrow syntax (->) makes the intent clear and eliminates the need for break  
statements, reducing common programming errors.  

Switch expressions evaluate each case in order until a match is found, then  
return the corresponding value. If no case matches, the default case is  
evaluated. Guards (conditional expressions) can be used within cases for more  
complex matching logic.  

## Matching string literals 

String matching is one of the most common uses of switch expressions,  
providing clean alternatives to multiple string comparisons.  

```groovy
def res = System.console().readLine 'What is the capital of Slovakia?: '
def capital = res.capitalize()

def msg = switch (capital) {

    case 'Bratislava' -> 'correct answer'
    default -> 'wrong answer'
}

println msg
```

This example demonstrates basic string literal matching. The switch expression  
compares the input string against literal values and returns the corresponding  
result. The default case handles any non-matching input, ensuring the  
expression always returns a value.

## Matching integers 

Integer matching enables menu-driven applications and numeric categorization  
with clean, readable syntax.  

```groovy
def menu = '''
Select option
1 - start
2 - slow down
3 - accelerate
4 - pause
5 - terminate
'''

println menu

def opt = System.console().readLine ': ' 

def res = switch (opt as Integer) {

    case 1 -> 'start'
    case 2 -> 'slow down'
    case 3 -> 'accelerate'
    case 4 -> 'pause'
    case 5 -> 'terminate'
    default -> 'unknown'
}

println "your option: $res"
```

This example shows how integer values can be matched directly in switch  
expressions. The input string is converted to an integer using the `as`  
operator, and each case handles a specific menu option. The default case  
provides error handling for invalid menu choices.

## Matching types 

Type matching allows polymorphic behavior based on object types, essential for  
handling heterogeneous collections.  

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

Type matching compares the runtime type of the expression against class  
objects. This is particularly useful when processing collections containing  
mixed types, enabling type-safe operations and appropriate handling based  
on object type.

## Multiple options 

Multiple case values can be grouped together to share the same result,  
reducing code duplication and improving maintainability.  

```groovy
def grades = ['A', 'B', 'C', 'D', 'E', 'F', 'FX']

for (grade in grades) {

    switch (grade) {
        case 'A' , 'B' , 'C' , 'D' , 'E' , 'F' -> println('passed')
        case 'FX' -> println('failed')

    }
}
```

This example demonstrates how multiple values can be combined in a single case  
using comma separation. All passing grades are grouped together, making the  
code more concise than individual cases for each grade.

## Default option

The default case provides a fallback for values that don't match any specific  
case, ensuring switch expressions always return a value.  

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

This factorial implementation showcases the default case in recursive functions.  
The base cases (0 and 1) return 1 directly, while the default case handles all  
other values with recursive multiplication, demonstrating switch expressions in  
mathematical computations.

## Guards within options

Guards enable conditional logic within cases, allowing complex matching  
criteria beyond simple value comparison.  

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

Guard expressions use closures to define complex matching conditions. Each  
closure is evaluated and if it returns true, that case is selected. This allows  
for sophisticated pattern matching that goes beyond simple equality checks,  
enabling mathematical comparisons and custom predicates.

## Matching enumerations

Enum matching provides type-safe handling of enumerated values, with compiler  
support for exhaustive case coverage.  

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

This example shows individual enum constant matching with explicit cases for  
each day. Each enum value is matched directly, providing clear handling for  
specific enumerated values.

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

This advanced example demonstrates enum range matching, where consecutive  
enum values can be grouped using range syntax. This provides a more concise  
way to handle related enum constants together.


## Matching objects 

Object matching enables polymorphic behavior based on object types,  
particularly useful with records and custom classes.  

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

This example shows type-based matching with record classes. Multiple types  
can be grouped together (Cat, Dog) to share common behavior, while the  
default case handles unexpected types safely.

## Matching ranges

Range matching provides an elegant way to categorize numeric values into  
intervals without complex comparison logic.  

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

Range matching uses Groovy's range syntax to define numeric intervals.  
Each case checks if the value falls within the specified range, providing  
a clean alternative to multiple boolean conditions.

## Matching regular expressions

Regular expression matching enables sophisticated string pattern recognition  
within switch expressions.  

```groovy 
// select all words starting with w or c

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

Regular expression matching uses the ~/ syntax to define patterns. Each case  
can match against complex string patterns, enabling sophisticated text  
processing and filtering operations within switch expressions.

## Matching lists 

List matching enables checking membership and pattern matching against  
collection contents, useful for data filtering and validation.  

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

This example demonstrates list containment checking. The switch expression  
tests if the occupation string is contained within each user list, enabling  
efficient filtering of collections based on content.

---

```groovy
def users = [
    ['name': 'Paul', 'grades': ['D', 'A', 'B', 'A']],
    ['name': 'Martin', 'grades': ['F', 'B', 'E', 'FX']],
    ['name': 'Lucia', 'grades': ['A', 'A', 'B', 'FX']],
    ['name': 'Jan', 'grades': ['A', 'B', 'B', 'B']]
]

for (user in users) {
    
    switch ('FX') {

        case user.grades ->
            println("${user.name} did not pass the exams")
    }
}
```

This second example shows how to check if a specific value exists within  
nested collections. The switch expression matches when the failing grade  
'FX' is found in a user's grades list.

## Matching collections and sets

Collections and sets provide powerful membership testing capabilities  
within switch expressions for complex data filtering.  

```groovy
def allowedPorts = [80, 443, 8080, 8443] as Set
def restrictedIPs = ['192.168.1.100', '10.0.0.50', '172.16.0.200'] as Set

def connections = [
    [port: 80, ip: '192.168.1.10'],
    [port: 22, ip: '10.0.0.50'],
    [port: 443, ip: '192.168.1.100'],
    [port: 8080, ip: '203.0.113.1']
]

for (conn in connections) {
    def status = switch (conn.port) {
        case allowedPorts -> switch (conn.ip) {
            case restrictedIPs -> 'blocked - restricted IP'
            default -> 'allowed'
        }
        default -> 'blocked - invalid port'
    }
    
    println "Connection ${conn.ip}:${conn.port} - ${status}"
}
```

This example demonstrates nested switch expressions with set membership  
testing. Collections are converted to sets for efficient lookup, and  
nested switches provide multi-criteria validation logic.

## Matching map keys and values

Map matching enables sophisticated key-value pair processing and  
configuration-based conditional logic.  

```groovy
def config = [
    database: 'postgresql',
    cache: 'redis', 
    queue: 'rabbitmq',
    storage: 's3'
]

def services = ['web', 'api', 'worker', 'scheduler']

for (service in services) {
    def requirements = switch (service) {
        case config.keySet() -> "Service matches config key"
        default -> switch (service) {
            case 'web' -> ['database', 'cache']
            case 'api' -> ['database', 'cache', 'queue']  
            case 'worker' -> ['database', 'queue']
            case 'scheduler' -> ['database']
            default -> []
        }
    }
    
    println "Service: ${service}, Requirements: ${requirements}"
}
```

Map-based matching allows checking against keys, values, or entries.  
This example shows both key membership testing and service configuration  
mapping using nested switch expressions.

## Nested switch expressions

Switch expressions can be nested to create complex decision trees  
with clean, readable syntax.  

```groovy
def employees = [
    [name: 'Alice', department: 'Engineering', level: 'Senior', years: 5],
    [name: 'Bob', department: 'Sales', level: 'Junior', years: 2],
    [name: 'Carol', department: 'Engineering', level: 'Manager', years: 8],
    [name: 'Dave', department: 'Marketing', level: 'Senior', years: 4]
]

for (emp in employees) {
    def bonus = switch (emp.department) {
        case 'Engineering' -> switch (emp.level) {
            case 'Junior' -> emp.years * 1000
            case 'Senior' -> emp.years * 1500  
            case 'Manager' -> emp.years * 2000
            default -> 0
        }
        case 'Sales' -> switch (emp.level) {
            case 'Junior' -> emp.years * 800
            case 'Senior' -> emp.years * 1200
            default -> 0  
        }
        default -> emp.years * 500
    }
    
    println "${emp.name}: \$${bonus} bonus"
}
```

Nested switch expressions enable multi-dimensional decision making.  
Each level of nesting adds another criteria, creating sophisticated  
conditional logic while maintaining readability.

## Switch with closures and method references

Closures and method references provide flexible, reusable matching  
logic within switch expressions.  

```groovy
def isEven = { it % 2 == 0 }
def isPrime = { n ->
    if (n < 2) return false
    for (i in 2..<Math.sqrt(n) + 1) {
        if (n % i == 0) return false
    }
    return true
}

def numbers = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]

for (num in numbers) {
    def category = switch (num) {
        case isPrime -> switch (num) {
            case isEven -> "${num} is an even prime"
            default -> "${num} is an odd prime"  
        }
        case isEven -> "${num} is even but not prime"
        default -> "${num} is odd but not prime"
    }
    
    println category
}
```

Closures in switch expressions enable custom matching logic. Multiple  
closures can be combined in nested switches to create complex  
categorization systems with reusable predicates.

## Matching class instances with instanceof

Instance checking provides type-safe polymorphic behavior with  
detailed object introspection capabilities.  

```groovy
class Vehicle { String brand }
class Car extends Vehicle { int doors }
class Motorcycle extends Vehicle { boolean hasSidecar }
class Truck extends Vehicle { double loadCapacity }

def vehicles = [
    new Car(brand: 'Toyota', doors: 4),
    new Motorcycle(brand: 'Harley', hasSidecar: false),
    new Truck(brand: 'Ford', loadCapacity: 5.5),
    "Not a vehicle"
]

for (vehicle in vehicles) {
    def description = switch (vehicle) {
        case { it instanceof Car } -> 
            "${vehicle.brand} car with ${vehicle.doors} doors"
        case { it instanceof Motorcycle } -> 
            "${vehicle.brand} motorcycle" + 
            (vehicle.hasSidecar ? " with sidecar" : "")
        case { it instanceof Truck } ->
            "${vehicle.brand} truck (${vehicle.loadCapacity}t capacity)"
        case { it instanceof Vehicle } ->
            "${vehicle.brand} vehicle"
        default -> 
            "Not a vehicle: ${vehicle}"
    }
    
    println description
}
```

Instance checking with closures enables detailed type inspection  
beyond simple class matching. Each case can access specific properties  
while maintaining type safety through the instanceof operator.

## Switch with null handling

Null-safe switch expressions prevent NullPointerException while  
providing elegant handling of missing or undefined values.  

```groovy
def processUser(user) {
    return switch (user) {
        case null -> 'No user provided'
        case { it.name == null } -> 'User has no name'  
        case { it.name.trim().isEmpty() } -> 'User has empty name'
        case { it.age == null } -> "User ${user.name} has no age"
        case { it.age < 0 } -> "User ${user.name} has invalid age"
        case { it.age < 18 } -> "Minor: ${user.name} (${user.age})"
        case { it.age >= 65 } -> "Senior: ${user.name} (${user.age})"
        default -> "Adult: ${user.name} (${user.age})"
    }
}

def users = [
    null,
    [name: null, age: 25],
    [name: '', age: 30],
    [name: 'Alice', age: null],
    [name: 'Bob', age: -5],
    [name: 'Carol', age: 16],
    [name: 'Dave', age: 70],
    [name: 'Eve', age: 35]
]

for (user in users) {
    println processUser(user)
}
```

Null handling in switch expressions requires careful ordering of cases.  
Early null checks prevent runtime errors, while guard expressions  
enable safe property access with detailed validation logic.

## Switch with arithmetic operations

Mathematical expressions and calculations can be elegantly handled  
using switch expressions for algorithmic decision making.  

```groovy
def calculateTax(income, filingStatus) {
    def taxRate = switch (filingStatus.toLowerCase()) {
        case 'single' -> switch (income) {
            case { it <= 10000 } -> 0.10
            case { it <= 40000 } -> 0.15  
            case { it <= 85000 } -> 0.22
            case { it <= 160000 } -> 0.28
            default -> 0.35
        }
        case 'married' -> switch (income) {
            case { it <= 20000 } -> 0.10
            case { it <= 80000 } -> 0.15
            case { it <= 170000 } -> 0.22  
            case { it <= 320000 } -> 0.28
            default -> 0.35
        }
        default -> 0.25 // Default rate
    }
    
    return income * taxRate
}

def taxpayers = [
    [income: 25000, status: 'single'],
    [income: 75000, status: 'married'], 
    [income: 150000, status: 'single'],
    [income: 200000, status: 'married']
]

for (taxpayer in taxpayers) {
    def tax = calculateTax(taxpayer.income, taxpayer.status)
    printf "Income: \$%,.0f (%s) -> Tax: \$%,.2f%n", 
           taxpayer.income, taxpayer.status, tax
}
```

Arithmetic-based switch expressions enable complex calculation logic  
with bracket-based tax calculations, progressive rates, and  
mathematical decision trees for financial computations.

## Matching file extensions

File processing often requires different handling based on file types,  
making extension matching essential for file management applications.  

```groovy
def getFileProcessor(filename) {
    def ext = filename.toLowerCase().split('\\.').last()
    
    return switch (ext) {
        case ['jpg', 'jpeg', 'png', 'gif', 'bmp'] -> 
            "Image processor for ${filename}"
        case ['mp4', 'avi', 'mkv', 'mov', 'wmv'] ->
            "Video processor for ${filename}"  
        case ['mp3', 'wav', 'flac', 'aac', 'ogg'] ->
            "Audio processor for ${filename}"
        case ['pdf', 'doc', 'docx', 'txt', 'rtf'] ->
            "Document processor for ${filename}"
        case ['zip', 'rar', '7z', 'tar', 'gz'] ->
            "Archive processor for ${filename}"
        case ['exe', 'msi', 'dmg', 'deb', 'rpm'] ->
            "Installer processor for ${filename}"
        default -> 
            "Generic file processor for ${filename}"
    }
}

def files = [
    'photo.jpg', 'movie.mp4', 'song.mp3', 'document.pdf',
    'archive.zip', 'setup.exe', 'data.csv', 'script.groovy'
]

for (file in files) {
    println getFileProcessor(file)
}
```

File extension matching uses lists of related extensions grouped by  
file type. This provides a clean way to categorize files and apply  
appropriate processing logic based on file format.

## Switch with date/time patterns

Temporal pattern matching enables sophisticated date and time-based  
conditional logic for scheduling and time-sensitive applications.  

```groovy
import java.time.*
import java.time.format.DateTimeFormatter

def getSchedule(dateTime) {
    def hour = dateTime.hour
    def dayOfWeek = dateTime.dayOfWeek
    def month = dateTime.month
    
    return switch (dayOfWeek) {
        case DayOfWeek.SATURDAY, DayOfWeek.SUNDAY -> 
            switch (hour) {
                case 8..12 -> "Weekend morning shift"
                case 13..18 -> "Weekend afternoon shift"  
                default -> "Weekend off-hours"
            }
        case DayOfWeek.MONDAY..DayOfWeek.FRIDAY ->
            switch (month) {
                case Month.DECEMBER, Month.JANUARY -> 
                    switch (hour) {
                        case 7..9 -> "Winter morning shift"
                        case 10..15 -> "Winter day shift"
                        case 16..20 -> "Winter evening shift"
                        default -> "Winter off-hours"
                    }
                case Month.JUNE, Month.JULY, Month.AUGUST ->
                    switch (hour) {
                        case 6..8 -> "Summer early shift"
                        case 9..14 -> "Summer day shift" 
                        case 15..19 -> "Summer late shift"
                        default -> "Summer off-hours"
                    }
                default -> 
                    switch (hour) {
                        case 8..12 -> "Regular morning shift"
                        case 13..17 -> "Regular day shift"
                        default -> "Regular off-hours"  
                    }
            }
        default -> "Invalid day"
    }
}

def dates = [
    LocalDateTime.of(2024, 1, 15, 8, 30),  // Monday winter morning
    LocalDateTime.of(2024, 7, 10, 10, 0),  // Wednesday summer day
    LocalDateTime.of(2024, 4, 20, 14, 45), // Saturday spring afternoon
    LocalDateTime.of(2024, 12, 25, 22, 0)  // Wednesday winter off-hours
]

for (dt in dates) {
    def schedule = getSchedule(dt)
    def formatter = DateTimeFormatter.ofPattern("EEEE, MMMM d, yyyy 'at' h:mm a")
    def formatted = dt.format(formatter)
    println "${formatted}: ${schedule}"
}
```

Date/time pattern matching combines multiple temporal dimensions  
(day of week, hour, month) to create sophisticated scheduling logic.  
Nested switches handle seasonal variations and weekend patterns.

## Advanced pattern matching with destructuring

Record destructuring combined with switch expressions enables  
sophisticated data extraction and pattern matching capabilities.  

```groovy
record Point(double x, double y) {}
record Circle(Point center, double radius) {}  
record Rectangle(Point topLeft, double width, double height) {}
record Triangle(Point a, Point b, Point c) {}

def analyzeShape(shape) {
    return switch (shape) {
        case Point -> 
            "Point at (${shape.x}, ${shape.y})"
            
        case Circle -> {
            def area = Math.PI * shape.radius * shape.radius
            def circumference = 2 * Math.PI * shape.radius
            "Circle at (${shape.center.x}, ${shape.center.y}) " +
            "with radius ${shape.radius}, area ${area:.2f}, " +
            "circumference ${circumference:.2f}"
        }
        
        case Rectangle -> {
            def area = shape.width * shape.height
            def perimeter = 2 * (shape.width + shape.height)
            "Rectangle at (${shape.topLeft.x}, ${shape.topLeft.y}) " +
            "size ${shape.width}x${shape.height}, area ${area}, " +
            "perimeter ${perimeter}"
        }
        
        case Triangle -> {
            // Calculate side lengths using distance formula
            def sideA = Math.sqrt(Math.pow(shape.b.x - shape.a.x, 2) + 
                                Math.pow(shape.b.y - shape.a.y, 2))
            def sideB = Math.sqrt(Math.pow(shape.c.x - shape.b.x, 2) + 
                                Math.pow(shape.c.y - shape.b.y, 2))  
            def sideC = Math.sqrt(Math.pow(shape.a.x - shape.c.x, 2) + 
                                Math.pow(shape.a.y - shape.c.y, 2))
            
            // Heron's formula for area
            def s = (sideA + sideB + sideC) / 2
            def area = Math.sqrt(s * (s - sideA) * (s - sideB) * (s - sideC))
            
            "Triangle with vertices (${shape.a.x}, ${shape.a.y}), " +
            "(${shape.b.x}, ${shape.b.y}), (${shape.c.x}, ${shape.c.y}) " +
            "and area ${area:.2f}"
        }
        
        default -> "Unknown shape: ${shape}"
    }
}

def shapes = [
    new Point(3.0, 4.0),
    new Circle(new Point(0.0, 0.0), 5.0),
    new Rectangle(new Point(1.0, 1.0), 10.0, 8.0),
    new Triangle(new Point(0.0, 0.0), new Point(3.0, 0.0), new Point(1.5, 2.6)),
    "Invalid shape"
]

for (shape in shapes) {
    println analyzeShape(shape)
}
```

Advanced pattern matching with records enables complex data structure  
analysis. Each case can destructure record fields and perform  
sophisticated calculations while maintaining type safety and  
clean separation of concerns.


