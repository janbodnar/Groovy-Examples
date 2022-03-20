# Sorting

## Alphanumeric sorting

```groovy
def nums = [ 7, 9, 3, -2, 8, 1, 0 ]
def words = [ "sky", "cloud", "atom", "brown", "den", "kite", "town" ]

nums.sort()
println nums

nums.sort { -it }
println nums

nums.sort { a, b -> a <=> b }
println nums

println "--------------------------------------------"

words.sort()
println words
Collections.reverse(words)
println words
```

## Sort words by length

```groovy
def words = [ "sky", "Sun", "Albert", "cloud", "by", "Earth", "else",
      "atom", "brown", "a", "den", "kite", "town" ]

words.sort { e -> e.length() }
println words
```

## Sort by surnames

```groovy
def names = [ "John Doe", "Lucy Smith", "Benjamin Young", "Robert Brown",
      "Thomas Moore", "Linda Black", "Adam Smith", "Jane Smith" ]

names.sort { it.split('\s{1}')[1] }
println names

names.sort { name ->

    def ns = name.split('\s{1}')
    def a = ns[0]
    def b = ns[1]

    return a <=> b 
}

println names
```

## Sort string ci

```groovy
def words = [ "sky", "Sun", "Albert", "cloud", "by", "Earth", "else",
      "atom", "brown", "a", "den", "kite", "town" ]

words.sort { a -> a.toLowerCase() }
println words

words.sort()
println words
```

## Custom comparator

```groovy
Comparator mc = { a, b -> 
    def n1 = a.length()
    def n2 = b.length()

    if (n1 == n2) return a <=> b 
    else return n1 <=> n2
}

def words = [ "sky", "eye", "Sun", "Albert", "cloud", "by", "Earth", "else",
      "of", "atom", "brown", "lie", "a", "be", "den", "kite", "to", "town" ]

words.sort(mc)
println words
```
