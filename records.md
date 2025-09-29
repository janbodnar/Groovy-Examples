# Records

Groovy 4 introduced records as a new feature for creating immutable data  
classes. Records are a special kind of class that are designed to be simple  
carriers for immutable data. They automatically generate constructors,  
accessors, equals(), hashCode(), and toString() methods based on their  
component fields.  

Records provide a concise syntax for declaring classes that are primarily  
used to store data. They are similar to Java records but adapted for Groovy's  
dynamic nature and syntax. Groovy already had similar annotations such as  
`@Immutable` and `@Canonical` that provide comparable functionality, but  
records offer a more standardized and concise approach.  

Key characteristics of records:  
- Immutable by design - once created, their state cannot be changed  
- Automatic generation of standard methods  
- Compact declaration syntax  
- Built-in support for destructuring  
- Integration with Groovy's collection processing capabilities  

## Simple

```groovy
record User(String fname, String lname, String occupation) { }

def u = new User('John', 'Doe', 'gardener')
println u 

println u.fname
println u.lname
println u.occupation
```

This example demonstrates the basic record syntax. Records automatically  
generate accessor methods for each component, allowing direct access to  
the field values.  

## Named-arg constructor

```groovy
record User(String fname, String lname, String occupation) { }

def u = new User(lname:'Roe', fname:'Roger', occupation:'driver')
println u
```

Records support Groovy's named argument constructor syntax, allowing you  
to specify arguments in any order using their parameter names.  

## Destructure

```groovy
record User(String fname, String lname, String occupation) { }

def u = new User('John', 'Doe', 'gardener')
def (fname, lname, occupation) = u

println "${fname} ${lname} is a ${occupation}"
```

Records can be destructured using Groovy's multiple assignment syntax,  
extracting all components into separate variables in declaration order.  

## Sortable with records

```groovy
import groovy.transform.Sortable

@Sortable(includes='lname')
record User(String fname, String lname, String occupation) {}

def users = [
    new User('John', 'Doe', 'gardener'),
    new User('Roger', 'Roe', 'driver'),
    new User('Lucia', 'Smith', 'accountant'),
    new User('Paul', 'Newman', 'firefighter'),
    new User('Adam', 'Clapton', 'teacher'),
    new User('Jane', 'Walter', 'pilot')
]

for (def user in users) {
    println user
}

println '----------------------'

users.sort()

for (def user in users) {
    println user
}
```

Records work well with Groovy's transformation annotations. The @Sortable  
annotation makes records sortable by specified fields, enabling easy  
collection ordering.  

## Grouping 

```groovy
import java.time.LocalDate

record User(String name, String occupation, LocalDate dob) { }

def users = [

    new User('John Doe', 'gardener', LocalDate.parse('1973-09-07', 'yyyy-MM-dd')),
    new User('Roger Roe', 'driver', LocalDate.parse('1963-03-30', 'yyyy-MM-dd')),
    new User('Kim Smith', 'teacher', LocalDate.parse('1980-05-12', 'yyyy-MM-dd')),
    new User('Joe Nigel', 'artist', LocalDate.parse('1983-03-30', 'yyyy-MM-dd')),
    new User('Liam Strong', 'teacher', LocalDate.parse('2009-03-06', 'yyyy-MM-dd')),
    new User('Robert Young', 'gardener', LocalDate.parse('1978-11-16', 'yyyy-MM-dd')),
    new User('Liam Strong', 'teacher', LocalDate.parse('1986-10-23', 'yyyy-MM-dd'))
]
 
def res = users.groupBy({ it.occupation })

for (def e in res) {
    
    println e
}
```

Records integrate seamlessly with Groovy's collection methods. The groupBy  
method groups users by their occupation, creating a map where keys are  
occupations and values are lists of matching users.  

Split users into millenials and others. We use the static `of` builder method for the record.  

```groovy
import java.time.LocalDate

record User(String name, String occupation, LocalDate dob) { 
    static User of(String name, String occupation, LocalDate dob) {
        return new User(name, occupation, dob)
    }
}

def users = [

    User.of('John Doe', 'gardener', LocalDate.parse('1973-09-07', 'yyyy-MM-dd')),
    User.of('Roger Roe', 'driver', LocalDate.parse('1963-03-30', 'yyyy-MM-dd')),
    User.of('Kim Smith', 'teacher', LocalDate.parse('1980-05-12', 'yyyy-MM-dd')),
    User.of('Joe Nigel', 'artist', LocalDate.parse('1983-03-30', 'yyyy-MM-dd')),
    User.of('Liam Strong', 'teacher', LocalDate.parse('2009-03-06', 'yyyy-MM-dd')),
    User.of('Robert Young', 'gardener', LocalDate.parse('1978-11-16', 'yyyy-MM-dd')),
    User.of('Liam Strong', 'teacher', LocalDate.parse('1986-10-23', 'yyyy-MM-dd'))
]

def millen = LocalDate.parse('2000-01-01')
def res = users.groupBy({ it.dob > millen })

println 'millenials'

for (def e in res[true]) {
    println e
}

println 'others'

for (def e in res[false]) {
    println e
}
```

## Equality and hashCode

Records automatically implement equals() and hashCode() based on all  
components. Two records are equal if all their components are equal.  

```groovy
record Point(int x, int y) { }

def p1 = new Point(3, 4)
def p2 = new Point(3, 4)
def p3 = new Point(5, 6)

println p1 == p2
println p1 == p3
println p1.hashCode() == p2.hashCode()
println p1.hashCode() == p3.hashCode()
```

## toString customization

Records provide a default toString() implementation, but you can  
customize it by overriding the method.  

```groovy
record Product(String name, double price, String category) {
    @Override
    String toString() {
        return "${name} (${category}) - \$${price}"
    }
}

def product = new Product('Laptop', 999.99, 'Electronics')
println product
```

## Record with validation

Records can include validation logic in constructors or methods  
to ensure data integrity.  

```groovy
record Temperature(double celsius) {
    Temperature {
        if (celsius < -273.15) {
            throw new IllegalArgumentException('Temperature cannot be below absolute zero')
        }
    }
    
    double getFahrenheit() {
        return (celsius * 9/5) + 32
    }
    
    double getKelvin() {
        return celsius + 273.15
    }
}

def temp = new Temperature(25.0)
println "Celsius: ${temp.celsius}"
println "Fahrenheit: ${temp.fahrenheit}"
println "Kelvin: ${temp.kelvin}"
```

## Record with collections

Records can contain collection components and work seamlessly  
with Groovy's collection methods.  

```groovy
record Course(String name, List<String> students, Map<String, Integer> grades) { }

def course = new Course(
    'Groovy Programming',
    ['Alice', 'Bob', 'Charlie'],
    [Alice: 85, Bob: 92, Charlie: 78]
)

println "Course: ${course.name}"
println "Students: ${course.students.join(', ')}"
println "Average grade: ${course.grades.values().sum() / course.grades.size()}"
```

## Record inheritance with interfaces

Records can implement interfaces to provide polymorphic behavior  
while maintaining immutability.  

```groovy
interface Drawable {
    void draw()
}

record Circle(double radius, String color) implements Drawable {
    void draw() {
        println "Drawing a ${color} circle with radius ${radius}"
    }
    
    double getArea() {
        return Math.PI * radius * radius
    }
}

def circle = new Circle(5.0, 'red')
circle.draw()
println "Area: ${circle.area}"
```

## Record serialization

Records work well with serialization, maintaining their immutable  
nature across serialization boundaries.  

```groovy
import java.io.*

record Employee(String name, int id, String department) implements Serializable { }

def employee = new Employee('Jane Smith', 123, 'Engineering')

// Serialize
def baos = new ByteArrayOutputStream()
def oos = new ObjectOutputStream(baos)
oos.writeObject(employee)
oos.close()

// Deserialize
def bais = new ByteArrayInputStream(baos.toByteArray())
def ois = new ObjectInputStream(bais)
def deserializedEmployee = ois.readObject()
ois.close()

println "Original: ${employee}"
println "Deserialized: ${deserializedEmployee}"
println "Equal: ${employee == deserializedEmployee}"
```

## Builder pattern with records

Records can be enhanced with builder patterns for more flexible  
object construction, especially useful for optional components.  

```groovy
record Address(String street, String city, String state, String zipCode) {
    static Builder builder() {
        return new Builder()
    }
    
    static class Builder {
        String street, city, state, zipCode
        
        Builder street(String street) { this.street = street; return this }
        Builder city(String city) { this.city = city; return this }
        Builder state(String state) { this.state = state; return this }
        Builder zipCode(String zipCode) { this.zipCode = zipCode; return this }
        
        Address build() {
            return new Address(street, city, state, zipCode)
        }
    }
}

def address = Address.builder()
    .street('123 Main St')
    .city('Springfield')
    .state('IL')
    .zipCode('62701')
    .build()

println address
```

## Record with annotations

Records can be annotated with various Groovy transformations  
to add additional functionality.  

```groovy
import groovy.transform.ToString
import groovy.transform.EqualsAndHashCode

@ToString(includeNames=true)
@EqualsAndHashCode
record Book(String title, String author, int pages, double rating) { 
    boolean isLongBook() {
        return pages > 300
    }
}

def book = new Book('Groovy in Action', 'Dierk König', 496, 4.5)
println book
println "Long book: ${book.isLongBook()}"
```

## Method overriding in records

Records allow overriding methods to provide custom behavior  
while maintaining the benefits of records.  

```groovy
record Money(double amount, String currency) {
    Money plus(Money other) {
        if (this.currency != other.currency) {
            throw new IllegalArgumentException('Cannot add different currencies')
        }
        return new Money(this.amount + other.amount, this.currency)
    }
    
    Money multiply(double factor) {
        return new Money(this.amount * factor, this.currency)
    }
    
    @Override
    String toString() {
        return String.format("%.2f %s", amount, currency)
    }
}

def price = new Money(19.99, 'USD')
def tax = new Money(1.60, 'USD')
def total = price.plus(tax)
def discounted = total.multiply(0.9)

println "Price: ${price}"
println "Tax: ${tax}"
println "Total: ${total}"
println "After 10% discount: ${discounted}"
```

## Generic records

Records can be parameterized with generics to create reusable  
data structures for different types.  

```groovy
record Pair<T, U>(T first, U second) {
    Pair<U, T> swap() {
        return new Pair<>(second, first)
    }
}

record Container<T>(T value, String label) {
    T getValue() { return value }
}

def stringIntPair = new Pair<>('age', 25)
def swapped = stringIntPair.swap()

def container = new Container<>([1, 2, 3, 4, 5], 'Numbers')

println "Original pair: ${stringIntPair}"
println "Swapped pair: ${swapped}"
println "Container: ${container.label} = ${container.value}"
```

## Record conversion and transformation

Records can be easily converted to other formats and transformed  
using Groovy's powerful collection methods.  

```groovy
record Student(String name, int age, List<Double> grades) {
    double getAverageGrade() {
        return grades.sum() / grades.size()
    }
    
    Map<String, Object> toMap() {
        return [
            name: this.name,
            age: this.age,
            grades: this.grades,
            average: this.averageGrade
        ]
    }
}

def students = [
    new Student('Emma', 20, [85.5, 92.0, 88.5]),
    new Student('Liam', 21, [78.0, 85.5, 90.0]),
    new Student('Sofia', 19, [95.0, 88.0, 92.5])
]

def studentMaps = students.collect { it.toMap() }
def topStudents = students.findAll { it.averageGrade > 85 }

println 'All students as maps:'
studentMaps.each { println it }

println '\nTop students:'
topStudents.each { println "${it.name}: ${it.averageGrade}" }
```

## Record comparison and sorting

Records can be easily compared and sorted using custom criteria,  
leveraging Groovy's flexible comparison operators.  

```groovy
record Score(String player, int points, String game) implements Comparable<Score> {
    @Override
    int compareTo(Score other) {
        return this.points <=> other.points
    }
}

def scores = [
    new Score('Alice', 1200, 'Chess'),
    new Score('Bob', 950, 'Chess'),
    new Score('Charlie', 1450, 'Chess'),
    new Score('Diana', 1100, 'Chess')
]

println 'Original scores:'
scores.each { println it }

println '\nSorted by points (ascending):'
scores.sort().each { println it }

println '\nSorted by points (descending):'
scores.sort().reverse().each { println it }

println '\nSorted by player name:'
scores.sort { it.player }.each { println it }
```

## Record filtering and selection

Records work seamlessly with Groovy's filtering methods,  
making data selection and manipulation straightforward.  

```groovy
import java.time.LocalDate

record Transaction(String id, double amount, String type, LocalDate date) { }

def transactions = [
    new Transaction('T001', 250.0, 'DEPOSIT', LocalDate.parse('2024-01-15')),
    new Transaction('T002', -75.50, 'WITHDRAWAL', LocalDate.parse('2024-01-16')),
    new Transaction('T003', 1200.0, 'DEPOSIT', LocalDate.parse('2024-01-17')),
    new Transaction('T004', -45.25, 'WITHDRAWAL', LocalDate.parse('2024-01-18')),
    new Transaction('T005', 800.0, 'DEPOSIT', LocalDate.parse('2024-01-19'))
]

def deposits = transactions.findAll { it.type == 'DEPOSIT' }
def withdrawals = transactions.findAll { it.type == 'WITHDRAWAL' }
def largeTransactions = transactions.findAll { Math.abs(it.amount) > 100 }

println 'All deposits:'
deposits.each { println "${it.id}: ${it.amount}" }

println '\nAll withdrawals:'
withdrawals.each { println "${it.id}: ${it.amount}" }

println '\nLarge transactions:'
largeTransactions.each { println "${it.id}: ${it.amount} (${it.type})" }
```

## Record mapping and transformation

Records can be easily mapped to other data structures,  
enabling flexible data processing pipelines.  

```groovy
record Order(String id, String customer, List<String> items, double total) { 
    int getItemCount() {
        return items.size()
    }
}

def orders = [
    new Order('O001', 'John Doe', ['Laptop', 'Mouse'], 1050.0),
    new Order('O002', 'Jane Smith', ['Book', 'Pen', 'Notebook'], 25.50),
    new Order('O003', 'Mike Johnson', ['Phone', 'Case', 'Charger'], 650.0)
]

def customerTotals = orders.collectEntries { [it.customer, it.total] }
def itemCounts = orders.collect { [customer: it.customer, items: it.itemCount] }
def summaryReport = orders.collect { 
    "${it.customer}: ${it.itemCount} items, \$${it.total}"
}

println 'Customer totals:'
customerTotals.each { customer, total -> println "${customer}: \$${total}" }

println '\nItem counts:'
itemCounts.each { println it }

println '\nSummary report:'
summaryReport.each { println it }
```

## Record with date and time operations

Records containing date/time components can leverage Groovy's  
excellent date/time handling capabilities.  

```groovy
import java.time.*

record Event(String name, LocalDateTime startTime, Duration duration) {
    LocalDateTime getEndTime() {
        return startTime.plus(duration)
    }
    
    boolean isActive(LocalDateTime now) {
        return now.isAfter(startTime) && now.isBefore(getEndTime())
    }
    
    boolean overlapsWith(Event other) {
        return startTime.isBefore(other.getEndTime()) && 
               getEndTime().isAfter(other.startTime)
    }
}

def now = LocalDateTime.now()
def events = [
    new Event('Meeting', now.plusHours(1), Duration.ofHours(2)),
    new Event('Lunch', now.plusHours(3), Duration.ofMinutes(60)),
    new Event('Workshop', now.plusHours(2), Duration.ofHours(3))
]

events.each { event ->
    println "${event.name}: ${event.startTime} to ${event.endTime}"
    println "  Currently active: ${event.isActive(now)}"
}

println '\nOverlapping events:'
for (int i = 0; i < events.size(); i++) {
    for (int j = i + 1; j < events.size(); j++) {
        if (events[i].overlapsWith(events[j])) {
            println "${events[i].name} overlaps with ${events[j].name}"
        }
    }
}
```

## Record with numeric operations

Records can encapsulate numeric data and provide mathematical  
operations while maintaining immutability.  

```groovy
record Vector(double x, double y, double z) {
    double getMagnitude() {
        return Math.sqrt(x*x + y*y + z*z)
    }
    
    Vector add(Vector other) {
        return new Vector(x + other.x, y + other.y, z + other.z)
    }
    
    Vector multiply(double scalar) {
        return new Vector(x * scalar, y * scalar, z * scalar)
    }
    
    double dotProduct(Vector other) {
        return x * other.x + y * other.y + z * other.z
    }
    
    Vector normalize() {
        double mag = getMagnitude()
        return new Vector(x/mag, y/mag, z/mag)
    }
}

def v1 = new Vector(3, 4, 5)
def v2 = new Vector(1, 2, 3)

println "v1: ${v1}"
println "v2: ${v2}"
println "v1 magnitude: ${v1.magnitude}"
println "v1 + v2: ${v1.add(v2)}"
println "v1 * 2: ${v1.multiply(2)}"
println "v1 · v2: ${v1.dotProduct(v2)}"
println "v1 normalized: ${v1.normalize()}"
```

## Record with JSON-like operations

Records can be easily converted to and from JSON-like structures,  
making them ideal for data interchange.  

```groovy
import groovy.json.JsonBuilder
import groovy.json.JsonSlurper

record Person(String name, int age, List<String> hobbies, Address address) { }
record Address(String street, String city, String country) { }

def address = new Address('123 Oak St', 'Boston', 'USA')
def person = new Person('Alex Thompson', 28, ['reading', 'hiking', 'coding'], address)

// Convert to JSON
def json = new JsonBuilder(person)
def jsonString = json.toPrettyString()
println 'Person as JSON:'
println jsonString

// Convert from Map (simulating JSON parsing)
def personMap = [
    name: 'Sarah Wilson',
    age: 32,
    hobbies: ['photography', 'travel'],
    address: [street: '456 Pine Ave', city: 'Seattle', country: 'USA']
]

def addressFromMap = new Address(
    personMap.address.street,
    personMap.address.city, 
    personMap.address.country
)
def personFromMap = new Person(
    personMap.name,
    personMap.age,
    personMap.hobbies,
    addressFromMap
)

println '\nPerson from Map:'
println personFromMap
```

## Record stream operations

Records integrate well with Java streams and Groovy's  
collection processing capabilities for functional programming.  

```groovy
import java.util.stream.Collectors

record Sale(String product, double amount, String region, String salesperson) { }

def sales = [
    new Sale('Laptop', 1200.0, 'North', 'Alice'),
    new Sale('Mouse', 25.0, 'South', 'Bob'),
    new Sale('Keyboard', 80.0, 'North', 'Alice'),
    new Sale('Monitor', 300.0, 'East', 'Charlie'),
    new Sale('Laptop', 1150.0, 'West', 'Diana'),
    new Sale('Phone', 600.0, 'South', 'Bob')
]

// Using streams
def totalByRegion = sales.stream()
    .collect(Collectors.groupingBy(
        { it.region }, 
        Collectors.summingDouble { it.amount }
    ))

def topSalespeople = sales.stream()
    .collect(Collectors.groupingBy(
        { it.salesperson }, 
        Collectors.summingDouble { it.amount }
    ))
    .entrySet().stream()
    .sorted { -it.value }
    .limit(2)
    .collect(Collectors.toList())

println 'Sales by region:'
totalByRegion.each { region, total -> 
    println "${region}: \$${total}" 
}

println '\nTop salespeople:'
topSalespeople.each { entry -> 
    println "${entry.key}: \$${entry.value}" 
}
```

## Record validation with constraints

Records can include sophisticated validation logic to ensure  
data integrity and business rule compliance.  

```groovy
record BankAccount(String accountNumber, String owner, double balance, String type) {
    BankAccount {
        if (!accountNumber?.matches(/\d{10}/)) {
            throw new IllegalArgumentException('Account number must be 10 digits')
        }
        if (!owner?.trim()) {
            throw new IllegalArgumentException('Owner name cannot be empty')
        }
        if (balance < 0 && type != 'CREDIT') {
            throw new IllegalArgumentException('Only credit accounts can have negative balance')
        }
        if (!(type in ['CHECKING', 'SAVINGS', 'CREDIT'])) {
            throw new IllegalArgumentException('Invalid account type')
        }
    }
    
    boolean canWithdraw(double amount) {
        return balance >= amount || type == 'CREDIT'
    }
    
    BankAccount withdraw(double amount) {
        if (!canWithdraw(amount)) {
            throw new IllegalStateException('Insufficient funds')
        }
        return new BankAccount(accountNumber, owner, balance - amount, type)
    }
    
    BankAccount deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException('Deposit amount must be positive')
        }
        return new BankAccount(accountNumber, owner, balance + amount, type)
    }
}

def account = new BankAccount('1234567890', 'John Doe', 1000.0, 'CHECKING')
println "Initial account: ${account}"

def afterDeposit = account.deposit(250.0)
println "After deposit: ${afterDeposit}"

def afterWithdrawal = afterDeposit.withdraw(100.0)
println "After withdrawal: ${afterWithdrawal}"
```

## Record factory methods and constants

Records can provide factory methods and constants for common  
instances and convenient object creation patterns.  

```groovy
record Color(int red, int green, int blue) {
    // Constants
    static final Color BLACK = new Color(0, 0, 0)
    static final Color WHITE = new Color(255, 255, 255)
    static final Color RED = new Color(255, 0, 0)
    static final Color GREEN = new Color(0, 255, 0)
    static final Color BLUE = new Color(0, 0, 255)
    
    // Factory methods
    static Color fromHex(String hex) {
        if (!hex.startsWith('#') || hex.length() != 7) {
            throw new IllegalArgumentException('Invalid hex color format')
        }
        int rgb = Integer.parseInt(hex.substring(1), 16)
        return new Color(
            (rgb >> 16) & 0xFF,
            (rgb >> 8) & 0xFF,
            rgb & 0xFF
        )
    }
    
    static Color grayscale(int value) {
        return new Color(value, value, value)
    }
    
    String toHex() {
        return String.format("#%02X%02X%02X", red, green, blue)
    }
    
    Color blend(Color other, double ratio) {
        int r = (int)(red * (1-ratio) + other.red * ratio)
        int g = (int)(green * (1-ratio) + other.green * ratio) 
        int b = (int)(blue * (1-ratio) + other.blue * ratio)
        return new Color(r, g, b)
    }
}

println "Constants:"
println "Red: ${Color.RED} -> ${Color.RED.toHex()}"
println "Blue: ${Color.BLUE} -> ${Color.BLUE.toHex()}"

println "\nFactory methods:"
def purple = Color.fromHex('#800080')
def gray = Color.grayscale(128)
println "Purple from hex: ${purple} -> ${purple.toHex()}"
println "Gray: ${gray} -> ${gray.toHex()}"

println "\nBlending:"
def blended = Color.RED.blend(Color.BLUE, 0.5)
println "Red + Blue (50/50): ${blended} -> ${blended.toHex()}"
```

## Complex record composition

Records can be composed together to create complex data structures  
while maintaining the benefits of immutability and value semantics.  

```groovy
import java.time.LocalDate

record ContactInfo(String email, String phone) { }

record PersonalInfo(String firstName, String lastName, LocalDate birthDate) {
    String getFullName() {
        return "${firstName} ${lastName}"
    }
    
    int getAge() {
        return Period.between(birthDate, LocalDate.now()).years
    }
}

record WorkInfo(String company, String position, double salary) { }

record FullProfile(PersonalInfo personal, ContactInfo contact, WorkInfo work, List<String> skills) {
    String getSummary() {
        return "${personal.fullName} (${personal.age}) - ${work.position} at ${work.company}"
    }
    
    boolean hasSkill(String skill) {
        return skills.any { it.toLowerCase().contains(skill.toLowerCase()) }
    }
    
    FullProfile addSkill(String skill) {
        def newSkills = new ArrayList<>(skills)
        newSkills.add(skill)
        return new FullProfile(personal, contact, work, newSkills)
    }
    
    FullProfile updateSalary(double newSalary) {
        def newWork = new WorkInfo(work.company, work.position, newSalary)
        return new FullProfile(personal, contact, newWork, skills)
    }
}

def personalInfo = new PersonalInfo('Emma', 'Johnson', LocalDate.parse('1990-05-15'))
def contactInfo = new ContactInfo('emma.johnson@email.com', '+1-555-0123')
def workInfo = new WorkInfo('Tech Corp', 'Software Engineer', 85000.0)
def skills = ['Java', 'Groovy', 'Python', 'SQL']

def profile = new FullProfile(personalInfo, contactInfo, workInfo, skills)

println "Profile summary: ${profile.summary}"
println "Has Java skill: ${profile.hasSkill('Java')}"
println "Has C++ skill: ${profile.hasSkill('C++')}"

def updatedProfile = profile.addSkill('Docker').updateSalary(90000.0)
println "Updated profile summary: ${updatedProfile.summary}"
println "Updated skills: ${updatedProfile.skills}"
println "Updated salary: \$${updatedProfile.work.salary}"
```



