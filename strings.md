# Strings 

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

## Formatting

```groovy
int age = 34
String name = 'John Doe'

String msg = sprintf "%s is %d years old", name, age
println msg
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
