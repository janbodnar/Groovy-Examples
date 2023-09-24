# Records

Groovy 4 introduced records.    
Groovy already had similar annotations: `@Immutable` and  `@Canonical`.  



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

for (e in res) {
    
    println e
}
```

