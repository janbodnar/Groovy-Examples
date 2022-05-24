# Lists

```groovy
vals = [1, 2, 3, 4, 5]
println vals

println "The size is: ${vals.size()}"
println vals[0]
println vals[1]
println vals[-1]
println vals[-2]
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
