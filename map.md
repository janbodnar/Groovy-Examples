# Maps 

## Get elements

```groovy
def cts = [ sk: "Slovakia", ru: "Russia", de: "Germany", no: "Norway" ]

println cts['sk']
println cts.get('sk')

println cts.size()
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
