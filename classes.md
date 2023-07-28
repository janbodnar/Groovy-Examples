# Groovy Classes

- classes or methods with no visibility modifier public
- fields with no visibility modifier are turned into properties


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
The `@Canonical` annotation adds constructors, `ToString` method and equals/hashcode.  
