# Cycles


## for cycle

```groovy
def vals = [1, 2, 3, 4]

for (def e in vals) {
    println e
}

for (int i=0; i<vals.size(); i++) {
    println vals[i]
}
```

Iterate over a string and range  

```groovy
def msg = 'an old falcon'

for (def e in msg) {
    println e
}

println '-------------------'

for (def e in 1..5) {
    println e
}
```

Iterate over a map  

```groovy
def user = [name: 'John Doe', occupation: 'gardener']

for (def u in user) {

    println "${u.key}, ${u.value}"
}
```

## iterate with index

```groovy
def words = ['key', 'cup', 'cloud', 'storm', 'wood']

words.eachWithIndex { e, i ->
    println "${i}: ${e}"
}
```

## while cycle

```groovy

def words = ['key', 'cup', 'cloud', 'storm', 'wood']

int n = words.size()

while (n > 0) {
    
    n--
    println words[n]
} 


def vals = [2, 1, -2, 0, 8, 3, 5, 6]

int sum = 0
int m = vals.size()

while (m > 0) {

    m--
    sum += vals[m] 
}

println sum
```


