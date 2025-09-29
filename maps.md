# Maps 

Maps in Groovy are key-value data structures that provide efficient lookup,  
storage, and manipulation of data pairs. A map associates keys with values,  
where each key is unique and can be used to retrieve its corresponding value.  
In Groovy, maps are implemented as instances of java.util.LinkedHashMap by  
default, which maintains insertion order while providing O(1) average-time  
complexity for basic operations like get, put, and remove.  

Maps are fundamental data structures in Groovy programming, offering powerful  
methods for data manipulation, filtering, transformation, and aggregation.  
They can store any type of object as both keys and values, making them  
incredibly versatile for various programming scenarios. Groovy enhances the  
standard Java Map interface with additional methods and syntactic sugar that  
makes working with maps more intuitive and expressive.  

The syntax for creating maps in Groovy is concise and readable, using square  
brackets with colon-separated key-value pairs. Keys can be strings, numbers,  
or any object, and Groovy automatically converts string keys to avoid common  
quoting requirements. This flexibility, combined with Groovy's closure support,  
enables powerful functional programming patterns for map manipulation.  

## Basics

Basic map operations include creation, access, and simple modifications.  
Maps store key-value pairs and provide constant-time access to values.  

```groovy
def cts = [ sk: 'Slovakia', ru: 'Russia', de: 'Germany', no: 'Norway' ]

println cts['sk']
println cts.get('sk')
println cts.size()

cts.put('hu', 'Hungary')
println cts.containsKey('hu')
```

The example demonstrates different ways to access map values and basic  
operations like size checking and key existence verification.  

## Map creation variations

Different ways to create maps in Groovy, including empty maps,  
explicit type declarations, and various constructor approaches.  

```groovy
// Empty map
def empty = [:]
def empty2 = new HashMap()

// With initial values
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF']
def numbers = [1: 'one', 2: 'two', 3: 'three']

// Explicit typing
Map<String, Integer> ages = [john: 25, jane: 30, bob: 35]
HashMap<Integer, String> lookup = [100: 'hundred', 200: 'two hundred']

println colors
println numbers.getClass()
```

This shows various map creation patterns, from simple literals to  
typed declarations, demonstrating Groovy's flexibility in map handling.  

## Map access patterns

Multiple ways to access and retrieve values from maps,  
including safe navigation and default values.  

```groovy
def person = [name: 'John Doe', age: 35, city: 'New York']

// Different access methods
println person.name
println person['age']
println person.get('city')

// Safe navigation
println person?.occupation
println person?.name?.length()

// With default values
println person.get('salary', 0)
println person.getOrDefault('department', 'Unknown')

// Multiple key access
def keys = ['name', 'age']
println keys.collect { person[it] }
```

These patterns show robust ways to access map data, handling missing keys  
gracefully and providing fallback values when needed.  

## Traversing 

Iterating through maps using various methods including each, eachWithIndex,  
and accessing keys and values separately.  

```groovy
def capitals = [ Bratislava: 424207, Vilnius: 556723, Lisbon: 564657,
    Riga: 713016, Jerusalem: 780200, Warsaw: 1711324,
    Budapest: 1729040, Prague: 1241664, Helsinki: 596661,
    Tokyo: 13189000, Madrid: 3233527 ]

capitals.each { e -> 
    println "$e.key $e.value"
}

println "-----------------------------"

capitals.each { k, v -> 
    println "$k $v"
}

println "-----------------------------"

capitals.eachWithIndex { k, v, i -> 
    println "$i $k $v"
}
```

The each method provides flexible iteration patterns, allowing access to  
map entries, separate keys and values, or indexed iteration.  

## Advanced traversal patterns

More sophisticated ways to iterate and process map data,  
including filtering during iteration and parallel processing.  

```groovy
def scores = [alice: 95, bob: 78, charlie: 88, diana: 92, eve: 76]

// Iterate with conditions
scores.each { name, score ->
    if (score >= 90) {
        println "$name: Excellent ($score)"
    } else if (score >= 80) {
        println "$name: Good ($score)"
    }
}

// Reverse iteration (LinkedHashMap maintains order)
scores.reverseEach { name, score ->
    println "Reverse: $name -> $score"
}

// Iterate over keys and values separately
println "Keys: ${scores.keySet().join(', ')}"
println "Values: ${scores.values().join(', ')}"

// EntrySet iteration
scores.entrySet().each { entry ->
    println "Entry: ${entry.key} = ${entry.value}"
}
```

These patterns demonstrate conditional processing during iteration and  
different ways to access map components for specialized processing needs.  

## Sorting

Sorting maps by keys or values using built-in sort methods.  
Maps can be sorted with custom comparators and closures.  

```groovy
def capitals = [ Bratislava: 424207, Vilnius: 556723, Lisbon: 564657,
    Riga: 713016, Jerusalem: 780200, Warsaw: 1711324,
    Budapest: 1729040, Prague: 1241664, Helsinki: 596661,
    Tokyo: 13189000, Madrid: 3233527 ]
    
println capitals.sort { it.key }
println capitals.sort { it.value }
println capitals.sort { a, b -> b.value <=> a.value }
```

The sort method creates new sorted maps based on key or value comparison,  
with custom comparators for descending or complex sorting logic.  

## Complex sorting patterns

Advanced sorting techniques including multi-level sorting,  
custom comparators, and sorting by transformed values.  

```groovy
def employees = [
    john: [age: 30, salary: 50000, department: 'IT'],
    jane: [age: 25, salary: 55000, department: 'HR'],
    bob: [age: 35, salary: 48000, department: 'IT'],
    alice: [age: 28, salary: 52000, department: 'Finance']
]

// Sort by nested property
def byAge = employees.sort { a, b -> a.value.age <=> b.value.age }
println "By age: $byAge"

// Sort by salary descending
def bySalary = employees.sort { a, b -> b.value.salary <=> a.value.salary }
println "By salary (desc): $bySalary"

// Multi-level sort: department then salary
def multiSort = employees.sort { a, b ->
    def deptCompare = a.value.department <=> b.value.department
    deptCompare != 0 ? deptCompare : b.value.salary <=> a.value.salary
}
println "Multi-level sort: $multiSort"
```

Complex sorting enables sophisticated data organization based on nested  
properties, multiple criteria, and custom business logic.  

## Finding 

Finding specific entries in maps using find and findAll methods.  
These methods work with closures to locate matching elements.  

```groovy
def users = [
   [fname: "Robert", lname: "Novak", salary: 1770],
   [fname: "John", lname:"Doe", salary: 1230],
   [fname: "Lucy", lname:"Novak", salary: 670],
   [fname: "Ben", lname:"Walter", salary: 2050],
   [fname: "Robin",lname: "Brown", salary: 2300],
   [fname: "Amy",lname: "Doe", salary: 1250],
   [fname: "Joe", lname:"Draker", salary: 1190],
   [fname: "Janet", lname:"Doe", salary: 980],
   [fname: "Peter",lname: "Novak", salary: 990],
   [fname:"Albert", lname:"Novak",salary: 193]
]

println users.find { u -> u.salary < 1000 }
println users.findAll { u -> u.salary < 1000 }
```

The find method returns the first matching element, while findAll  
returns all matching elements as a new collection.  

## Advanced finding and filtering

Complex search operations using various map methods  
for sophisticated data retrieval and filtering.  

```groovy
def products = [
    laptop: [price: 999, category: 'electronics', rating: 4.5],
    desk: [price: 299, category: 'furniture', rating: 4.2],
    phone: [price: 699, category: 'electronics', rating: 4.8],
    chair: [price: 199, category: 'furniture', rating: 4.0],
    tablet: [price: 399, category: 'electronics', rating: 4.3]
]

// Find by key pattern
def electronics = products.findAll { k, v -> 
    v.category == 'electronics' 
}
println "Electronics: $electronics"

// Find by multiple criteria
def premiumElectronics = products.findAll { k, v ->
    v.category == 'electronics' && v.price > 500 && v.rating > 4.0
}
println "Premium electronics: $premiumElectronics"

// Find key by value
def expensiveProduct = products.find { k, v -> v.price > 800 }?.key
println "Expensive product: $expensiveProduct"

// Any/All matching
def hasExpensive = products.any { k, v -> v.price > 900 }
def allRated = products.every { k, v -> v.rating > 3.0 }
println "Has expensive: $hasExpensive, All rated: $allRated"
```

These advanced patterns enable complex searches with multiple conditions  
and boolean logic for comprehensive data analysis.  

## Counting 

Counting elements in maps and lists of maps using count and countBy methods.  
These methods provide statistical information about data collections.  

```groovy
def users = [
   [fname: "Robert", lname: "Novak", salary: 1770],
   [fname: "John", lname:"Doe", salary: 1230],
   [fname: "Lucy", lname:"Novak", salary: 670],
   [fname: "Ben", lname:"Walter", salary: 2050],
   [fname: "Robin",lname: "Brown", salary: 2300],
   [fname: "Amy",lname: "Doe", salary: 1250],
   [fname: "Joe", lname:"Draker", salary: 1190],
   [fname: "Janet", lname:"Doe", salary: 980],
   [fname: "Peter",lname: "Novak", salary: 990],
   [fname:"Albert", lname:"Novak",salary: 193]
]

println users.count { u -> u.lname == 'Novak' || u.lname == 'Doe' }
println users.countBy { u -> u.salary < 1000 }
```

The count method returns the number of elements matching a condition,  
while countBy groups elements by a classification function.  

## Statistical operations

Advanced counting and statistical operations on map data,  
including frequency analysis and distribution calculations.  

```groovy
def sales = [
    jan: 15000, feb: 18000, mar: 22000, apr: 19000,
    may: 25000, jun: 28000, jul: 31000, aug: 29000,
    sep: 26000, oct: 23000, nov: 27000, dec: 32000
]

// Count by range
def highSales = sales.count { k, v -> v > 25000 }
def mediumSales = sales.count { k, v -> v >= 20000 && v <= 25000 }
def lowSales = sales.count { k, v -> v < 20000 }

println "High: $highSales, Medium: $mediumSales, Low: $lowSales"

// Frequency distribution
def ranges = sales.countBy { k, v ->
    if (v < 20000) 'Low'
    else if (v <= 25000) 'Medium'
    else 'High'
}
println "Sales distribution: $ranges"

// Min/Max counting
def minSales = sales.min { it.value }
def maxSales = sales.max { it.value }
println "Min sales: ${minSales.key} = ${minSales.value}"
println "Max sales: ${maxSales.key} = ${maxSales.value}"
```

Statistical operations provide insights into data distribution and help  
identify patterns, outliers, and performance metrics in map data.  

## collect & groupBy

Transforming and grouping map data using collect and groupBy methods.  
These methods enable data aggregation and restructuring operations.  

```groovy
def users = [
   [fname: "Robert", lname: "Novak", salary: 1770],
   [fname: "John", lname: "Doe", salary: 1230],
   [fname: "Lucy", lname: "Novak", salary: 670],
   [fname: "Ben", lname: "Walter", salary: 2050],
   [fname: "Robin",lname: "Brown", salary: 2300],
   [fname: "Amy",lname: "Doe", salary: 1250],
   [fname: "Joe", lname: "Draker", salary: 1190],
   [fname: "Janet", lname: "Doe", salary: 980],
   [fname: "Peter", lname: "Novak", salary: 990],
   [fname: "Albert", lname: "Novak",salary: 193]
]

println users.groupBy { it.lname }

println '--------------------------'

println users.collect { [it.fname, it.lname, it.salary * 1.1] }

println '--------------------------'

println users.collect { ["$it.fname $it.lname", it.salary * 1.1] }
```

The groupBy method organizes data by classification criteria, while collect  
transforms each element to create new data structures.  

## Advanced transformations

Complex data transformations using collectEntries, inject,  
and combination of multiple collection operations.  

```groovy
def inventory = [
    apples: [price: 2.50, quantity: 100, category: 'fruit'],
    bread: [price: 1.80, quantity: 50, category: 'bakery'],
    milk: [price: 3.20, quantity: 30, category: 'dairy'],
    cheese: [price: 8.50, quantity: 20, category: 'dairy'],
    oranges: [price: 3.00, quantity: 80, category: 'fruit']
]

// Transform to price list
def priceList = inventory.collectEntries { k, v ->
    [k, v.price]
}
println "Price list: $priceList"

// Calculate total value per item
def totalValues = inventory.collectEntries { k, v ->
    [k, v.price * v.quantity]
}
println "Total values: $totalValues"

// Group by category with aggregation
def categoryTotals = inventory.groupBy { k, v -> v.category }
    .collectEntries { category, items ->
        def total = items.values().sum { it.price * it.quantity }
        [category, total]
    }
println "Category totals: $categoryTotals"

// Complex transformation with multiple operations
def summary = inventory.collect { k, v ->
    [
        name: k,
        value: v.price * v.quantity,
        status: v.quantity > 50 ? 'In Stock' : 'Low Stock'
    ]
}.groupBy { it.status }
println "Inventory summary: $summary"
```

Advanced transformations combine multiple operations to create sophisticated  
data pipelines and analytical summaries from raw map data.  

## Map modification operations

Comprehensive set of operations for modifying maps,  
including adding, removing, and updating entries safely.  

```groovy
def config = [host: 'localhost', port: 8080, ssl: false]

// Adding entries
config.put('timeout', 30)
config['retries'] = 3
config.putAll([debug: true, verbose: false])

println "After additions: $config"

// Updating entries
config.replace('port', 8443)
config.ssl = true
config.compute('timeout') { k, v -> v * 2 }

println "After updates: $config"

// Conditional operations
config.putIfAbsent('database', 'embedded')
config.computeIfAbsent('cache') { 'redis' }
config.computeIfPresent('retries') { k, v -> v + 1 }

println "After conditional ops: $config"

// Removing entries
config.remove('verbose')
def oldPort = config.remove('port', 8443)
config.removeAll { k, v -> v == false }

println "After removals: $config"
println "Old port was: $oldPort"
```

Modification operations provide safe and efficient ways to update map  
contents with atomic operations and conditional logic.  

## Nested maps and deep operations

Working with nested map structures and performing deep operations  
on hierarchical data with safe navigation and manipulation.  

```groovy
def company = [
    name: 'Tech Corp',
    departments: [
        engineering: [
            manager: 'Alice Johnson',
            employees: [
                [name: 'Bob Smith', role: 'Developer', salary: 75000],
                [name: 'Carol White', role: 'Senior Dev', salary: 85000]
            ],
            budget: 500000
        ],
        marketing: [
            manager: 'Dave Brown',
            employees: [
                [name: 'Eve Davis', role: 'Marketing Lead', salary: 70000],
                [name: 'Frank Wilson', role: 'Designer', salary: 60000]
            ],
            budget: 300000
        ]
    ]
]

// Deep access with safe navigation
println "Engineering manager: ${company.departments?.engineering?.manager}"
println "Total employees: ${company.departments.values().sum { it.employees.size() }}"

// Deep transformation
def salaryReport = company.departments.collectEntries { deptName, dept ->
    def totalSalary = dept.employees.sum { it.salary }
    def avgSalary = totalSalary / dept.employees.size()
    [deptName, [total: totalSalary, average: avgSalary, count: dept.employees.size()]]
}
println "Salary report: $salaryReport"

// Deep finding
def highEarners = company.departments.collectMany { deptName, dept ->
    dept.employees.findAll { it.salary > 70000 }
        .collect { it + [department: deptName] }
}
println "High earners: $highEarners"

// Nested updates
company.departments.each { deptName, dept ->
    dept.employees.each { employee ->
        employee.adjustedSalary = employee.salary * 1.05
    }
}
println "After salary adjustment: ${company.departments.engineering.employees}"
```

Nested operations enable complex data processing on hierarchical structures  
while maintaining readability and safety through Groovy's navigation features.  

## Map comparison and equality

Comparing maps for equality, subset relationships,  
and finding differences between map structures.  

```groovy
def map1 = [a: 1, b: 2, c: 3]
def map2 = [a: 1, b: 2, c: 3]
def map3 = [a: 1, b: 2, c: 4]
def map4 = [a: 1, b: 2]

// Equality comparison
println "map1 == map2: ${map1 == map2}"
println "map1 == map3: ${map1 == map3}"
println "map1.equals(map2): ${map1.equals(map2)}"

// Subset checking
println "map4 subset of map1: ${map4.every { k, v -> map1[k] == v }}"

// Finding differences
def diff = map1.findAll { k, v -> map3[k] != v }
println "Differences: $diff"

// Intersection and union
def common = map1.findAll { k, v -> map3.containsKey(k) }
def union = map1 + map3
println "Common keys: $common"
println "Union: $union"
```

Map comparison operations enable data validation, synchronization,  
and analysis of changes between different map versions.  

## Map aggregation and reduction

Performing aggregation operations on maps using inject, sum,  
and other reduction methods for statistical analysis.  

```groovy
def sales = [
    Q1: [jan: 15000, feb: 18000, mar: 22000],
    Q2: [apr: 19000, may: 25000, jun: 28000],
    Q3: [jul: 31000, aug: 29000, sep: 26000],
    Q4: [oct: 23000, nov: 27000, dec: 32000]
]

// Total across all quarters
def totalSales = sales.values().sum { quarter ->
    quarter.values().sum()
}
println "Total sales: $totalSales"

// Average per quarter
def quarterlyTotals = sales.collectEntries { quarter, months ->
    [quarter, months.values().sum()]
}
def avgQuarterly = quarterlyTotals.values().sum() / quarterlyTotals.size()
println "Average quarterly: $avgQuarterly"

// Using inject for complex aggregation
def stats = sales.inject([total: 0, months: 0, max: 0]) { acc, quarter, months ->
    def quarterTotal = months.values().sum()
    def quarterMax = months.values().max()
    [
        total: acc.total + quarterTotal,
        months: acc.months + months.size(),
        max: Math.max(acc.max, quarterMax)
    ]
}
println "Aggregated stats: $stats"

// Monthly performance analysis
def monthlyAnalysis = sales.collectMany { quarter, months ->
    months.collect { month, amount ->
        [quarter: quarter, month: month, amount: amount]
    }
}.groupBy { it.amount > 25000 ? 'High' : 'Normal' }
println "Performance analysis: $monthlyAnalysis"
```

Aggregation operations provide powerful tools for statistical analysis  
and business intelligence reporting on complex map-based datasets.  

## Map serialization patterns

Converting maps to and from various formats including JSON-like strings,  
properties, and custom serialization approaches.  

```groovy
def user = [
    id: 12345,
    name: 'John Doe',
    email: 'john@example.com',
    preferences: [
        theme: 'dark',
        language: 'en',
        notifications: true
    ],
    roles: ['user', 'admin']
]

// Convert to string representation
def userString = user.inspect()
println "Inspect: $userString"

// Convert to properties-like format
def propsFormat = user.collectEntries { k, v ->
    if (v instanceof Map) {
        v.collectEntries { nk, nv -> ["$k.$nk", nv] }
    } else if (v instanceof List) {
        [(k): v.join(',')]
    } else {
        [(k): v.toString()]
    }
}.flatten()
println "Properties format: $propsFormat"

// Custom serialization
def serialize = { map ->
    map.collect { k, v ->
        def value = v instanceof Map ? serialize(v) :
                   v instanceof List ? "[${v.join(',')}]" :
                   v.toString()
        "$k=$value"
    }.join('&')
}
println "Custom serialized: ${serialize(user)}"

// Parse back simple key-value pairs
def parseSimple = { str ->
    str.split('&').collectEntries { pair ->
        def (k, v) = pair.split('=', 2)
        [k, v]
    }
}
def simple = "name=Jane&age=30&active=true"
println "Parsed: ${parseSimple(simple)}"
```

Serialization patterns enable data persistence, transmission, and  
interoperability between different systems and formats.  

## Performance considerations

Optimizing map operations for performance, memory usage,  
and choosing appropriate map implementations for different use cases.  

```groovy
import java.util.concurrent.ConcurrentHashMap
import java.util.TreeMap

// Different map implementations
def linkedMap = [:]  // LinkedHashMap (default)
def hashMap = new HashMap()
def treeMap = new TreeMap()
def concurrentMap = new ConcurrentHashMap()

// Performance comparison setup
def testData = (1..1000).collectEntries { [it, "value$it"] }

// Bulk operations performance
def startTime = System.currentTimeMillis()
testData.each { k, v -> linkedMap[k] = v }
def linkedTime = System.currentTimeMillis() - startTime

startTime = System.currentTimeMillis()
hashMap.putAll(testData)
def hashTime = System.currentTimeMillis() - startTime

println "LinkedHashMap time: ${linkedTime}ms"
println "HashMap time: ${hashTime}ms"

// Memory-efficient operations
def largeMap = (1..10000).collectEntries { [it, "data$it"] }

// Stream-like processing for memory efficiency
def result = largeMap
    .findAll { k, v -> k % 100 == 0 }
    .collectEntries { k, v -> [k, v.toUpperCase()] }
println "Filtered result size: ${result.size()}"

// Lazy evaluation patterns
def lazyProcessing = { map ->
    map.lazy()
       .filter { k, v -> k > 5000 }
       .map { k, v -> [k, v.reverse()] }
       .take(5)
       .collectEntries()
}
def lazResult = lazyProcessing(largeMap)
println "Lazy result: $lazResult"

// Best practices
def optimizedMap = new LinkedHashMap(16, 0.75f)  // Initial capacity
optimizedMap.putAll([a: 1, b: 2, c: 3])
println "Optimized map: $optimizedMap"
```

Performance considerations help choose the right map implementation  
and optimization strategies for specific application requirements.  

## Map filtering with grep

Using grep for pattern-based filtering of maps,  
supporting regular expressions and complex matching criteria.  

```groovy
def products = [
    'laptop-dell-xps': [price: 1299, brand: 'Dell'],
    'laptop-hp-pavilion': [price: 799, brand: 'HP'],
    'desktop-mac-mini': [price: 699, brand: 'Apple'],
    'tablet-ipad-pro': [price: 999, brand: 'Apple'],
    'phone-iphone-14': [price: 899, brand: 'Apple']
]

// Filter by key pattern
def laptops = products.findAll { k, v -> k.startsWith('laptop') }
println "Laptops: $laptops"

// Filter by brand using grep on values
def appleProducts = products.findAll { k, v -> v.brand == 'Apple' }
println "Apple products: $appleProducts"

// Complex pattern matching
def premiumProducts = products.grep { k, v ->
    v.price > 800 && k.contains('laptop')
}
println "Premium laptops: $premiumProducts"

// Regular expression filtering on keys
def mobileDevices = products.findAll { k, v ->
    k.matches('.*(phone|tablet).*')
}
println "Mobile devices: $mobileDevices"
```

Grep-style filtering enables powerful pattern matching and selection  
based on both keys and values using flexible criteria.  

## Map validation and constraints

Implementing validation rules and constraints for map data,  
ensuring data integrity and business rule compliance.  

```groovy
def validateUser = { user ->
    def errors = []
    
    if (!user.name || user.name.trim().isEmpty()) {
        errors << "Name is required"
    }
    if (!user.email || !user.email.contains('@')) {
        errors << "Valid email is required"
    }
    if (user.age && (user.age < 18 || user.age > 120)) {
        errors << "Age must be between 18 and 120"
    }
    
    return [valid: errors.isEmpty(), errors: errors]
}

def users = [
    [name: 'John Doe', email: 'john@example.com', age: 30],
    [name: '', email: 'invalid-email', age: 15],
    [name: 'Jane Smith', email: 'jane@example.com', age: 25],
    [name: 'Bob Wilson', email: 'bob@test.com', age: 150]
]

users.each { user ->
    def result = validateUser(user)
    println "User: $user.name - Valid: $result.valid"
    if (!result.valid) {
        println "Errors: ${result.errors.join(', ')}"
    }
}

// Schema validation
def validateSchema = { data, schema ->
    schema.every { field, rules ->
        def value = data[field]
        rules.every { rule ->
            switch(rule.type) {
                case 'required': return value != null
                case 'type': return value == null || value.getClass() == rule.value
                case 'range': return value == null || (value >= rule.min && value <= rule.max)
                default: return true
            }
        }
    }
}

def schema = [
    name: [[type: 'required'], [type: 'type', value: String]],
    age: [[type: 'type', value: Integer], [type: 'range', min: 0, max: 150]]
]

def testData = [name: 'Alice', age: 28]
println "Schema valid: ${validateSchema(testData, schema)}"
```

Validation patterns ensure data quality and business rule compliance  
through systematic checking and constraint enforcement.  

## Map utility operations

Utility operations for common map manipulation tasks including  
merging, cloning, and specialized data processing functions.  

```groovy
def map1 = [a: 1, b: 2, c: 3]
def map2 = [c: 4, d: 5, e: 6]
def map3 = [f: 7, g: 8]

// Deep clone
def cloned = map1.collectEntries { k, v ->
    [k, v instanceof Map ? v.clone() : v]
}
println "Cloned: $cloned"

// Merge multiple maps with conflict resolution
def merged = [map1, map2, map3].inject([:]) { result, map ->
    map.each { k, v ->
        if (result.containsKey(k)) {
            result[k] = result[k] + v  // Add values for conflicts
        } else {
            result[k] = v
        }
    }
    result
}
println "Merged with addition: $merged"

// Selective merge
def selectiveMerge = { maps, selector ->
    maps.collectMany { it.entrySet() }
        .groupBy { it.key }
        .collectEntries { k, entries ->
            [k, selector(entries.collect { it.value })]
        }
}

def maxMerge = selectiveMerge([map1, map2], { values -> values.max() })
println "Max merge: $maxMerge"

// Map inversion (swap keys and values)
def inverted = map1.collectEntries { k, v -> [v, k] }
println "Inverted: $inverted"

// Partition map based on predicate
def partitioned = map1.groupBy { k, v -> v > 2 }
def [higher, lower] = [partitioned[true] ?: [:], partitioned[false] ?: [:]]
println "Higher values: $higher"
println "Lower values: $lower"
```

Utility operations provide reusable functions for common map manipulation  
patterns and complex data restructuring tasks.  
