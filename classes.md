# Groovy Classes

- classes or methods with no visibility modifier are public
- fields with no visibility modifier are turned into properties
- if no constructors are supplied, an empty no-arg constructor is provided  

```groovy
import groovy.transform.Canonical

@Canonical
class User {

    String name
    String occupation
}

def u = new User('John Doe', 'gardener')
println u

println u.getName()
println u.getOccupation()

def u2 = new User('occupation': 'driver', 'name': 'Roger Roe')

println u2.name
println u2.occupation
```

We have getters/setters and attributes automatically created.  
The `@Canonical` annotation adds constructors, `toString` method and equals/hashcode.  


## Object instance creation

There are several ways to instantiate an object  

```groovy
class User {

    String name
    String occupation

    User(String name, String occupation) {
        this.name = name
        this.occupation = occupation
    }

    String toString() {
        "${this.name} is a ${this.occupation}"
    }
}

def u = new User('John Doe', 'gardener')
println u

def u2 = ['Roger Roe', 'driver'] as User
println u2.name
println u2.occupation

User u3 = ['Paul Smith', 'teacher']
println u3
```

## Object creation with tap 

```groovy
class User {

    String name
    String occupation

    User() {}

    String toString() {
        "${this.name} is a ${this.occupation}"
    }
}

def u = new User().tap {
    name = 'John Doe'
    occupation = 'gardener'
}

println u
```

## @TupleConstructor annotation

The `@TupleConstructor` creates a classic constructor  

```groovy
import groovy.transform.TupleConstructor

@TupleConstructor
class User {

    String name
    String occupation
    List<String> favcols

    String toString() {
        "${this.name} is a ${this.occupation}, favourite colours ${favcols}"
    }
}


def u = new User('John Doe', 'gardener', ['red', 'green', 'blue'])
println u
```

## @MapConstructor annotation

The `@MapConstructor` creates a map-style constructor  

```groovy
import groovy.transform.MapConstructor

@MapConstructor
class User {

    String name
    String occupation

    String toString() {
        "${this.name} is a ${this.occupation}"
    }
}

def u = new User(name: 'John Doe', occupation: 'gardener')
println u
```

## findAll

```groovy
import groovy.transform.Immutable

@Immutable
class Task {
    String title
    boolean done
}

def tasks = [ 
    new Task("Task 1", true), new Task("Task 2", true), 
    new Task("Task 3", false), new Task("Task 4", true), 
    new Task("Task 5", false) 
]

def res = tasks.findAll { it.done == true }
println res
```

