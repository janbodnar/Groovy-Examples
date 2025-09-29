# Sorting

Sorting is a fundamental operation in programming that arranges elements in a  
collection according to a specific order. Groovy provides powerful and  
flexible sorting capabilities through its rich collection API and closures.  

In Groovy, sorting can be performed on various data types including numbers,  
strings, objects, and custom data structures. The language offers multiple  
approaches to sorting: using the built-in `sort()` method with default  
natural ordering, providing custom comparison logic through closures, or  
implementing `Comparable` interface for object sorting.  

Groovy's sorting methods modify collections in-place by default, but also  
provide options for creating sorted copies. The `sort()` method accepts  
closures that define custom comparison criteria, making it highly flexible  
for complex sorting requirements.  

Key sorting concepts in Groovy include:  
- Natural ordering using default comparators  
- Custom sorting with closure-based comparators  
- Multi-field sorting for complex objects  
- Stable vs unstable sorting behaviors  
- Performance considerations for large datasets  

The spaceship operator `<=>` is particularly useful in Groovy sorting,  
providing a concise way to implement comparison logic. This operator returns  
-1, 0, or 1 based on whether the left operand is less than, equal to, or  
greater than the right operand.  

## Basic sorting

This example demonstrates fundamental sorting operations on numeric and  
string collections using Groovy's built-in sort methods.  

```groovy
def nums = [ 7, 9, 3, -2, 8, 1, 0 ]
def words = [ "sky", "cloud", "atom", "brown", "den", "kite", "town" ]

nums.sort()
println nums

nums.sort { -it }
println nums

nums.sort { a, b -> a <=> b }
println nums

println "--------------------------------------------"

words.sort()
println words
Collections.reverse(words)
println words
```

The first `nums.sort()` call sorts numbers in ascending order using natural  
ordering. The second sort uses a closure `{ -it }` to sort in descending  
order by negating each value. The third sort explicitly uses the spaceship  
operator for ascending order. For strings, `sort()` applies lexicographic  
ordering, and `Collections.reverse()` reverses the sorted order.  

## Sort words by length

This example shows how to sort strings based on their length rather than  
alphabetical order using a custom closure.  

```groovy
def words = [ "sky", "Sun", "Albert", "cloud", "by", "Earth", "else",
      "atom", "brown", "a", "den", "kite", "town" ]

words.sort { e -> e.length() }
println words
```

The closure `{ e -> e.length() }` extracts the length of each string element  
for comparison. Groovy automatically uses these length values to determine  
the sorting order, placing shorter strings before longer ones.  

## Sort by surnames

This example demonstrates extracting and sorting by specific parts of string  
elements, focusing on surnames in full names.  

```groovy
def names = [ "John Doe", "Lucy Smith", "Benjamin Young", "Robert Brown",
      "Thomas Moore", "Linda Black", "Adam Smith", "Jane Smith" ]

names.sort { it.split('\s{1}')[1] }
println names

names.sort { name ->

    def ns = name.split('\s{1}')
    def a = ns[0]
    def b = ns[1]

    return a <=> b 
}

println names
```

The first sort uses `split('\s{1}')[1]` to extract the surname (second part)  
from each full name for comparison. The second approach shows a more verbose  
method that splits names into parts but incorrectly sorts by first name  
instead of surname, demonstrating the importance of correct comparison logic.  

## Case-insensitive string sorting

This example shows how to sort strings while ignoring case differences,  
useful for alphabetical organization regardless of capitalization.  

```groovy
def words = [ "sky", "Sun", "Albert", "cloud", "by", "Earth", "else",
      "atom", "brown", "a", "den", "kite", "town" ]

words.sort { a -> a.toLowerCase() }
println words

words.sort()
println words
```

The first sort uses `toLowerCase()` to normalize case before comparison,  
ensuring case-insensitive alphabetical ordering. The second sort demonstrates  
default behavior where uppercase letters are sorted before lowercase letters  
in ASCII order.  

## Custom comparator

This example demonstrates creating a reusable comparator that sorts by  
multiple criteria: first by length, then alphabetically for equal lengths.  

```groovy
Comparator mc = { a, b -> 
    def n1 = a.length()
    def n2 = b.length()

    if (n1 == n2) return a <=> b 
    else return n1 <=> n2
}

def words = [ "sky", "eye", "Sun", "Albert", "cloud", "by", "Earth", "else",
      "of", "atom", "brown", "lie", "a", "be", "den", "kite", "to", "town" ]

words.sort(mc)
println words
```

The custom comparator first compares string lengths, and when lengths are  
equal, it falls back to lexicographic comparison. This creates a two-level  
sorting hierarchy: shorter strings come first, and strings of equal length  
are sorted alphabetically.  

## Sorting with null values

This example shows how Groovy handles null values during sorting operations  
and demonstrates different approaches to null handling.  

```groovy
def words = [ "falcon", null, "eagle", "hawk", null, "owl", "sparrow" ]

def wordsWithNulls = words.clone()
wordsWithNulls.sort()
println wordsWithNulls

def wordsFiltered = words.findAll { it != null }
wordsFiltered.sort()
println wordsFiltered

def customNullSort = words.sort { a, b ->
    if (a == null && b == null) return 0
    if (a == null) return 1  // nulls last
    if (b == null) return -1
    return a <=> b
}
println customNullSort
```

By default, Groovy places null values at the beginning when sorting. The  
second approach filters out nulls before sorting. The third example shows  
custom null handling, placing nulls at the end of the sorted collection.  

## Reverse sorting

Multiple techniques for sorting collections in descending order, from simple  
reversal to custom comparison logic.  

```groovy
def numbers = [5, 2, 8, 1, 9, 3]

def reversed1 = numbers.sort().reverse()
println "Method 1: ${reversed1}"

def reversed2 = numbers.sort { -it }
println "Method 2: ${reversed2}"

def reversed3 = numbers.sort { a, b -> b <=> a }
println "Method 3: ${reversed3}"

def words = ["zebra", "apple", "mountain", "river"]
def reversedWords = words.sort().reverse()
println "Reversed words: ${reversedWords}"
```

Three approaches are shown: sorting normally then reversing, negating values  
for numeric sorts, and using reverse comparison with the spaceship operator.  
The last example demonstrates reverse alphabetical sorting of strings.  

## Sorting maps by keys

This example demonstrates sorting map entries based on their keys while  
preserving the key-value relationships.  

```groovy
def fruits = [
    banana: 3,
    apple: 7,
    orange: 2,
    grape: 5
]

def sortedByKey = fruits.sort { it.key }
println "Sorted by key: ${sortedByKey}"

def sortedByKeyDesc = fruits.sort { a, b -> b.key <=> a.key }
println "Sorted by key (desc): ${sortedByKeyDesc}"

// Converting to list of entries for more complex operations
def entryList = fruits.entrySet().toList()
entryList.sort { it.key }
println "As entry list: ${entryList}"
```

The first sort arranges map entries alphabetically by key. The second uses  
reverse comparison for descending key order. Converting to entry list  
provides more flexibility for complex sorting scenarios.  

## Sorting maps by values

This example shows how to sort map entries based on their values rather  
than keys, useful for ranking and prioritization scenarios.  

```groovy
def scores = [
    alice: 95,
    bob: 87,
    charlie: 92,
    diana: 98,
    eve: 89
]

def sortedByValue = scores.sort { it.value }
println "Sorted by score (asc): ${sortedByValue}"

def sortedByValueDesc = scores.sort { a, b -> b.value <=> a.value }
println "Sorted by score (desc): ${sortedByValueDesc}"

def topScorers = scores.sort { a, b -> b.value <=> a.value }.take(3)
println "Top 3 scorers: ${topScorers}"
```

Maps are sorted by their numeric values, first in ascending order, then  
descending. The final example combines sorting with `take()` to get the  
top performers, demonstrating practical use cases for value-based sorting.  

## Sorting lists of objects

This example demonstrates sorting custom objects using different properties  
and multiple sorting criteria.  

```groovy
class Student {
    String name
    int age
    double gpa
    
    Student(String name, int age, double gpa) {
        this.name = name
        this.age = age
        this.gpa = gpa
    }
    
    String toString() {
        return "${name} (age: ${age}, gpa: ${gpa})"
    }
}

def students = [
    new Student("Alice", 20, 3.8),
    new Student("Bob", 19, 3.2),
    new Student("Charlie", 21, 3.9),
    new Student("Diana", 20, 3.5)
]

println "Sorted by name:"
students.sort { it.name }.each { println it }

println "\nSorted by age:"
students.sort { it.age }.each { println it }

println "\nSorted by GPA (desc):"
students.sort { a, b -> b.gpa <=> a.gpa }.each { println it }
```

Students are sorted by different properties: name alphabetically, age  
numerically, and GPA in descending order. Each sort demonstrates accessing  
object properties within closure comparators.  

## Multi-field sorting

This example shows how to sort by multiple criteria in order of priority,  
creating complex sorting hierarchies for structured data.  

```groovy
class Employee {
    String department
    String name
    double salary
    
    Employee(String dept, String name, double salary) {
        this.department = dept
        this.name = name
        this.salary = salary
    }
    
    String toString() {
        return "${department}: ${name} (\$${salary})"
    }
}

def employees = [
    new Employee("IT", "Alice", 75000),
    new Employee("HR", "Bob", 65000),
    new Employee("IT", "Charlie", 80000),
    new Employee("HR", "Diana", 70000),
    new Employee("IT", "Eve", 72000)
]

def multiSort = employees.sort { a, b ->
    def deptCmp = a.department <=> b.department
    if (deptCmp != 0) return deptCmp
    
    def salaryCmp = b.salary <=> a.salary  // desc by salary
    if (salaryCmp != 0) return salaryCmp
    
    return a.name <=> b.name  // asc by name
}

multiSort.each { println it }
```

The comparator sorts first by department alphabetically, then by salary in  
descending order, and finally by name alphabetically. This creates a  
three-level hierarchy useful for organizational reporting and analysis.  

## Sorting with custom data types

This example demonstrates sorting collections containing mixed data types  
and custom objects with complex comparison logic.  

```groovy
class Version implements Comparable<Version> {
    int major, minor, patch
    
    Version(String version) {
        def parts = version.split('\\.')
        major = parts[0] as int
        minor = parts[1] as int
        patch = parts[2] as int
    }
    
    int compareTo(Version other) {
        def majorCmp = this.major <=> other.major
        if (majorCmp != 0) return majorCmp
        
        def minorCmp = this.minor <=> other.minor
        if (minorCmp != 0) return minorCmp
        
        return this.patch <=> other.patch
    }
    
    String toString() {
        return "${major}.${minor}.${patch}"
    }
}

def versions = [
    new Version("2.1.0"),
    new Version("1.5.3"),
    new Version("2.0.1"),
    new Version("1.5.10"),
    new Version("2.1.2")
]

versions.sort()
println "Sorted versions: ${versions}"

def versionStrings = ["2.1.0", "1.5.3", "2.0.1", "1.5.10", "2.1.2"]
def sortedStrings = versionStrings.sort { a, b ->
    def v1 = new Version(a)
    def v2 = new Version(b)
    return v1 <=> v2
}
println "Sorted version strings: ${sortedStrings}"
```

The Version class implements semantic version comparison through the  
Comparable interface. String versions are sorted using temporary Version  
objects, demonstrating how to apply custom logic to primitive collections.  

## Stable vs unstable sorting

This example illustrates the difference between stable and unstable sorts,  
showing how element order is preserved for equal values.  

```groovy
class Card {
    String suit
    int rank
    
    Card(String suit, int rank) {
        this.suit = suit
        this.rank = rank
    }
    
    String toString() {
        return "${rank} of ${suit}"
    }
}

def cards = [
    new Card("Hearts", 5),
    new Card("Spades", 3),
    new Card("Hearts", 3),
    new Card("Diamonds", 5),
    new Card("Clubs", 3),
    new Card("Spades", 5)
]

println "Original order:"
cards.each { println it }

println "\nSorted by rank (stable):"
def stableSorted = cards.sort(false) { it.rank }  // false = stable
stableSorted.each { println it }

println "\nSorted by rank then by suit:"
def multiSorted = cards.sort { a, b ->
    def rankCmp = a.rank <=> b.rank
    return rankCmp != 0 ? rankCmp : a.suit <=> b.suit
}
multiSorted.each { println it }
```

The stable sort preserves original ordering for cards with equal ranks.  
The multi-field sort first orders by rank, then by suit for ties,  
demonstrating how to maintain consistent ordering for identical values.  

## Sorting with performance considerations

This example demonstrates different sorting approaches and their performance  
characteristics for large datasets.  

```groovy
def generateRandomNumbers(int count) {
    def random = new Random()
    return (1..count).collect { random.nextInt(1000) }
}

def numbers = generateRandomNumbers(10000)

// Method 1: Default sort (in-place)
def time1 = System.currentTimeMillis()
def sorted1 = numbers.clone()
sorted1.sort()
def elapsed1 = System.currentTimeMillis() - time1
println "In-place sort: ${elapsed1}ms"

// Method 2: Sort with custom comparator
def time2 = System.currentTimeMillis()
def sorted2 = numbers.clone()
sorted2.sort { a, b -> a <=> b }
def elapsed2 = System.currentTimeMillis() - time2
println "Custom comparator: ${elapsed2}ms"

// Method 3: Parallel sort for very large datasets
def time3 = System.currentTimeMillis()
def sorted3 = numbers.toArray()
Arrays.parallelSort(sorted3)
def elapsed3 = System.currentTimeMillis() - time3
println "Parallel sort: ${elapsed3}ms"

println "First 10 elements: ${sorted1.take(10)}"
```

Different sorting methods are compared for performance. In-place sorting is  
typically fastest for moderate datasets. Custom comparators add overhead.  
Parallel sorting can improve performance for very large collections on  
multi-core systems.  

## Natural ordering with custom objects

This example shows how to implement the Comparable interface for natural  
ordering of custom objects without explicit comparators.  

```groovy
import groovy.transform.EqualsAndHashCode

@EqualsAndHashCode
class Product implements Comparable<Product> {
    String name
    double price
    int quantity
    
    Product(String name, double price, int quantity) {
        this.name = name
        this.price = price
        this.quantity = quantity
    }
    
    int compareTo(Product other) {
        def priceCmp = this.price <=> other.price
        if (priceCmp != 0) return priceCmp
        return this.name <=> other.name
    }
    
    String toString() {
        return "${name}: \$${price} (qty: ${quantity})"
    }
}

def products = [
    new Product("Laptop", 999.99, 5),
    new Product("Mouse", 29.99, 20),
    new Product("Keyboard", 79.99, 15),
    new Product("Monitor", 299.99, 8),
    new Product("Headphones", 79.99, 12)
]

products.sort()
println "Products sorted by price then name:"
products.each { println it }

// Can still use custom sorting if needed
products.sort { it.quantity }
println "\nProducts sorted by quantity:"
products.each { println it }
```

Products implement natural ordering by price then name. The `sort()` method  
without arguments uses this natural ordering. Custom closures can still  
override the natural ordering when needed for different sorting criteria.  

## Sorting with method references

This example demonstrates using method references and property access for  
cleaner, more readable sorting code.  

```groovy
def people = [
    [name: "Alice", age: 30, city: "New York"],
    [name: "Bob", age: 25, city: "London"],
    [name: "Charlie", age: 35, city: "Paris"],
    [name: "Diana", age: 28, city: "Tokyo"]
]

// Sort by property using property access
def sortedByAge = people.sort { it.age }
println "Sorted by age:"
sortedByAge.each { println "${it.name}: ${it.age}" }

def sortedByName = people.sort { it['name'] }
println "\nSorted by name:"
sortedByName.each { println "${it.name}: ${it.age}" }

def cities = people.collect { it.city }.sort()
println "\nCities alphabetically: ${cities}"

// Using multiple property access
def sortedMulti = people.sort { a, b ->
    def ageCmp = (a.age as Integer) <=> (b.age as Integer)
    return ageCmp != 0 ? ageCmp : a.name <=> b.name
}
println "\nSorted by age then name:"
sortedMulti.each { println "${it.name}: ${it.age}" }
```

The examples show different ways to access object properties for sorting:  
dot notation, bracket notation, and combining multiple properties. This  
approach works well with maps and objects with dynamic properties.  

## Sorting nested collections

This example shows how to sort collections that contain other collections  
or complex nested data structures.  

```groovy
def departments = [
    [
        name: "Engineering",
        employees: [
            [name: "Alice", salary: 90000],
            [name: "Bob", salary: 85000],
            [name: "Charlie", salary: 95000]
        ]
    ],
    [
        name: "Marketing", 
        employees: [
            [name: "Diana", salary: 70000],
            [name: "Eve", salary: 75000]
        ]
    ],
    [
        name: "Sales",
        employees: [
            [name: "Frank", salary: 65000],
            [name: "Grace", salary: 80000],
            [name: "Henry", salary: 72000]
        ]
    ]
]

// Sort departments by employee count
def sortedByCount = departments.sort { it.employees.size() }
println "Departments by employee count:"
sortedByCount.each { 
    println "${it.name}: ${it.employees.size()} employees" 
}

// Sort departments by average salary
def sortedByAvgSalary = departments.sort { dept ->
    def totalSalary = dept.employees.sum { it.salary }
    return totalSalary / dept.employees.size()
}
println "\nDepartments by average salary:"
sortedByAvgSalary.each { dept ->
    def avg = dept.employees.sum { it.salary } / dept.employees.size()
    println "${dept.name}: \$${avg as int} average"
}

// Sort employees within each department
departments.each { dept ->
    dept.employees.sort { it.salary }
}
println "\nEmployees sorted by salary within departments:"
departments.each { dept ->
    println "${dept.name}:"
    dept.employees.each { emp ->
        println "  ${emp.name}: \$${emp.salary}"
    }
}
```

Nested sorting involves calculating values from inner collections for  
department-level sorting, then sorting elements within each nested  
collection. This pattern is common in hierarchical data processing.  

## Sorting with locale-specific rules

This example demonstrates locale-aware sorting for internationalization  
and proper handling of non-English characters and collation rules.  

```groovy
import java.text.Collator

def germanWords = ["Müller", "Zöller", "Äpfel", "Übung", "müde", "Zürich"]

// Default sorting (may not handle umlauts correctly)
def defaultSort = germanWords.sort()
println "Default sort: ${defaultSort}"

// Locale-specific sorting
def germanCollator = Collator.getInstance(Locale.GERMAN)
def localeSort = germanWords.sort(germanCollator)
println "German locale sort: ${localeSort}"

// Case-insensitive locale sorting
germanCollator.setStrength(Collator.SECONDARY)
def caseInsensitiveSort = germanWords.sort(germanCollator)
println "Case-insensitive German sort: ${caseInsensitiveSort}"

// French words with accents
def frenchWords = ["café", "naïve", "résumé", "élève", "côte", "créer"]
def frenchCollator = Collator.getInstance(Locale.FRENCH)
def frenchSort = frenchWords.sort(frenchCollator)
println "French locale sort: ${frenchSort}"
```

Locale-specific collators ensure proper sorting of international characters  
and accented letters according to language-specific rules. The strength  
setting controls whether case and accents affect comparison results.  

## Grouping and sorting

This example combines grouping operations with sorting to organize and  
order data in multiple dimensions simultaneously.  

```groovy
def sales = [
    [region: "North", product: "Laptop", amount: 15000],
    [region: "South", product: "Mouse", amount: 800],
    [region: "North", product: "Monitor", amount: 8000],
    [region: "East", product: "Laptop", amount: 12000],
    [region: "South", product: "Laptop", amount: 18000],
    [region: "East", product: "Monitor", amount: 6000],
    [region: "North", product: "Mouse", amount: 1200]
]

// Group by region, then sort within groups
def groupedByRegion = sales.groupBy { it.region }
groupedByRegion.each { region, salesData ->
    def sortedSales = salesData.sort { -it.amount }  // desc by amount
    println "${region} region (by amount desc):"
    sortedSales.each { sale ->
        println "  ${sale.product}: \$${sale.amount}"
    }
}

// Sort regions by total sales
def regionTotals = groupedByRegion.collectEntries { region, salesData ->
    [region, salesData.sum { it.amount }]
}
def sortedRegions = regionTotals.sort { -it.value }
println "\nRegions by total sales:"
sortedRegions.each { region, total ->
    println "${region}: \$${total}"
}

// Group by product and sort
def groupedByProduct = sales.groupBy { it.product }
    .collectEntries { product, salesData ->
        [product, salesData.sort { it.region }]
    }
println "\nProducts by region (alphabetical):"
groupedByProduct.each { product, salesData ->
    println "${product}:"
    salesData.each { sale ->
        println "  ${sale.region}: \$${sale.amount}"
    }
}
```

The example demonstrates multi-dimensional data organization: grouping by  
region with internal sorting by amount, calculating totals for region  
comparison, and grouping by product with regional sorting. This pattern  
is essential for data analysis and reporting applications.  

## Advanced sorting with predicates

This final example shows sophisticated sorting using predicates, filters,  
and conditional logic for complex business requirements.  

```groovy
def orders = [
    [id: 1, customer: "Alice", total: 250.0, status: "shipped", priority: "high"],
    [id: 2, customer: "Bob", total: 100.0, status: "pending", priority: "low"],
    [id: 3, customer: "Charlie", total: 500.0, status: "processing", priority: "high"],
    [id: 4, customer: "Diana", total: 75.0, status: "shipped", priority: "medium"],
    [id: 5, customer: "Eve", total: 300.0, status: "pending", priority: "high"],
    [id: 6, customer: "Frank", total: 150.0, status: "processing", priority: "low"]
]

// Priority mapping for custom ordering
def priorityOrder = [high: 1, medium: 2, low: 3]
def statusOrder = [pending: 1, processing: 2, shipped: 3]

// Complex sorting: priority first, then status, then total desc
def complexSort = orders.sort { a, b ->
    def priorityCmp = priorityOrder[a.priority] <=> priorityOrder[b.priority]
    if (priorityCmp != 0) return priorityCmp
    
    def statusCmp = statusOrder[a.status] <=> statusOrder[b.status]
    if (statusCmp != 0) return statusCmp
    
    return b.total <=> a.total  // desc by total
}

println "Orders sorted by business rules:"
complexSort.each { order ->
    println "ID ${order.id}: ${order.customer} - \$${order.total} " +
           "(${order.priority}/${order.status})"
}

// Conditional sorting based on data characteristics
def conditionalSort = orders.sort { a, b ->
    // High-value orders (>200) sorted by total desc
    if (a.total > 200 && b.total > 200) {
        return b.total <=> a.total
    }
    // Low-value orders sorted by customer name
    if (a.total <= 200 && b.total <= 200) {
        return a.customer <=> b.customer
    }
    // Mixed: high-value orders come first
    return (b.total > 200 ? 1 : 0) <=> (a.total > 200 ? 1 : 0)
}

println "\nConditional sorting (high-value first by amount, low-value by name):"
conditionalSort.each { order ->
    println "ID ${order.id}: ${order.customer} - \$${order.total}"
}

// Filtering and sorting combination
def urgentOrders = orders
    .findAll { it.priority == "high" || it.total > 400 }
    .sort { a, b ->
        // Urgent orders: pending status first, then by total desc
        def statusWeight = [pending: 0, processing: 1, shipped: 2]
        def statusCmp = statusWeight[a.status] <=> statusWeight[b.status]
        return statusCmp != 0 ? statusCmp : b.total <=> a.total
    }

println "\nUrgent orders (high priority or >$400):"
urgentOrders.each { order ->
    println "ID ${order.id}: ${order.customer} - \$${order.total} " +
           "(${order.status})"
}
```

This comprehensive example demonstrates real-world sorting scenarios:  
custom priority orderings using lookup maps, conditional sorting logic  
based on data values, and combining filtering with sorting for targeted  
data processing. These patterns are essential for business applications  
requiring complex data organization rules.  
