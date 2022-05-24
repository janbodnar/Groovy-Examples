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
--

```groovy
record User(String name, String occupation) {}

def users = [
    new User("John Doe", "gardener"), 
    new User("Roger Roe", "driver"),
    new User("Lucy Smith", "teacher")
]

for (def user in users) {

    println user
}
```

## each* methods

```groovy
def vals = [11, 22, 33, 44, 55, 66]

vals.each {v -> println v}
vals.eachWithIndex{idx, v -> println "${v} -> ${idx}"}
```

## break statement

```groovy
def rnd = new Random()

while (true) {

    int num = rnd.nextInt(30)
    print "$num "

    if (num == 22) {

        break
    }
}

println ""
```

## continue statement

```groovy
int num = 0

while (num < 100) {

    num++

    if ((num % 2) == 0) {
        continue
    }

    print "$num "
}

print '\n'
```
