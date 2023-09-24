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

## Element access

Element access via index `[]` operator or via `getAt` method.  

```groovy
int[] nums = -3..2

println nums[0]
println nums[1]
println nums.getAt(0)
println nums.getAt(1)

println '------------------------'

String[] words2 = ['borrow', 'water', 'globe']
println words[0]
println words[-1]
println words[0..2]
```

## Array iteration

```groovy
def nums = [1, 2, 3, 4, 5] as int[]

for (def e in nums) {
    println e
}

def words = ['sky', 'blue', 'war', 'water', 'coffee'] as String[]

words.each {e -> println e}
words.eachWithIndex { e, i -> println "${i}: ${e}" }
```

## Finding elements

```groovy
def nums = -4..10

def res = nums.findAll(e -> e % 2 == 0)
println res

res = nums.find { it > 0 } // get fst element giving true
println res
```


