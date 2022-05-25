# Lists

## Size/max/min

```groovy
def vals = [-1, 0, 1, 2, 3, 4, 5]

println vals.min()
println vals.max()
println vals.size()

def words = ['sky', 'at', 'storm', 'falcon', 'universe']
println words.min { it.size() }
println words.max { it.size() }
```

## Avg/count/sum

```groovy
def vals = [-2, -1, 0, 1, 2, 3, 4]

println vals.average()
println vals.count{ it > 0}
println vals.sum()
println vals.grep(it -> it < 0).sum()
```

## Clear/empty

```groovy
def vals = [-2, -1, 0, 1, 2, 3, 4]

if (vals.empty) {
    println "list is empty"
} else {
    println "list is not empty"
}

vals.clear()

if (vals.isEmpty()) {
    println "list is empty"
} else {
    println "list is not empty"
}
```

## Type

```groovy
def vals = [2, 3, 4, 5]

println vals.getClass()
println vals instanceof List
```

## First/Last, Head/Tail/Init

```groovy
def vals = [1, 2, 3, 4, 5]

println vals.first()
println vals.head()

println vals.last()

println vals.tail()
println vals.init()
```

## Index/get

```groovy
def vals = [-2, -1, 0, 1, 2, 3, 4, 5]

println vals[0]
println vals[1]
println vals[-1]
println vals[-2]

println '-------------------'

println vals[0..3]
println vals[3..-1]
println vals[0, 2, 5]
println vals[-2..-5]

println '-------------------'

println vals.get(0)
println vals.get(1)
println vals.getAt(-1)
println vals.getAt(-2)
```

## Add/remove

```groovy
def vals = [2, 3, 4, 5]

vals.add(6)
vals << 7
vals << 8 << 9 << 10

vals.add(0, 1)
vals.add(0, 0)
vals.add(0, -1)

println vals
println "-------------------"

vals.remove(vals.size()-1)
vals.remove(0)

println vals
```

## Chopping 

```groovy
def vals = [-2, -1, 0, 0, 1, 1, 2, 3, 4, 4, 4, 5, 6, 4]

println vals.chop(5)
println vals.chop(5, 6)
println vals.chop(2, 3, -1)
```

## Modify

```groovy

def vals = [3, 4, 5, 6, 7]

println vals

vals.add(8)
vals << 9 << 10
vals.addAll([11, 12])

println '--------------------------'

vals.pop()
vals.push(0)

println vals

println '--------------------------'

vals[1] = 2

println vals

println '--------------------------'

vals.removeAt(3)
vals.swap(0, vals.size()-1)

println vals
```

## Plus/minus

```groovy
def vals = [3, 4]
def res = vals.plus(5).plus([6, 7]).plus(8..11)

println vals
println res

def res2 = res.minus(4).minus(5).minus([6, 7]).minus(8..10)

println res2
```

## Loop

```groovy
def words = ['cup', 'crisp', 'cloud', 'break', 
    'falcon', 'war', 'oil']

for (def word in words) {
    println word
}

println "----------------------"

words.each { word -> println word }

println "----------------------"

words.reverseEach { word -> println word }
```

## Sorting

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

words.sort { a, b -> b <=> a }
println words
```

## Reversing

```groovy
def vals = [1, 2, 3, 4, 5, 6]

println vals.reverse()
println vals

println '-----------------------'

vals.reverse(true)
println vals

println '-----------------------'

Collections.reverse(vals)
println vals
```

## Grep 

```groovy
def vals = [-2, -1, 0, 1, 2, 3, 4, 5]

def r1 = vals.grep { it > 0 }
println r1

def some = [1, true, -4, "falcon", 3.4]

def r2 = some.grep(Number)
println r2
```

## Unique values

```groovy

def vals = [2, 2, -1, -2, 0, 1, 1, 2, -3, 11, 3, 4]

def uniq = vals.unique(false) // creates a copy
println uniq
println vals 

println '-----------------------'

vals.unique(true) // removes duplicates in-place
println vals 
```

## Counting

```groovy
def vals = [-2, -1, 0, 0, 1, 1, 2, 3, 4, 4, 4, 5, 6, 4]

println vals.count(0)
println vals.count(4)
println vals.count(6)
println vals.count { it > 0 }
println vals.countBy { it < 0}
```

## Shuffle 

```groovy
def vals = [-2, -1, 0, 1, 2, 3, 4, 5, 6, 7]

def shuffled = vals.shuffled()
println shuffled
println vals

println '---------------------------'

vals.shuffle()
println vals
```

## Flatten

```groovy
def vals = [1, 2, 3, 4, 5, [6, 7, [8, 9, [10]]]]

println vals
println vals[5]
println vals[5][2]
println vals[5][2][2]
println vals[5][2][2][0]

println vals.flatten()
```

## Execute

```groovy
def cmds = ['ls', '-l'].execute()
cmds.waitFor()

println cmds.text
```
