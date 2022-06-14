# Strings 

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
