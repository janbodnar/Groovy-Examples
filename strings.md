# Strings 


## string indexing

```groovy
String msg = 'an old falcon'

println msg[0]
println msg[1]
println msg[-1]
println msg[-2]

println msg[1..4]
println msg[3..6]

println msg[-1..0]
```

## int to String

```groovy
int n = 4
String msg = "There are " + n + " hawks"
println msg

int n2 = 4
String msg2 = "There are $n2 hawks"
println msg2

int n3 = 4

def builder = new StringBuilder("There are ")
builder.append(n3)
builder.append(" hawks")

println builder
```

## String to int

```groovy
def data = ['1', '2', '3', '4', '5']
def sum = 0 

data.each {

    sum += it as int
}

println sum

def sum2 = data.sum {
    it as int
}

println sum2
```

## Concatenation

```groovy
def w1 = 'an'
def w2 = 'old'
def w3 = 'falcon'

def s1 = w1 + ' ' + w2 + ' ' + w3
println s1

def s2 = w1 << ' ' << w2 << ' ' << w3
println s2

def s3 = w1.concat(' ').concat(w2).concat(' ').concat(w3)
println s3
```

## Size

```groovy
import java.text.BreakIterator

def s1 = 'falcon'
println s1.length()

def s2 = 'čerešňa'
println s2.length()

def s3 = '🐜🐬🐄🐘🦂🐫🐑🦍🐯🐞'

def it = BreakIterator.getCharacterInstance()
it.setText(s3)

def start = it.first()
def end = it.next()
int n = 0

while (start < end) { 

    n++
    start = end 
    end = it.next()
}

println n
```

## Interpolation

```groovy
def name = 'John Doe'
def occupation = 'gardener'

def msg = "$name is a $occupation"
println msg

def x = 11
def y = 12 

println "$x + $y = ${x + y}"
```

---

```groovy
def user = [name: 'John Doe', age: 34]
println "$user.name is $user.age years old"
```

## Formatting

```groovy
int age = 34
String name = 'John Doe'

String msg = sprintf "%s is %d years old", name, age
println msg
```

## Multiply

```groovy
println 'An old falcon'
println '-' * 15

println 'a foggy mountain'
println '-'.multiply(15)

println 'a sunny day'
```

## Iterate over lines 

```groovy
def text = """\
an old falcon
blue sky
rainy day
new computer
precious stone
"""

text.eachLine { line -> 
    println line
}
```
