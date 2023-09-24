# Arrays 

## Arrays

```groovy
def vals = [1, 2, 3, 4, 5] as int[]
println vals
println vals.length
println vals.size()

def words = new String[] { 'sky', 'cloud', 'pen' }
println words

String[] words2 = ['borrow', 'water', 'globe']
println words[0]
println words[-1]
println words[0..2]
```

---

Defining an array through range.  
Element access via index `[]` operator or via `getAt` method.  

```groovy
int[] nums = -3..2
println nums

println '------------------------'

println nums[0]
println nums[1]
println nums.getAt(0)
println nums.getAt(1)

println '------------------------'

def res = nums.findAll(e -> e % 2 == 0)
println res
```
