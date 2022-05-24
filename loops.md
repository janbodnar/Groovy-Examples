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

for (val in vals) {

    println val
}
```
