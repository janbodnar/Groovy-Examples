# Records


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
