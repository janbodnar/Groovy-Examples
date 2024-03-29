# Records

Groovy 4 introduced records.    
Groovy already had similar annotations: `@Immutable` and  `@Canonical`.  

## Simple

```groovy
record User(String fname, String lname, String occupation) { }

def u = new User('John', 'Doe', 'gardener')
println u 

println u.fname
println u.lname
println u.occupation
```

## Named-arg constructor

```groovy
record User(String fname, String lname, String occupation) { }

def u = new User(lname:'Roe', fname:'Roger', occupation:'driver')
println u
```

## Destructure

```groovy
record User(String fname, String lname, String occupation) { }

def u = new User('John', 'Doe', 'gardener')
def (fname, lname, occupation) = u

println "${fname} ${lname} is a ${occupation}"
```

## Sortable with records

```groovy
import groovy.transform.Sortable

@Sortable(includes='lname')
record User(String fname, String lname, String occupation) {}

def users = [
    new User('John', 'Doe', 'gardener'),
    new User('Roger', 'Roe', 'driver'),
    new User('Lucia', 'Smith', 'accountant'),
    new User('Paul', 'Newman', 'firefighter'),
    new User('Adam', 'Clapton', 'teacher'),
    new User('Jane', 'Walter', 'pilot')
]

for (def user in users) {
    println user
}

println '----------------------'

users.sort()

for (def user in users) {
    println user
}
```

## Grouping 

```groovy
import java.time.LocalDate

record User(String name, String occupation, LocalDate dob) { }

def users = [

    new User('John Doe', 'gardener', LocalDate.parse('1973-09-07', 'yyyy-MM-dd')),
    new User('Roger Roe', 'driver', LocalDate.parse('1963-03-30', 'yyyy-MM-dd')),
    new User('Kim Smith', 'teacher', LocalDate.parse('1980-05-12', 'yyyy-MM-dd')),
    new User('Joe Nigel', 'artist', LocalDate.parse('1983-03-30', 'yyyy-MM-dd')),
    new User('Liam Strong', 'teacher', LocalDate.parse('2009-03-06', 'yyyy-MM-dd')),
    new User('Robert Young', 'gardener', LocalDate.parse('1978-11-16', 'yyyy-MM-dd')),
    new User('Liam Strong', 'teacher', LocalDate.parse('1986-10-23', 'yyyy-MM-dd'))
]
 
def res = users.groupBy({ it.occupation })

for (def e in res) {
    
    println e
}
```

Split users into millenials and others. We use the static `of` builder method for the record.  

```groovy
import java.time.LocalDate

record User(String name, String occupation, LocalDate dob) { 
    static User of(String name, String occupation, LocalDate dob) {
        return new User(name, occupation, dob)
    }
}

def users = [

    User.of('John Doe', 'gardener', LocalDate.parse('1973-09-07', 'yyyy-MM-dd')),
    User.of('Roger Roe', 'driver', LocalDate.parse('1963-03-30', 'yyyy-MM-dd')),
    User.of('Kim Smith', 'teacher', LocalDate.parse('1980-05-12', 'yyyy-MM-dd')),
    User.of('Joe Nigel', 'artist', LocalDate.parse('1983-03-30', 'yyyy-MM-dd')),
    User.of('Liam Strong', 'teacher', LocalDate.parse('2009-03-06', 'yyyy-MM-dd')),
    User.of('Robert Young', 'gardener', LocalDate.parse('1978-11-16', 'yyyy-MM-dd')),
    User.of('Liam Strong', 'teacher', LocalDate.parse('1986-10-23', 'yyyy-MM-dd'))
]

def millen = LocalDate.parse('2000-01-01')
def res = users.groupBy({ it.dob > millen })

println 'millenials'

for (def e in res[true]) {
    println e
}

println 'others'

for (def e in res[false]) {
    println e
}
```


