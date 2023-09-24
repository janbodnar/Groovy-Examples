# Arrays 

Arrays are collections of fixed size.  

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

## Mapping

The map operation is done with `collect`.  

```groovy
int[] nums = -4..10

def res = nums.collect { it * it } 
println res
```

## Finding elements

```groovy
int[] nums = -4..10

def res = nums.findAll(e -> e % 2 == 0)
println res

res = nums.find { it > 0 } // get fst element giving true
println res
```

## The any/every methods 

```groovy
int[] nums = -4..10

def res = nums.any { it > 0 } // are there any positive values?
println res

res = nums.every { it > 0 } // is every element positive?
println res
```

