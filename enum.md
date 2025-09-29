# Enum type

Enums (enumerations) are a special data type in Groovy that represents a  
group of named constants. They provide a way to define a collection of  
related constants with type safety and improved readability. Groovy enums  
are classes that extend the `java.lang.Enum` class and can contain fields,  
constructors, and methods just like regular classes.  

Enums are useful when you need to represent a fixed set of constants such as  
days of the week, seasons, states, or any other predefined set of values.  
Each enum constant is implicitly public, static, and final. They provide  
better type safety compared to using string or integer constants.  

## Basic enum declaration

The simplest form of enum contains a list of comma-separated constants.  
Each constant represents an instance of the enum type.  

```groovy
enum Color {
    RED, GREEN, BLUE
}

println Color.RED
println Color.GREEN
println Color.BLUE

println Color.RED.class
```

This example shows basic enum declaration and usage. Each enum constant  
(RED, GREEN, BLUE) is an instance of the Color enum type. The enum constants  
are accessible as static members and each has the same type as the enum itself.  

## ordinal, values

Every enum constant has an ordinal value (zero-based index) and you can  
retrieve all enum values as an array.  

```groovy
enum Size {
    SMALL, MEDIUM, LARGE
}

println Size.SMALL

Size.each { println it }
Size.each { println "${it} ${it.ordinal()}" }

println Size.values()
```

The `ordinal()` method returns the position of the enum constant (starting  
from 0). The `values()` method returns an array of all enum constants.  
Groovy's `each` method can iterate over all enum values automatically.  

## String coercion

Groovy supports automatic conversion between strings and enum constants  
using the `as` operator or direct assignment.  

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

String values can be automatically coerced to enum constants when the  
string matches the enum constant name. This feature makes it easy to  
convert user input or configuration values into enum types.  

## Custom method

Enums can contain static and instance methods just like regular classes.  
This enables adding behavior specific to the enum type.  

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

The custom `randomSeason()` static method demonstrates how enums can include  
utility methods. This particular method randomly selects one of the enum  
constants, which is useful for generating test data or random selections.  


## Switch expression ranges

Enums work naturally with switch expressions and can be used in ranges  
for grouping related constants.  

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

Switch expressions with enums provide elegant pattern matching. The range  
syntax (Day.Monday..Day.Friday) creates a convenient way to group enum  
constants, making the code more readable than multiple case statements.  

## Planets

Complex enums can have constructor parameters, fields, and methods to  
represent real-world entities with associated data and behavior.  

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

This advanced example demonstrates enums with constructors, fields, and  
instance methods. Each planet constant is initialized with mass and radius  
values, enabling calculations like surface gravity and weight conversion.  

## Enum name method

The `name()` method returns the exact name of the enum constant as declared  
in the source code.  

```groovy
enum Status {
    ACTIVE, INACTIVE, PENDING
}

Status.values().each { status ->
    println "Enum constant: ${status.name()}"
    println "String representation: ${status.toString()}"
    println "Ordinal: ${status.ordinal()}"
    println "---"
}
```

The `name()` method always returns the original constant name, while  
`toString()` can be overridden to provide custom string representation.  
This example shows the difference between various enum methods.  

## valueOf method

The `valueOf()` method converts a string to the corresponding enum constant,  
throwing an exception if the string doesn't match any constant.  

```groovy
enum Priority {
    LOW, MEDIUM, HIGH, URGENT
}

def priorities = ['LOW', 'HIGH', 'MEDIUM']

priorities.each { str ->
    try {
        Priority p = Priority.valueOf(str)
        println "Successfully converted '${str}' to ${p}"
    } catch (IllegalArgumentException e) {
        println "Invalid priority: ${str}"
    }
}

// Alternative using safe navigation
def safePriority = Priority.values().find { it.name() == 'CRITICAL' }
println "Safe lookup result: ${safePriority}"
```

The `valueOf()` method provides strict string-to-enum conversion with error  
handling. It's case-sensitive and requires exact matches. The safe navigation  
approach using `find()` returns null for non-matches instead of throwing.  

## Enum equality and comparison

Enums support equality comparison using `==` and can be compared using  
the spaceship operator `<=>` based on their ordinal values.  

```groovy
enum Grade {
    F, D, C, B, A
}

def grade1 = Grade.B
def grade2 = Grade.B
def grade3 = Grade.A

println "grade1 == grade2: ${grade1 == grade2}"
println "grade1.equals(grade2): ${grade1.equals(grade2)}"
println "grade1 is grade2: ${grade1.is(grade2)}"

println "Comparing grades:"
println "B <=> A: ${Grade.B <=> Grade.A}"
println "A <=> B: ${Grade.A <=> Grade.B}"
println "B <=> B: ${Grade.B <=> Grade.B}"

// Sorting enums by ordinal
def grades = [Grade.B, Grade.F, Grade.A, Grade.C]
println "Original: ${grades}"
println "Sorted: ${grades.sort()}"
```

Enum constants are singleton objects, so `==` and `is` produce the same  
result. The spaceship operator enables natural sorting based on declaration  
order, making it easy to sort collections of enum values.  

## Traditional switch statements

Besides switch expressions, enums work with traditional switch statements  
for control flow and side effects.  

```groovy
enum Operation {
    ADD, SUBTRACT, MULTIPLY, DIVIDE
}

def calculate(double a, double b, Operation op) {
    switch (op) {
        case Operation.ADD:
            return a + b
        case Operation.SUBTRACT:
            return a - b
        case Operation.MULTIPLY:
            return a * b
        case Operation.DIVIDE:
            return b != 0 ? a / b : Double.NaN
        default:
            throw new IllegalArgumentException("Unknown operation: ${op}")
    }
}

def operations = [Operation.ADD, Operation.MULTIPLY, Operation.DIVIDE]
operations.each { op ->
    def result = calculate(10, 3, op)
    println "10 ${op.name().toLowerCase()} 3 = ${result}"
}
```

Traditional switch statements with enums provide clear, readable control  
flow. Each case handles a specific enum constant, and the compiler can warn  
about missing cases when all enum values aren't handled.  

## Enums in collections

Enums work seamlessly with Groovy collections, providing type safety  
and easy filtering operations.  

```groovy
enum TaskStatus {
    TODO, IN_PROGRESS, REVIEW, DONE, CANCELLED
}

// Using enums in lists
def taskStatuses = [TaskStatus.TODO, TaskStatus.IN_PROGRESS, 
                   TaskStatus.DONE, TaskStatus.TODO, TaskStatus.REVIEW]

println "All statuses: ${taskStatuses}"
println "Unique statuses: ${taskStatuses.unique()}"
println "Completed tasks: ${taskStatuses.count(TaskStatus.DONE)}"

// Using enums in maps
def statusCounts = [:]
taskStatuses.each { status ->
    statusCounts[status] = (statusCounts[status] ?: 0) + 1
}

println "Status counts:"
statusCounts.each { status, count ->
    println "  ${status}: ${count}"
}

// Using enums in sets
def activeStatuses = [TaskStatus.TODO, TaskStatus.IN_PROGRESS, 
                     TaskStatus.REVIEW] as Set
println "Is TODO active? ${activeStatuses.contains(TaskStatus.TODO)}"
```

Enums in collections maintain type safety while providing all collection  
operations. They work well as map keys due to their singleton nature and  
consistent hash codes, making them ideal for counting and grouping.  

## Enum with instance methods

Enums can define instance methods that operate on the specific enum  
constant's data or provide constant-specific behavior.  

```groovy
enum Currency {
    USD("United States Dollar", "$"),
    EUR("Euro", "€"),
    GBP("British Pound", "£"),
    JPY("Japanese Yen", "¥");

    private final String fullName
    private final String symbol

    Currency(String fullName, String symbol) {
        this.fullName = fullName
        this.symbol = symbol
    }

    String getFullName() { fullName }
    String getSymbol() { symbol }

    String format(double amount) {
        return "${symbol}${String.format('%.2f', amount)}"
    }

    boolean isEuropean() {
        return this == EUR || this == GBP
    }
}

Currency.values().each { currency ->
    println "${currency.name()}: ${currency.fullName}"
    println "  Symbol: ${currency.symbol}"
    println "  Format 100.50: ${currency.format(100.50)}"
    println "  European: ${currency.isEuropean()}"
    println "---"
}
```

Instance methods in enums provide behavior specific to each constant.  
The `format()` method shows how to combine constant data with parameters,  
while `isEuropean()` demonstrates conditional logic based on enum identity.  

## Enum with abstract methods

Enums can define abstract methods that must be implemented by each  
enum constant, enabling constant-specific behavior.  

```groovy
enum MathOperation {
    PLUS {
        double apply(double x, double y) { return x + y }
        String getSymbol() { return "+" }
    },
    MINUS {
        double apply(double x, double y) { return x - y }
        String getSymbol() { return "-" }
    },
    TIMES {
        double apply(double x, double y) { return x * y }
        String getSymbol() { return "*" }
    },
    DIVIDE {
        double apply(double x, double y) { return x / y }
        String getSymbol() { return "/" }
    };

    abstract double apply(double x, double y)
    abstract String getSymbol()

    String calculate(double x, double y) {
        return "${x} ${getSymbol()} ${y} = ${apply(x, y)}"
    }
}

def x = 12.0, y = 4.0
MathOperation.values().each { op ->
    println op.calculate(x, y)
}
```

Abstract methods in enums force each constant to provide its own  
implementation. This pattern is powerful for operations where the behavior  
varies by constant while sharing common structure.  

## Enum implementing interfaces

Enums can implement interfaces to provide standardized behavior  
across different enum types.  

```groovy
interface Describable {
    String getDescription()
}

enum Vehicle implements Describable {
    CAR("Four wheels, personal transport"),
    MOTORCYCLE("Two wheels, fast and agile"), 
    TRUCK("Heavy duty, cargo transport"),
    BICYCLE("Two wheels, human-powered");

    private final String description

    Vehicle(String description) {
        this.description = description
    }

    @Override
    String getDescription() {
        return description
    }

    String getType() {
        return name().toLowerCase().capitalize()
    }
}

// Using interface method
Vehicle.values().each { vehicle ->
    println "${vehicle.getType()}: ${vehicle.getDescription()}"
}

// Using as interface type
List<Describable> items = [Vehicle.CAR, Vehicle.BICYCLE]
items.each { item ->
    println "Description: ${item.getDescription()}"
}
```

Implementing interfaces allows enums to participate in polymorphic behavior.  
The `Describable` interface enables treating different enum types uniformly  
while maintaining their specific implementations.  

## Enum with static initialization

Enums can have static fields and initialization blocks for sharing data  
across all enum constants.  

```groovy
enum HttpStatus {
    OK(200, "Success"),
    NOT_FOUND(404, "Resource not found"),
    INTERNAL_ERROR(500, "Server error");

    private final int code
    private final String message
    
    private static final Map<Integer, HttpStatus> BY_CODE = [:]
    
    static {
        values().each { status ->
            BY_CODE[status.code] = status
        }
    }

    HttpStatus(int code, String message) {
        this.code = code
        this.message = message
    }

    int getCode() { code }
    String getMessage() { message }

    static HttpStatus fromCode(int code) {
        return BY_CODE[code]
    }

    boolean isError() {
        return code >= 400
    }
}

// Using static lookup
def codes = [200, 404, 500, 999]
codes.each { code ->
    def status = HttpStatus.fromCode(code)
    if (status) {
        println "Code ${code}: ${status.name()} - ${status.message}"
        println "  Is error: ${status.isError()}"
    } else {
        println "Code ${code}: Unknown status"
    }
}
```

Static initialization blocks run when the enum class is first loaded,  
enabling efficient lookup structures. The `BY_CODE` map provides O(1)  
lookup by status code, which is more efficient than iterating values().  

## Enum serialization

Enums have built-in serialization support and maintain singleton  
properties during serialization and deserialization.  

```groovy
import java.io.*

enum Priority {
    LOW(1), NORMAL(5), HIGH(10), CRITICAL(15);

    private final int level

    Priority(int level) {
        this.level = level
    }

    int getLevel() { level }
}

// Serialize enum
def originalPriority = Priority.HIGH
def baos = new ByteArrayOutputStream()
def oos = new ObjectOutputStream(baos)
oos.writeObject(originalPriority)
oos.close()

// Deserialize enum
def bais = new ByteArrayInputStream(baos.toByteArray())
def ois = new ObjectInputStream(bais)
def deserializedPriority = ois.readObject()
ois.close()

println "Original: ${originalPriority}"
println "Deserialized: ${deserializedPriority}"
println "Same instance: ${originalPriority.is(deserializedPriority)}"
println "Equal: ${originalPriority == deserializedPriority}"
println "Level preserved: ${deserializedPriority.level}"
```

Enum serialization preserves the singleton property - deserialized enum  
constants are the same instances as the originals. This ensures that  
`==` and `is` comparisons work correctly after deserialization.  

## Enum with nested classes

Enums can contain nested classes to provide additional functionality  
and organization for related types.  

```groovy
enum DatabaseType {
    MYSQL("com.mysql.jdbc.Driver", 3306),
    POSTGRESQL("org.postgresql.Driver", 5432),
    ORACLE("oracle.jdbc.driver.OracleDriver", 1521);

    private final String driverClass
    private final int defaultPort

    DatabaseType(String driverClass, int defaultPort) {
        this.driverClass = driverClass
        this.defaultPort = defaultPort
    }

    String getDriverClass() { driverClass }
    int getDefaultPort() { defaultPort }

    static class ConnectionBuilder {
        private String host = "localhost"
        private int port
        private String database
        private DatabaseType type

        ConnectionBuilder(DatabaseType type) {
            this.type = type
            this.port = type.defaultPort
        }

        ConnectionBuilder host(String host) {
            this.host = host
            return this
        }

        ConnectionBuilder port(int port) {
            this.port = port
            return this
        }

        ConnectionBuilder database(String database) {
            this.database = database
            return this
        }

        String build() {
            return "jdbc:${type.name().toLowerCase()}://${host}:${port}/${database}"
        }
    }

    ConnectionBuilder connectionBuilder() {
        return new ConnectionBuilder(this)
    }
}

// Using nested class
def connection = DatabaseType.POSTGRESQL
    .connectionBuilder()
    .host("prod-server")
    .database("myapp")
    .build()

println "Connection string: ${connection}"
println "Driver class: ${DatabaseType.POSTGRESQL.driverClass}"
```

Nested classes in enums provide related functionality while maintaining  
encapsulation. The `ConnectionBuilder` pattern demonstrates how to create  
flexible, readable APIs using enum constants and nested helper classes.  

## Enum iteration and filtering

Groovy provides powerful iteration and filtering capabilities for enums  
using closures and collection methods.  

```groovy
enum FileType {
    TEXT("txt", "Text files"),
    IMAGE("jpg", "Image files"), 
    DOCUMENT("pdf", "Document files"),
    ARCHIVE("zip", "Archive files"),
    VIDEO("mp4", "Video files");

    private final String extension
    private final String description

    FileType(String extension, String description) {
        this.extension = extension
        this.description = description
    }

    String getExtension() { extension }
    String getDescription() { description }
}

// Various iteration patterns
println "All file types:"
FileType.each { type ->
    println "  ${type.name()}: .${type.extension} - ${type.description}"
}

// Filtering with findAll
def mediaTypes = FileType.values().findAll { 
    it == FileType.IMAGE || it == FileType.VIDEO 
}
println "\nMedia types: ${mediaTypes}"

// Using collect to transform
def extensions = FileType.collect { ".${it.extension}" }
println "Extensions: ${extensions}"

// Finding specific items
def documentType = FileType.values().find { it.extension == "pdf" }
println "Document type: ${documentType}"

// Grouping by criteria
def groupedByLength = FileType.values().groupBy { 
    it.extension.length() > 3 ? "long" : "short" 
}
println "Grouped by extension length: ${groupedByLength}"
```

Groovy's collection methods work seamlessly with enums, enabling functional  
programming patterns. These methods make it easy to filter, transform,  
and organize enum constants based on their properties.  

## Enum with validation

Enums can include validation logic to ensure data integrity and  
provide meaningful error messages for invalid operations.  

```groovy
enum PasswordStrength {
    WEAK(0, 7, "Weak password"),
    FAIR(8, 11, "Fair password"),
    GOOD(12, 15, "Good password"),
    STRONG(16, 20, "Strong password"),
    EXCELLENT(21, Integer.MAX_VALUE, "Excellent password");

    private final int minLength
    private final int maxLength
    private final String description

    PasswordStrength(int minLength, int maxLength, String description) {
        this.minLength = minLength
        this.maxLength = maxLength
        this.description = description
    }

    int getMinLength() { minLength }
    int getMaxLength() { maxLength }
    String getDescription() { description }

    boolean accepts(String password) {
        return password && password.length() >= minLength && 
               password.length() <= maxLength
    }

    static PasswordStrength evaluate(String password) {
        if (!password) {
            throw new IllegalArgumentException("Password cannot be null or empty")
        }
        
        return values().find { it.accepts(password) } ?: WEAK
    }

    List<String> getRequirements() {
        def reqs = ["Minimum ${minLength} characters"]
        if (maxLength != Integer.MAX_VALUE) {
            reqs << "Maximum ${maxLength} characters"
        }
        return reqs
    }
}

// Test password validation
def passwords = ["123", "password123", "MySecureP@ssw0rd2023", "x" * 25]

passwords.each { pwd ->
    try {
        def strength = PasswordStrength.evaluate(pwd)
        println "Password '${pwd}' (${pwd.length()} chars): ${strength.description}"
        println "  Requirements: ${strength.requirements.join(', ')}"
    } catch (Exception e) {
        println "Error: ${e.message}"
    }
    println "---"
}
```

Validation enums encapsulate business rules and provide clear feedback.  
The `evaluate()` method demonstrates how enums can serve as validators,  
while `getRequirements()` shows how to provide user-friendly guidance.  

## Complex enum with multiple features

This advanced example combines multiple enum features to create a  
sophisticated, real-world enum implementation.  

```groovy
import java.time.LocalTime

enum Restaurant {
    FAST_FOOD("Quick Bites", LocalTime.of(6, 0), LocalTime.of(23, 0), 
              ["burger", "fries", "soda"]) {
        @Override
        double calculateTip(double bill) {
            return bill * 0.10 // 10% tip for fast food
        }
        
        @Override
        String getAmbiance() { "Casual and quick" }
    },
    
    CASUAL("Family Diner", LocalTime.of(7, 0), LocalTime.of(22, 0),
           ["pasta", "pizza", "salad", "wine"]) {
        @Override  
        double calculateTip(double bill) {
            return bill * 0.15 // 15% tip for casual dining
        }
        
        @Override
        String getAmbiance() { "Relaxed family atmosphere" }
    },
    
    FINE_DINING("Le Gourmet", LocalTime.of(17, 0), LocalTime.of(23, 0),
                ["lobster", "steak", "wine", "dessert"]) {
        @Override
        double calculateTip(double bill) {
            return bill * 0.20 // 20% tip for fine dining
        }
        
        @Override
        String getAmbiance() { "Elegant and sophisticated" }
    };

    private final String name
    private final LocalTime openTime
    private final LocalTime closeTime
    private final List<String> menuItems
    
    private static final Map<String, Restaurant> BY_NAME = [:]
    
    static {
        values().each { restaurant ->
            BY_NAME[restaurant.name.toLowerCase()] = restaurant
        }
    }

    Restaurant(String name, LocalTime openTime, LocalTime closeTime, 
               List<String> menuItems) {
        this.name = name
        this.openTime = openTime
        this.closeTime = closeTime
        this.menuItems = new ArrayList<>(menuItems)
    }

    // Abstract methods for polymorphic behavior
    abstract double calculateTip(double bill)
    abstract String getAmbiance()

    // Instance methods
    String getName() { name }
    LocalTime getOpenTime() { openTime }
    LocalTime getCloseTime() { closeTime }
    List<String> getMenuItems() { new ArrayList<>(menuItems) }

    boolean isOpen(LocalTime time) {
        return !time.isBefore(openTime) && !time.isAfter(closeTime)
    }

    boolean hasMenuItem(String item) {
        return menuItems.any { it.toLowerCase().contains(item.toLowerCase()) }
    }

    String formatBill(double bill) {
        double tip = calculateTip(bill)
        double total = bill + tip
        return "Bill: \$${String.format('%.2f', bill)}, " +
               "Tip: \$${String.format('%.2f', tip)}, " +
               "Total: \$${String.format('%.2f', total)}"
    }

    // Static methods
    static Restaurant findByName(String name) {
        return BY_NAME[name?.toLowerCase()]
    }

    static List<Restaurant> getOpenRestaurants(LocalTime time) {
        return values().findAll { it.isOpen(time) }
    }
}

// Demonstrate complex enum usage
def currentTime = LocalTime.of(19, 30)
println "Current time: ${currentTime}"
println "Open restaurants:"

Restaurant.getOpenRestaurants(currentTime).each { restaurant ->
    println "\n${restaurant.name} (${restaurant.name()}):"
    println "  Hours: ${restaurant.openTime} - ${restaurant.closeTime}"
    println "  Ambiance: ${restaurant.ambiance}"
    println "  Menu items: ${restaurant.menuItems.join(', ')}"
    
    if (restaurant.hasMenuItem("wine")) {
        println "  Serves wine: Yes"
    }
    
    def bill = 85.00
    println "  ${restaurant.formatBill(bill)}"
}

// Test static lookup
def restaurant = Restaurant.findByName("family diner")
println "\nFound by name: ${restaurant?.name ?: 'Not found'}"
```

This comprehensive example demonstrates enums as powerful domain objects  
with multiple constructor parameters, abstract methods, instance and static  
methods, validation logic, and complex business rules. It showcases how  
enums can encapsulate sophisticated behavior while maintaining type safety.  




