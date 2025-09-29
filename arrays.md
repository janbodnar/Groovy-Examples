# Arrays 

Arrays are collections of fixed size that hold elements of the same type.  
In Groovy, arrays are objects that inherit from Java arrays, providing  
additional methods and functionality. Unlike lists, arrays have a fixed  
size that is determined at creation time and cannot be changed. Arrays  
provide indexed access to elements, starting from index 0.  

Groovy arrays offer several advantages: they provide fast element access  
through indexing, consume less memory than lists for storing primitive  
types, and integrate seamlessly with Java code. Arrays support various  
data types including primitives (int[], double[], boolean[]) and object  
references (String[], Object[]).  

Array elements can be accessed, modified, and processed using Groovy's  
rich collection methods. The language provides syntactic sugar for array  
operations, making array manipulation both powerful and expressive.  
Arrays can be created through literal notation, explicit constructors,  
or by converting from other collection types.  

## List vs array

Lists and arrays have the same initializers; therefore, we need to  
type it to an array.  

```groovy
def nums = [1, 2, 3, 4, 5]
println nums.class.array

def nums2 = [1, 2, 3, 4, 5] as int[]
println nums2.class.array
```

The array type can be provided on the left or on the right side.  

```groovy
def vals = [1, 2, 3, 4, 5] as int[]
println vals
println vals.length
println vals.size()

def words = new String[] { 'sky', 'cloud', 'pen' }
println words
```

```groovy
println nums.asType(List<Integer>)
println nums as List<Integer>
```

## Array via range

Defining an array through range.  

```groovy
int[] nums = -3..2
println nums
```

## size, min, max, sum, average, count

```groovy
int[] nums = -4..10

println nums.size()
println nums.min()
println nums.max()
println nums.sum()
println nums.average()
println nums.count(-4)
```

```groovy
String[] words = ['sky', 'waterfall', 'superintendent', 'war', 'count', 'up',
    'existence', 'powerful']

println words.max { it.size() }
println words.min { it.size() }
```

## Element access

Element access via index `[]` operator or via `getAt` method.  

```groovy
int[] nums = -3..2

println nums[0]
println nums[1]
println nums.getAt(0)
println nums.getAt(1)
println nums.getAt(-1..1)

println '------------------------'

String[] words = ['borrow', 'water', 'globe']
println words[0]
println words[-1]
println words[0..2]
```

## Iteration

```groovy
def nums = [1, 2, 3, 4, 5] as int[]

for (def e in nums) {
    println e
}

def words = ['sky', 'blue', 'war', 'water', 'coffee'] as String[]

words.each {e -> println e}
words.eachWithIndex { e, i -> println "${i}: ${e}" }
```

## Check element existence

Using `contains`.  

```groovy
String[] words = ['sky', 'water', 'war', 'count', 'array', 'pen',
        'cloud', 'top', 'ten', 'warm', 'cup', 'coin']

println words.contains('war')
println words.contains('small')
```

## Remove element 

Array elements can be modified but not deleted. The `-` operator  
creates a new array without the given element.  

```groovy
String[] words = ['sky', 'water', 'war', 'cup']

def words2 = words - 'war'

println words
println words2

words[0] = 'skylark'
println words
```

## Shuffle elements

Randomizing the order of array elements using shuffle method.  

```groovy
String[] words = ['sky', 'cloud', 'pen']
println words

words.shuffle()
println words

words.shuffle()
println words

words.shuffle()
println words
```

The shuffle method modifies the array in-place, changing the order of  
elements randomly. Each call produces a different arrangement, making  
it useful for randomization and testing scenarios.

## Sorting 

The `sort` method sorts the array in-place.  

```groovy
String[] words = ['sky', 'water', 'war', 'count', 'array', 'pen',
        'cloud', 'top', 'ten', 'warm', 'cup', 'coin']

println words.sort()

println '----------------------------'

println words.reverse()
```

## Filtering

Filtering is done with `grep`.  

```groovy
String[] words = ['sky', 'water', 'war', 'count', 'array', 'pen',
        'cloud', 'top', 'ten', 'warm', 'cup', 'coin']

def res = words.grep { e -> e.startsWith('w') || e.startsWith('c') }
println res
```

The grep method filters array elements based on a condition specified  
in the closure. It returns a new collection containing only elements  
that satisfy the given predicate, leaving the original array unchanged.

## Mapping

The map operation is done with `collect`.  

```groovy
int[] nums = -4..10

def res = nums.collect { it * it } 
println res
```

The collect method transforms each array element by applying the given  
closure and returns a new collection with the transformed values. This  
is useful for converting arrays to different types or applying calculations.

## Finding elements

Locating specific elements within arrays using find methods.  

```groovy
int[] nums = -4..10

def res = nums.findAll(e -> e % 2 == 0)
println res

res = nums.find { it > 0 } // get fst element giving true
println res
```

The findAll method returns all elements matching the condition, while  
find returns only the first matching element. These methods are essential  
for searching and extracting data from arrays based on specific criteria.

## Joining elements 

Combining array elements into a single string with separators.  

```groovy
String[] words = ['sky', 'waterfall', 'superintendent', 'war', 'count', 'up',
    'existence', 'powerful']

def res = words.join(', ')
println res
```

The join method concatenates all array elements into a string using the  
specified delimiter. This is particularly useful for creating formatted  
output, CSV data, or converting arrays to readable text representations.

## The any/every methods 

Testing array elements against conditions with boolean logic.  

```groovy
int[] nums = -4..10

def res = nums.any { it > 0 } // are there any positive values?
println res

res = nums.every { it > 0 } // is every element positive?
println res
```

The any method returns true if at least one element satisfies the  
condition, while every returns true only if all elements satisfy it.  
These methods provide efficient ways to validate array contents.

## Flatten array

Converting multi-dimensional arrays into single-dimensional arrays.  

```groovy
int[][] nums = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

println nums
println nums.flatten()
```

The flatten method recursively converts nested arrays into a single flat  
array containing all elements. This is useful when working with  
multi-dimensional data that needs to be processed as a linear sequence.

## Array initialization methods

Different ways to create and initialize arrays in Groovy.  

```groovy
// Using array literal with explicit type casting
int[] nums1 = [1, 2, 3, 4, 5] as int[]

// Using explicit array constructor
String[] words1 = new String[] { 'falcon', 'eagle', 'hawk' }

// Array of specified size with default values
double[] doubles = new double[5]
println doubles

// Initializing with repeated values
boolean[] flags = [true] * 10
println flags
```

## Multi-dimensional arrays

Working with arrays of arrays for complex data structures.  

```groovy
// 2D integer array
int[][] matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

// Accessing 2D array elements
println matrix[1][2]  // prints 6

// 3D array example
String[][][] cube = [[['a', 'b'], ['c', 'd']], [['e', 'f'], ['g', 'h']]]
println cube[0][1][0]  // prints 'c'

// Creating jagged arrays (arrays with different lengths)
int[][] jagged = new int[3][]
jagged[0] = [1, 2] as int[]
jagged[1] = [3, 4, 5, 6] as int[]
jagged[2] = [7] as int[]
println jagged
```

## Array conversion operations

Converting between different array types and collections.  

```groovy
// Converting string array to list
String[] words = ['mountain', 'valley', 'river', 'ocean']
List<String> wordList = words as List<String>
println wordList.class

// Converting list back to array
def backToArray = wordList as String[]
println backToArray.class.array

// Converting between primitive and object arrays
int[] primitives = [1, 2, 3, 4, 5] as int[]
Integer[] objects = primitives as Integer[]
println objects.class
```

## Array slicing and ranges

Extracting sub-arrays and working with array segments.  

```groovy
int[] numbers = [10, 20, 30, 40, 50, 60, 70, 80, 90] as int[]

// Get slice from index 2 to 5
println numbers[2..5]

// Get slice with step
println numbers[0..8:2]  // every second element

// Negative indices
println numbers[-3..-1]  // last three elements

// Using getAt with ranges
println numbers.getAt(1..4)

// First and last n elements
println numbers.take(3)  // first 3
println numbers.takeRight(3)  // last 3
```

## Array comparison operations

Comparing arrays for equality and ordering.  

```groovy
int[] array1 = [1, 2, 3, 4, 5] as int[]
int[] array2 = [1, 2, 3, 4, 5] as int[]
int[] array3 = [5, 4, 3, 2, 1] as int[]

// Array equality
println array1 == array2  // true
println array1 == array3  // false

// Element-wise comparison
println array1.equals(array2)
println Arrays.equals(array1, array2)

// Comparing arrays lexicographically
String[] words1 = ['apple', 'banana'] as String[]
String[] words2 = ['apple', 'cherry'] as String[]
println words1 <=> words2  // -1 (less than)
```

## Array concatenation and merging

Combining multiple arrays into single arrays.  

```groovy
int[] first = [1, 2, 3] as int[]
int[] second = [4, 5, 6] as int[]
int[] third = [7, 8, 9] as int[]

// Simple concatenation using plus operator
def combined = first + second
println combined

// Multiple array concatenation
def allCombined = first + second + third
println allCombined

// Concatenating with single elements
def withElement = first + 99
println withElement

// Using leftShift operator for building arrays
def builder = [] as int[]
builder += [1, 2, 3]
builder += [4, 5, 6]
println builder
```

## Array intersection and difference

Finding common elements and differences between arrays.  

```groovy
String[] set1 = ['apple', 'banana', 'cherry', 'date'] as String[]
String[] set2 = ['banana', 'date', 'elderberry', 'fig'] as String[]

// Intersection (common elements)
def common = set1.intersect(set2)
println common

// Difference (elements in first but not in second)
def difference = set1 - set2
println difference

// Union (all unique elements from both arrays)
def union = (set1 + set2).unique()
println union

// Symmetric difference (elements in either but not both)
def symmetricDiff = (set1 - set2) + (set2 - set1)
println symmetricDiff
```

## Custom sorting with comparators

Advanced sorting techniques using custom comparison logic.  

```groovy
String[] names = ['Alice Johnson', 'Bob Smith', 'Charlie Brown', 'David Lee'] as String[]

// Sort by last name
names.sort { a, b -> 
    def lastA = a.split(' ')[1]
    def lastB = b.split(' ')[1]
    lastA <=> lastB
}
println names

// Sort by string length, then alphabetically
String[] words = ['cat', 'elephant', 'dog', 'butterfly', 'ant'] as String[]
words.sort { a, b ->
    def lengthComp = a.length() <=> b.length()
    lengthComp != 0 ? lengthComp : a <=> b
}
println words

// Using custom comparator with closure
Integer[] nums = [15, 3, 27, 8, 19, 1] as Integer[]
nums.sort { Math.abs(it - 10) }  // sort by distance from 10
println nums
```

## Advanced filtering techniques

Complex filtering operations beyond basic contains.  

```groovy
String[] cities = ['New York', 'Los Angeles', 'Chicago', 'Houston', 
                  'Philadelphia', 'Phoenix', 'San Antonio'] as String[]

// Filter by multiple criteria
def longCities = cities.findAll { it.length() > 8 && it.contains(' ') }
println longCities

// Filter with index information
def evenIndexedCities = cities.findIndexValues { idx, city -> 
    idx % 2 == 0 
}.collect { cities[it] }
println evenIndexedCities

// Complex pattern matching
def startsWithVowel = cities.grep { it =~ /^[AEIOU]/ }
println startsWithVowel

// Filtering with numerical conditions
int[] scores = [85, 92, 78, 96, 88, 71, 94] as int[]
def topScores = scores.findAll { it >= 90 }
println topScores
```

## Array transformation operations

Applying transformations to create new arrays from existing ones.  

```groovy
String[] words = ['hello', 'world', 'groovy', 'programming'] as String[]

// Transform to uppercase
def uppercase = words.collect { it.toUpperCase() }
println uppercase

// Transform with index
def withIndex = words.collectWithIndex { word, idx -> 
    "${idx}: ${word.capitalize()}" 
}
println withIndex

// Multiple transformations in chain
int[] numbers = [1, 2, 3, 4, 5] as int[]
def transformed = numbers.collect { it * 2 }
                        .findAll { it > 5 }
                        .collect { it - 1 }
println transformed

// Transform to different type
Double[] doubles = numbers.collect { it * 0.5 } as Double[]
println doubles
```

## Statistical operations

Computing various statistical measures on array data.  

```groovy
double[] values = [12.5, 18.3, 22.1, 15.7, 28.9, 19.4, 25.2] as double[]

// Basic statistics
println "Count: ${values.size()}"
println "Sum: ${values.sum()}"
println "Average: ${values.sum() / values.size()}"
println "Min: ${values.min()}"
println "Max: ${values.max()}"

// Standard deviation calculation
def mean = values.sum() / values.size()
def variance = values.collect { (it - mean) ** 2 }.sum() / values.size()
def stdDev = Math.sqrt(variance)
println "Standard deviation: ${stdDev}"

// Median calculation
def sorted = values.sort()
def median = sorted.size() % 2 == 0 ? 
    (sorted[sorted.size() / 2 - 1] + sorted[sorted.size() / 2]) / 2 :
    sorted[sorted.size() / 2]
println "Median: ${median}"
```

## Array copying and cloning

Creating copies of arrays for safe manipulation.  

```groovy
int[] original = [1, 2, 3, 4, 5] as int[]

// Shallow copy using clone
def shallowCopy = original.clone()
println "Original: ${original}"
println "Copy: ${shallowCopy}"

// Verify independence
shallowCopy[0] = 99
println "After modifying copy:"
println "Original: ${original}"
println "Copy: ${shallowCopy}"

// Deep copy for object arrays
String[] stringArray = ['apple', 'banana', 'cherry'] as String[]
def deepCopy = stringArray.collect { new String(it) } as String[]

// Using System.arraycopy for performance
int[] source = [10, 20, 30, 40, 50] as int[]
int[] destination = new int[5]
System.arraycopy(source, 0, destination, 0, source.length)
println destination
```

## Array validation operations

Validating array contents and structure.  

```groovy
int[] numbers = [2, 4, 6, 8, 10, 12] as int[]
String[] emails = ['user@domain.com', 'test@example.org', 'admin@site.net'] as String[]

// Check if all elements satisfy condition
def allEven = numbers.every { it % 2 == 0 }
println "All even: ${allEven}"

// Check if any elements satisfy condition  
def hasLargeNumber = numbers.any { it > 10 }
println "Has number > 10: ${hasLargeNumber}"

// Validate email format
def validEmails = emails.every { it =~ /^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$/ }
println "All valid emails: ${validEmails}"

// Check for null or empty arrays
def isEmpty = (numbers == null || numbers.length == 0)
println "Is empty: ${isEmpty}"

// Validate array bounds
def isValidIndex = { arr, idx -> idx >= 0 && idx < arr.length }
println "Index 3 valid: ${isValidIndex(numbers, 3)}"
println "Index 10 valid: ${isValidIndex(numbers, 10)}"
```

## Array partitioning and chunking

Dividing arrays into smaller segments for processing.  

```groovy
int[] bigArray = (1..20) as int[]

// Split into chunks of specific size
def chunks = bigArray.collate(5)
println chunks

// Split with overlap
def overlapping = bigArray.collate(4, 2)
println overlapping

// Partition based on condition
def partitioned = bigArray.split { it % 2 == 0 }
println "Odds: ${partitioned[0]}"
println "Evens: ${partitioned[1]}"

// Group consecutive elements
String[] sequence = ['a', 'a', 'b', 'b', 'b', 'c', 'a'] as String[]
def grouped = []
def current = []
sequence.each { item ->
    if (current.isEmpty() || current[-1] == item) {
        current << item
    } else {
        grouped << current.clone()
        current = [item]
    }
}
if (!current.isEmpty()) grouped << current
println grouped
```

## Array performance optimization

Techniques for efficient array operations and memory usage.  

```groovy
// Pre-allocate arrays when size is known
def createOptimizedArray = { size ->
    int[] arr = new int[size]  // More efficient than growing dynamically
    for (int i = 0; i < size; i++) {
        arr[i] = i * i
    }
    return arr
}

// Use primitive arrays for better performance
int[] primitiveInts = (1..1000).collect { it } as int[]  // More memory efficient
Integer[] objectInts = (1..1000).collect { it } as Integer[]  // Uses more memory

// Batch operations instead of individual element processing
int[] largeArray = (1..10000) as int[]

// Efficient: single pass transformation
def squared = largeArray.collect { it * it }

// Less efficient: multiple passes
def processed = largeArray.findAll { it > 50 }
                          .collect { it * 2 }
                          .findAll { it < 1000 }

println "Optimized array size: ${createOptimizedArray(10).length}"
println "Primitive array memory footprint is smaller than object array"
```

## Advanced array search operations

Sophisticated searching techniques beyond basic find methods.  

```groovy
String[] dictionary = ['apple', 'banana', 'cherry', 'date', 'elderberry', 
                      'fig', 'grape', 'honeydew'] as String[]

// Binary search (requires sorted array)
def sortedArray = dictionary.sort()
def searchTerm = 'fig'
def index = Collections.binarySearch(sortedArray as List, searchTerm)
println "Binary search result: ${index}"

// Find multiple matching indices
def findAllIndices = { arr, predicate ->
    def indices = []
    arr.eachWithIndex { item, idx ->
        if (predicate(item)) indices << idx
    }
    return indices
}

def vowelIndices = findAllIndices(dictionary) { it[0] in ['a', 'e', 'i', 'o', 'u'] }
println "Indices of words starting with vowels: ${vowelIndices}"

// Pattern-based searching
def fuzzySearch = { arr, pattern ->
    arr.findAll { it.toLowerCase().contains(pattern.toLowerCase()) }
}

def berryWords = fuzzySearch(dictionary, 'berry')
println "Words containing 'berry': ${berryWords}"

// Searching with context (returning matches with surrounding elements)
def contextualFind = { arr, predicate, context ->
    def results = []
    arr.eachWithIndex { item, idx ->
        if (predicate(item)) {
            def start = Math.max(0, idx - context)
            def end = Math.min(arr.length - 1, idx + context)
            results << arr[start..end]
        }
    }
    return results
}

int[] numbers = (1..10) as int[]
def contextualized = contextualFind(numbers, { it == 5 }, 2)
println "Number 5 with context: ${contextualized}"
```

## Array aggregation operations

Computing aggregate values and summaries from array data.  

```groovy
int[] sales = [1200, 850, 1450, 900, 1100, 1350, 750] as int[]

// Calculate running totals
def runningTotals = []
sales.eachWithIndex { value, idx ->
    def total = sales[0..idx].sum()
    runningTotals << total
}
println "Running totals: ${runningTotals}"

// Group by criteria and aggregate
String[] products = ['laptop', 'mouse', 'laptop', 'keyboard', 'mouse', 'laptop'] as String[]
def productCounts = products.countBy { it }
println "Product counts: ${productCounts}"

// Calculate percentiles
double[] scores = [78, 85, 92, 88, 76, 94, 82, 90, 87, 79] as double[]
def sortedScores = scores.sort()
def percentile90 = sortedScores[(int)(sortedScores.length * 0.9)]
println "90th percentile: ${percentile90}"

// Moving average calculation
def movingAverage = { arr, window ->
    def averages = []
    for (int i = 0; i <= arr.length - window; i++) {
        def sum = arr[i..(i + window - 1)].sum()
        averages << sum / window
    }
    return averages
}

def movingAvg = movingAverage(sales, 3)
println "3-period moving average: ${movingAvg}"
```

## Array memory and performance patterns

Best practices for efficient array usage and memory management.  

```groovy
// Memory-efficient array creation patterns
def createLargeArray = { size ->
    // Pre-allocate with known size to avoid resizing overhead
    int[] arr = new int[size]
    
    // Batch initialization is more efficient
    for (int i = 0; i < size; i++) {
        arr[i] = i * 2
    }
    
    return arr
}

// Demonstrate memory efficiency of primitive vs object arrays
println "Creating arrays for performance comparison..."

// Primitive arrays use less memory
int[] primitiveArray = (1..1000).collect { it } as int[]

// Object arrays use more memory due to boxing
Integer[] objectArray = (1..1000).collect { new Integer(it) } as Integer[]

// Reuse arrays when possible instead of creating new ones
def reuseExample = { int[] workArray, List<Integer> data ->
    // Clear existing data
    Arrays.fill(workArray, 0)
    
    // Populate with new data (up to array size)
    int limit = Math.min(workArray.length, data.size())
    for (int i = 0; i < limit; i++) {
        workArray[i] = data[i]
    }
    
    return workArray
}

// Example of array reuse
int[] workingArray = new int[100]
def testData = [5, 10, 15, 20, 25]
def reused = reuseExample(workingArray, testData)
println "Reused array first 5 elements: ${reused[0..4]}"

// Lazy evaluation for memory efficiency
def lazyArrayProcessing = { int[] source ->
    // Process in chunks to manage memory
    def chunkSize = 10
    def results = []
    
    for (int i = 0; i < source.length; i += chunkSize) {
        def end = Math.min(i + chunkSize, source.length)
        def chunk = source[i..<end]
        def processed = chunk.collect { it * it }
        results.addAll(processed)
    }
    
    return results
}

def sourceArray = (1..50) as int[]
def processedResults = lazyArrayProcessing(sourceArray)
println "Processed ${sourceArray.length} elements in chunks"
```

## Array pattern matching and templates

Using arrays as templates and patterns for data processing.  

```groovy
// Pattern matching with arrays
def matchesPattern = { arr, pattern ->
    if (arr.length != pattern.length) return false
    
    return arr.indices.every { i ->
        pattern[i] == null || arr[i] == pattern[i]
    }
}

int[] data = [1, 2, 3, 4, 5] as int[]
def pattern1 = [1, null, 3, null, 5]  // matches positions 0, 2, 4
def pattern2 = [1, 2, null, 4, null]  // matches positions 0, 1, 3

println "Data matches pattern1: ${matchesPattern(data, pattern1)}"
println "Data matches pattern2: ${matchesPattern(data, pattern2)}"

// Template-based array generation
def generateFromTemplate = { template, substitutions ->
    return template.collect { item ->
        substitutions.containsKey(item) ? substitutions[item] : item
    }
}

String[] template = ['Hello', '%NAME%', 'welcome', 'to', '%PLACE%'] as String[]
def subs = ['%NAME%': 'Alice', '%PLACE%': 'Wonderland']
def generated = generateFromTemplate(template, subs)
println "Generated from template: ${generated.join(' ')}"

// Array-based state machines
def processStateMachine = { states, transitions, input ->
    def currentState = 0
    def result = []
    
    input.each { symbol ->
        if (transitions[currentState]?.containsKey(symbol)) {
            currentState = transitions[currentState][symbol]
            result << states[currentState]
        } else {
            result << 'ERROR'
        }
    }
    
    return result
}

String[] states = ['START', 'A_STATE', 'B_STATE', 'END'] as String[]
def transitions = [
    0: ['a': 1, 'b': 2],    // START -> A_STATE on 'a', B_STATE on 'b'
    1: ['b': 3],            // A_STATE -> END on 'b'  
    2: ['a': 3],            // B_STATE -> END on 'a'
    3: [:]                  // END state (no transitions)
]

def inputSequence = ['a', 'b']
def machineResult = processStateMachine(states, transitions, inputSequence)
println "State machine result: ${machineResult}"
```
