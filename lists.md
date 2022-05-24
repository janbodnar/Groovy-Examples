# Lists

## Type

```groovy
def vals = [2, 3, 4, 5]

println vals.getClass()
println vals instanceof List
```

## Size/index

```groovy
def vals = [1, 2, 3, 4, 5]
println vals

println "The size is: ${vals.size()}"
println vals[0]
println vals[1]
println vals[-1]
println vals[-2]
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

## Loop

```groovy

def words = ['cup', 'crisp', 'cloud', 'break', 
    'falcon', 'war', 'oil']

for (def word in words) {
    println word
}

println "----------------------"

words.each { word -> println word }
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
