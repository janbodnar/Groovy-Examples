# Cycles

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

