# Loops

## Classic for loop

```groovy
def vals = [1, 2, 3, 4, 5, 6]

for (int i=0; i<vals.size(); i++) {

    println vals[i]
}
```

## while loop

```groovy
def vals = [1, 2, 3, 4, 5, 6]

int i = 0
int n = vals.size()

while (i < n) {
    println vals[i]
    i++
}
```

## for/in loop

```groovy
def vals = [1, 2, 3, 4, 5, 6]

for (def val in vals) {

    println val
}
```

## each* methods

```groovy
def vals = [11, 22, 33, 44, 55, 66]

vals.each {v -> println v}
vals.eachWithIndex{idx, v -> println "${v} -> ${idx}"}
```
