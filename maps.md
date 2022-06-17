# Maps 

## Basics

```groovy
def cts = [ sk: 'Slovakia', ru: 'Russia', de: 'Germany', no: 'Norway' ]

println cts['sk']
println cts.get('sk')
println cts.size()

cts.put('hu', 'Hungary')
println cts.containsKey('hu')
```

## Traversing 

```groovy
def capitals = [ Bratislava: 424207, Vilnius: 556723, Lisbon: 564657,
    Riga: 713016, Jerusalem: 780200, Warsaw: 1711324,
    Budapest: 1729040, Prague: 1241664, Helsinki: 596661,
    Tokyo: 13189000, Madrid: 3233527 ]

capitals.each { e -> 
    println "$e.key $e.value"
}

println "-----------------------------"

capitals.each { k, v -> 
    println "$k $v"
}

println "-----------------------------"

capitals.eachWithIndex { k, v, i -> 
    println "$i $k $v"
}
```

## Sorting

```groovy
def capitals = [ Bratislava: 424207, Vilnius: 556723, Lisbon: 564657,
    Riga: 713016, Jerusalem: 780200, Warsaw: 1711324,
    Budapest: 1729040, Prague: 1241664, Helsinki: 596661,
    Tokyo: 13189000, Madrid: 3233527 ]
    
println capitals.sort { it.key }
println capitals.sort { it.value }
println capitals.sort { a, b -> b.value <=> a.value }
```

## Finding 

```groovy
def users = [
   [fname: "Robert", lname: "Novak", salary: 1770],
   [fname: "John", lname:"Doe", salary: 1230],
   [fname: "Lucy", lname:"Novak", salary: 670],
   [fname: "Ben", lname:"Walter", salary: 2050],
   [fname: "Robin",lname: "Brown", salary: 2300],
   [fname: "Amy",lname: "Doe", salary: 1250],
   [fname: "Joe", lname:"Draker", salary: 1190],
   [fname: "Janet", lname:"Doe", salary: 980],
   [fname: "Peter",lname: "Novak", salary: 990],
   [fname:"Albert", lname:"Novak",salary: 193]
]

println users.find { u -> u.salary < 1000 }
println users.findAll { u -> u.salary < 1000 }
```

## Counting 

```groovy
def users = [
   [fname: "Robert", lname: "Novak", salary: 1770],
   [fname: "John", lname:"Doe", salary: 1230],
   [fname: "Lucy", lname:"Novak", salary: 670],
   [fname: "Ben", lname:"Walter", salary: 2050],
   [fname: "Robin",lname: "Brown", salary: 2300],
   [fname: "Amy",lname: "Doe", salary: 1250],
   [fname: "Joe", lname:"Draker", salary: 1190],
   [fname: "Janet", lname:"Doe", salary: 980],
   [fname: "Peter",lname: "Novak", salary: 990],
   [fname:"Albert", lname:"Novak",salary: 193]
]

println users.count { u -> u.lname == 'Novak' || u.lname == 'Doe' }
println users.countBy { u -> u.salary < 1000 }
```

## collect & groupBy

```groovy
def users = [
   [fname: "Robert", lname: "Novak", salary: 1770],
   [fname: "John", lname:"Doe", salary: 1230],
   [fname: "Lucy", lname:"Novak", salary: 670],
   [fname: "Ben", lname:"Walter", salary: 2050],
   [fname: "Robin",lname: "Brown", salary: 2300],
   [fname: "Amy",lname: "Doe", salary: 1250],
   [fname: "Joe", lname:"Draker", salary: 1190],
   [fname: "Janet", lname:"Doe", salary: 980],
   [fname: "Peter",lname: "Novak", salary: 990],
   [fname:"Albert", lname:"Novak",salary: 193]
]

println users.groupBy { it.lname }

println '--------------------------'

println users.collect { [it.fname, it.lname, it.salary * 1.1] }

println '--------------------------'

println users.collect { ["$it.fname $it.lname", it.salary * 1.1] }
```
