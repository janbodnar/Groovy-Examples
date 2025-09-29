# Strings 

Strings are fundamental data types in Groovy that represent sequences of  
characters. Groovy provides extensive built-in support for string operations,  
making it easy to work with text data. In Groovy, strings are objects that  
can be created using single quotes (''), double quotes (""), or triple quotes  
(''' or """) for multi-line strings.  

Groovy strings are based on Java's String class but with additional  
functionality and syntactic sugar. Single-quoted strings are regular Java  
String objects, while double-quoted strings create GString objects that  
support variable interpolation. This allows for dynamic string construction  
with embedded expressions.  

The language provides powerful string manipulation capabilities including  
indexing, slicing, concatenation, formatting, regular expression support,  
and various transformation methods. Strings in Groovy are immutable objects,  
meaning that operations that appear to modify a string actually create new  
string instances.  

Groovy also supports advanced string features like multi-line strings with  
here-document syntax, string multiplication for repetition, and seamless  
integration with regular expressions. The language's dynamic nature makes  
string operations more flexible and expressive compared to traditional  
Java string handling.  


## String literals and creation

Basic string creation using different quotation styles and string literal  
syntax.  

```groovy
def str1 = 'single quotes'
def str2 = "double quotes"
def str3 = """triple quotes"""

println str1
println str2
println str3

def empty = ''
def unicode = 'café'
def escaped = 'line1\nline2\ttab'

println "Empty: '$empty'"
println "Unicode: $unicode"
println "Escaped: $escaped"
```

This example demonstrates different ways to create strings in Groovy.  
Single quotes create regular strings, double quotes create GStrings that  
support interpolation, and triple quotes allow multi-line strings.  

## String indexing

Access individual characters and substrings using index notation and  
range operations.  

```groovy
String msg = 'an old falcon'

println msg[0]
println msg[1]
println msg[-1]
println msg[-2]

println msg[1..4]
println msg[3..6]

println msg[-1..0]
```

String indexing in Groovy supports both positive and negative indices.  
Positive indices start from 0, negative indices count from the end.  
Range operations allow extracting substrings efficiently.  

## String comparison and equality

Compare strings using various equality and comparison operators.  

```groovy
def str1 = 'falcon'
def str2 = 'falcon'
def str3 = 'hawk'
def str4 = 'Falcon'

println str1 == str2
println str1.equals(str2)
println str1.is(str2)

println str1 == str3
println str1 < str3
println str1 > str3

println str1.equalsIgnoreCase(str4)
println str1.compareTo(str3)
println str1.compareToIgnoreCase(str4)
```

Groovy provides multiple ways to compare strings including case-sensitive  
and case-insensitive comparisons, identity checks, and lexicographic  
ordering operations.  

## Int to String

```groovy
int n = 4
String msg = "There are " + n + " hawks"
println msg

int n2 = 4
String msg2 = "There are $n2 hawks"
println msg2

int n3 = 4

def builder = new StringBuilder("There are ")
builder.append(n3)
builder.append(" hawks")

println builder
```

Converting integers to strings can be done through concatenation,  
interpolation, or StringBuilder for performance-critical applications.  
String interpolation with double quotes provides the most readable syntax.  

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

Converting strings to integers using the 'as' operator or explicit parsing  
methods. Collection operations like sum can be combined with conversion  
for efficient processing of string number collections.  

## Case operations

Transform string case using built-in case conversion methods.  

```groovy
def msg = 'An Old Falcon'

println msg.toLowerCase()
println msg.toUpperCase()
println msg.capitalize()

def words = 'hello there'
println words.split(' ').collect { it.capitalize() }.join(' ')

def mixedCase = 'tHiS iS mIxEd'
println mixedCase.toLowerCase()

def acronym = 'IBM'
println acronym.toLowerCase().capitalize()
```

Groovy provides convenient methods for case transformations including  
toLowerCase(), toUpperCase(), and capitalize(). These methods can be  
combined with collection operations for more complex transformations.  

## Concatenation

```groovy
def w1 = 'an'
def w2 = 'old'
def w3 = 'falcon'

def s1 = w1 + ' ' + w2 + ' ' + w3
println s1

def s2 = w1 << ' ' << w2 << ' ' << w3
println s2

def s3 = w1.concat(' ').concat(w2).concat(' ').concat(w3)
println s3
```

Multiple approaches to string concatenation in Groovy: plus operator (+),  
left shift operator (<<), and the concat() method. Each has different  
performance characteristics and use cases.  

## Trimming and padding

Remove whitespace and add padding to strings for formatting purposes.  

```groovy
def text = '   hello there   '

println "'${text}'"
println "'${text.trim()}'"
println "'${text.stripLeading()}'"
println "'${text.stripTrailing()}'"

def word = 'falcon'
println word.padLeft(10)
println word.padLeft(10, '*')
println word.padRight(10, '-')
println word.center(12, '=')

def multiSpace = 'hello    world'
println multiSpace.replaceAll(/\s+/, ' ')
```

String trimming removes whitespace from beginnings and ends of strings.  
Padding operations add characters to reach desired string lengths, useful  
for formatting output in tables or aligned displays.  

## Size

```groovy
import java.text.BreakIterator

def s1 = 'falcon'
println s1.length()

def s2 = 'čerešňa'
println s2.length()

def s3 = '🐜🐬🐄🐘🦂🐫🐑🦍🐯🐞'

def it = BreakIterator.getCharacterInstance()
it.setText(s3)

def start = it.first()
def end = it.next()
int n = 0

while (start < end) { 

    n++
    start = end 
    end = it.next()
}

println n
```

String length calculation in Groovy handles Unicode properly. For strings  
with emoji or special characters, BreakIterator provides accurate character  
counting that respects Unicode grapheme boundaries.  

## String searching

Find substrings and characters within strings using various search methods.  

```groovy
def text = 'an old falcon flies over the mountain'

println text.contains('falcon')
println text.indexOf('old')
println text.lastIndexOf('o')
println text.find(/f\w+/)
println text.findAll(/\w{4,}/)

def pattern = ~/\b\w{6,}\b/
println pattern.matcher(text).find()

println text.startsWith('an')
println text.endsWith('mountain')
println text.matches(/.*falcon.*/)
```

Groovy provides comprehensive string searching capabilities including  
simple contains checks, index finding, regular expression matching,  
and pattern-based searches for complex text analysis.  

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

---

```groovy
def user = [name: 'John Doe', age: 34]
println "$user.name is $user.age years old"
```

String interpolation with GStrings allows embedding variables and  
expressions directly in double-quoted strings. Complex expressions  
require ${} syntax for proper evaluation.  

## String replacement

Replace parts of strings using simple substitution and regular expressions.  

```groovy
def text = 'an old falcon'

println text.replace('old', 'young')
println text.replaceAll('a', 'A')
println text.replaceFirst('a', 'X')

def pattern = ~/\b\w{3}\b/
println text.replaceAll(pattern, 'XXX')

def html = '<p>Hello there</p>'
println html.replaceAll(/<\/?[^>]+>/, '')

def numbers = 'abc123def456ghi'
println numbers.replaceAll(/\d+/, '###')
```

String replacement operations support both literal text substitution and  
powerful regular expression-based replacements for complex text  
transformations and cleaning operations.  

## Formatting

```groovy
int age = 34
String name = 'John Doe'

String msg = sprintf "%s is %d years old", name, age
println msg
```

String formatting using sprintf provides precise control over output  
format with support for various data types, width specifications,  
and alignment options.  

## String splitting and joining

Split strings into arrays and join arrays back into strings.  

```groovy
def text = 'an old falcon flies high'
def words = text.split(' ')
println words

def csv = 'apple,banana,cherry'
def fruits = csv.split(',')
println fruits

def rejoined = words.join('-')
println rejoined

def numbers = '1,2,3,4,5'
def sum = numbers.split(',').collect { it as int }.sum()
println sum

def lines = 'line1\nline2\nline3'
println lines.split('\n')
```

String splitting converts strings to arrays using delimiters, while joining  
combines arrays back into strings. These operations are essential for  
text processing and data manipulation tasks.  

## Multiply

```groovy
println 'An old falcon'
println '-' * 15

println 'a foggy mountain'
println '-'.multiply(15)

println 'a sunny day'
```

String multiplication creates repeated patterns using the multiply operator  
or explicit multiply() method. This is useful for creating borders,  
separators, and formatted output displays.  

## Regular expressions

Use regular expressions for advanced pattern matching and text processing.  

```groovy
def text = 'Contact: john@example.com or call 555-1234'

def emailPattern = ~/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b/
def phonePattern = ~/\d{3}-\d{4}/

println text.find(emailPattern)
println text.find(phonePattern)
println text.findAll(/\b\w+\b/)

def validated = text ==~ /.*@.*\..*/
println validated

def words = 'apple123banana456cherry'
def extracted = words.findAll(/[a-z]+/)
println extracted
```

Regular expressions provide powerful pattern matching capabilities for  
email validation, phone number extraction, and complex text parsing  
operations in Groovy applications.  

## Iterate over lines 

```groovy
def text = """\
an old falcon
blue sky
rainy day
new computer
precious stone
"""

text.eachLine { line -> 
    println line
}
```

The eachLine method processes multi-line strings line by line, providing  
an efficient way to handle large text content without loading entire  
strings into memory at once.  

## Escape sequences and special characters

Handle special characters and escape sequences in strings properly.  

```groovy
def escaped = "Line 1\nLine 2\tTabbed"
println escaped

def quotes = "She said \"hello there\" to me"
println quotes

def backslash = "Path: C:\\Users\\Documents"
println backslash

def unicode = "Unicode: \u00A9 \u00AE \u2122"
println unicode

def rawString = /C:\Users\Documents\file.txt/
println rawString

def multiLine = """\
    First line
    Second line
    Third line
"""
println multiLine
```

Escape sequences allow including special characters in strings including  
newlines, tabs, quotes, and Unicode characters. Raw strings simplify  
handling of backslashes and complex paths.  

## String encoding and decoding

Work with different character encodings and byte representations.  

```groovy
def text = 'Hello there café'

def bytes = text.getBytes('UTF-8')
println bytes

def decoded = new String(bytes, 'UTF-8')
println decoded

def base64 = text.bytes.encodeBase64().toString()
println base64

def fromBase64 = new String(base64.decodeBase64())
println fromBase64

def urlEncoded = URLEncoder.encode(text, 'UTF-8')
println urlEncoded

def urlDecoded = URLDecoder.decode(urlEncoded, 'UTF-8')
println urlDecoded
```

String encoding operations convert between text and byte representations  
using various character sets. Base64 and URL encoding provide safe  
transport mechanisms for text data.  

## String reversal

Reverse strings using different approaches and techniques.  

```groovy
def text = 'falcon'

def reversed = text.reverse()
println reversed

def manual = text.toList().reverse().join('')
println manual

def words = 'an old falcon'
def wordReversed = words.split(' ').reverse().join(' ')
println wordReversed

def chars = words.split(' ').collect { it.reverse() }.join(' ')
println chars

def recursive = { String s ->
    s.length() <= 1 ? s : delegate(s[1..-1]) + s[0]
}
println recursive(text)
```

String reversal can be accomplished through built-in methods, manual  
character manipulation, or recursive algorithms. Different approaches  
suit different performance and readability requirements.  

## Substring operations

Extract portions of strings using various substring methods.  

```groovy
def text = 'an old falcon flies high'

println text.substring(3)
println text.substring(3, 6)
println text[3..5]
println text.take(6)
println text.drop(3)

def words = text.split(' ')
println words.take(3).join(' ')

def middle = text.substring(text.length() / 2 as int)
println middle

def beforeWord = text.substring(0, text.indexOf('falcon'))
println beforeWord.trim()

def afterWord = text.substring(text.indexOf('flies'))
println afterWord
```

Substring operations extract specific portions of strings using index-based  
methods, ranges, and functional approaches like take() and drop() for  
flexible text processing.  

## String validation and checking

Validate strings against various criteria and patterns.  

```groovy
def email = 'user@example.com'
def phone = '555-1234'
def empty = ''
def whitespace = '   '

println email.matches(/\w+@\w+\.\w+/)
println phone.matches(/\d{3}-\d{4}/)

println empty.isEmpty()
println whitespace.isBlank()
println whitespace.trim().isEmpty()

def isAlpha = { it.matches(/[a-zA-Z]+/) }
def isNumeric = { it.matches(/\d+/) }
def isAlphaNumeric = { it.matches(/[a-zA-Z0-9]+/) }

println isAlpha('falcon')
println isNumeric('12345')
println isAlphaNumeric('falcon123')

def hasDigits = text.find(/\d/)
println hasDigits != null
```

String validation uses regular expressions and built-in methods to check  
for empty strings, whitespace, and pattern compliance for data quality  
assurance in applications.  

## String tokenization

Break strings into tokens using various delimiters and patterns.  

```groovy
def sentence = 'The quick brown fox jumps over the lazy dog'

def tokens = sentence.tokenize(' ')
println tokens

def csvData = 'apple,banana;cherry:grape'
def multiDelim = csvData.tokenize(',;:')
println multiDelim

def code = 'if (x > 0) { return true; }'
def codeTokens = code.tokenize('(){}; ')
println codeTokens

def text = 'word1 word2,word3;word4:word5'
def customTokens = text.split(/[,;: ]+/)
println customTokens

def pathTokens = '/home/user/documents'.tokenize('/')
println pathTokens
```

Tokenization splits strings into meaningful units using single or multiple  
delimiters. This is essential for parsing structured text, code analysis,  
and data extraction tasks.  

## String transformation methods

Apply various transformation operations to modify string content.  

```groovy
def text = 'an old falcon'

println text.collect { it.toUpperCase() }.join('')
println text.findAll { it.isLetter() }.join('')
println text.findAll { it.isDigit() }.join('')

def transformed = text.split('').collect { char ->
    char.isLetter() ? char.toUpperCase() : char
}.join('')
println transformed

def camelCase = 'hello_world_example'.split('_').collect { word ->
    word.capitalize()
}.join('')
println camelCase.uncapitalize()

def kebabCase = 'HelloWorldExample'.replaceAll(/([A-Z])/, '-$1').toLowerCase()
println kebabCase.substring(1)
```

String transformations apply functions to characters or words, enabling  
format conversions, case transformations, and custom text processing  
operations for various naming conventions.  

## Multi-line strings and here-docs

Work with multi-line text using various string literal syntaxes.  

```groovy
def multiLine1 = """
First line
Second line
Third line
"""
println multiLine1

def multiLine2 = '''\
No leading newline
Second line
Third line
'''
println multiLine2

def indented = """
    Indented line 1
    Indented line 2
    Indented line 3
""".stripIndent()
println indented

def template = """
Dear ${'John'},
Welcome to our service.
Regards,
Team
"""
println template

def sql = """
SELECT name, age
FROM users
WHERE age > 25
ORDER BY name
"""
println sql.replaceAll(/\s+/, ' ').trim()
```

Multi-line strings support various quotation styles with different  
behaviors for leading whitespace, indentation, and interpolation  
capabilities for templates and formatted text.  

## StringBuilder advanced usage

Optimize string building operations with StringBuilder for performance.  

```groovy
def builder = new StringBuilder()

builder.append('Hello')
       .append(' ')
       .append('there')
       .append(' ')
       .append('world')

println builder.toString()

builder.insert(6, 'brave ')
println builder

builder.delete(6, 12)
println builder

builder.reverse()
println builder

def dynamicBuilder = new StringBuilder()
(1..5).each { 
    dynamicBuilder.append("Item $it\n")
}
println dynamicBuilder
```

StringBuilder provides efficient string construction through method chaining  
and in-place modifications, essential for performance-critical applications  
that build large strings incrementally.  

## String templates and GString usage

Leverage GString templating for dynamic string generation.  

```groovy
def name = 'Alice'
def age = 30
def city = 'Paris'

def template = "Name: $name, Age: $age, City: $city"
println template

def mathTemplate = "Result: ${2 + 3 * 4}"
println mathTemplate

def conditionalTemplate = "Status: ${age >= 18 ? 'Adult' : 'Minor'}"
println conditionalTemplate

def user = [name: 'Bob', score: 95]
def userTemplate = "$user.name scored $user.score points"
println userTemplate

def multiLineTemplate = """
User Report:
Name: $name
Age: $age  
City: $city
Generated: ${new Date()}
"""
println multiLineTemplate
```

GString templates enable dynamic string construction with embedded  
expressions, conditional logic, and object property access for  
flexible text generation scenarios.  

## String sorting and collections

Sort and manipulate collections of strings effectively.  

```groovy
def words = ['zebra', 'apple', 'banana', 'cherry']

println words.sort()
println words.sort { it.length() }
println words.sort { a, b -> b <=> a }

def byLength = words.groupBy { it.length() }
println byLength

def filtered = words.findAll { it.contains('a') }
println filtered

def transformed = words.collect { it.toUpperCase() }
println transformed

def joined = words.sort().join(', ')
println joined

def longestWord = words.max { it.length() }
println longestWord
```

String collections support sorting, grouping, filtering, and transformation  
operations using Groovy's powerful collection methods for comprehensive  
text data processing and analysis.  

## String file operations

Read from and write strings to files with various encoding options.  

```groovy
def content = 'Hello there\nSecond line\nThird line'

// Write to file
new File('/tmp/test.txt').text = content

// Read from file
def readContent = new File('/tmp/test.txt').text
println readContent

// Write with encoding
new File('/tmp/utf8.txt').write(content, 'UTF-8')

// Read lines as list
def lines = new File('/tmp/test.txt').readLines()
println lines

// Process large files line by line
new File('/tmp/test.txt').eachLine { line, number ->
    println "$number: $line"
}

// Append to file
new File('/tmp/test.txt').append('\nAppended line')
```

String file operations provide convenient methods for reading and writing  
text files with proper encoding support and memory-efficient line-by-line  
processing for large files.  

## String URL and path operations

Handle URLs and file paths using string manipulation techniques.  

```groovy
def url = 'https://example.com/path/to/resource?param=value&other=data'

def protocol = url.substring(0, url.indexOf('://'))
println protocol

def domain = url.split('/')[2]
println domain

def path = '/' + url.split('/', 4)[3].split('\\?')[0]
println path

def query = url.contains('?') ? url.split('\\?')[1] : ''
println query

def filePath = '/home/user/documents/file.txt'
def fileName = filePath.substring(filePath.lastIndexOf('/') + 1)
println fileName

def directory = filePath.substring(0, filePath.lastIndexOf('/'))
println directory

def extension = fileName.contains('.') ? 
    fileName.substring(fileName.lastIndexOf('.')) : ''
println extension
```

URL and path string operations extract components like protocols, domains,  
paths, and parameters using substring operations and regular expressions  
for web and filesystem applications.  

## String JSON operations

Parse and generate JSON strings for data interchange.  

```groovy
import groovy.json.JsonSlurper
import groovy.json.JsonBuilder

def jsonString = '{"name":"John","age":30,"city":"New York"}'

def slurper = new JsonSlurper()
def parsed = slurper.parseText(jsonString)

println parsed.name
println parsed.age
println parsed.city

def builder = new JsonBuilder()
builder {
    name 'Alice'
    age 25
    hobbies(['reading', 'swimming'])
    address {
        street '123 Main St'
        city 'Boston'
    }
}

println builder.toPrettyString()

def arrayJson = '[{"name":"Bob"},{"name":"Charlie"}]'
def array = slurper.parseText(arrayJson)
array.each { println it.name }
```

JSON string operations enable parsing JSON text into objects and generating  
JSON strings from data structures for web APIs and data exchange  
applications.  

## String XML operations

Parse and manipulate XML content using string-based approaches.  

```groovy
def xmlString = '''
<users>
    <user id="1">
        <name>John</name>
        <age>30</age>
    </user>
    <user id="2">
        <name>Alice</name>
        <age>25</age>
    </user>
</users>
'''

def xml = new XmlSlurper().parseText(xmlString)
println xml.user[0].name
println xml.user[0].age

xml.user.each { user ->
    println "User ${user.@id}: ${user.name} (${user.age})"
}

def builder = new groovy.xml.MarkupBuilder(new StringWriter())
def result = builder.books {
    book(id: '1') {
        title 'Groovy Guide'
        author 'Expert'
    }
}

println result.toString()
```

XML string operations provide parsing capabilities for extracting data  
from XML documents and building XML content programmatically for  
configuration and data exchange purposes.  

## String CSV operations

Process CSV data using string manipulation and parsing techniques.  

```groovy
def csvData = '''name,age,city
John,30,New York
Alice,25,Boston
Bob,35,Chicago'''

def lines = csvData.split('\n')
def headers = lines[0].split(',')
println headers

def records = lines[1..-1].collect { line ->
    def values = line.split(',')
    [headers, values].transpose().collectEntries()
}

records.each { record ->
    println "$record.name is $record.age years old from $record.city"
}

def generateCsv = { data ->
    def header = data[0].keySet().join(',')
    def rows = data.collect { row ->
        row.values().join(',')
    }.join('\n')
    "$header\n$rows"
}

def newData = [
    [name: 'Charlie', age: 28, city: 'Seattle'],
    [name: 'Diana', age: 32, city: 'Portland']
]
println generateCsv(newData)
```

CSV string operations parse comma-separated values into structured data  
and generate CSV output from collections for data import/export  
functionality in applications.  

## String password and security

Handle sensitive string data with security considerations.  

```groovy
import java.security.MessageDigest

def password = 'mySecretPassword'

// Hash password
def md5 = MessageDigest.getInstance('MD5')
def hashedBytes = md5.digest(password.bytes)
def hashed = hashedBytes.collect { String.format('%02x', it) }.join()
println hashed

// Generate salt
def salt = new Random().nextInt(100000).toString()
def saltedPassword = password + salt
println "Salted: $saltedPassword"

// Simple password strength check
def checkStrength = { pwd ->
    def checks = [
        hasUpper: pwd.find(/[A-Z]/) != null,
        hasLower: pwd.find(/[a-z]/) != null,
        hasDigit: pwd.find(/\d/) != null,
        hasSpecial: pwd.find(/[!@#$%^&*]/) != null,
        isLongEnough: pwd.length() >= 8
    ]
    checks.values().count(true)
}

println "Strength score: ${checkStrength(password)}/5"

// Mask sensitive data
def maskString = { str, start = 2, end = 2 ->
    str.length() <= start + end ? '*' * str.length() :
        str.take(start) + '*' * (str.length() - start - end) + 
        str.drop(str.length() - end)
}
println maskString('1234567890123456')
```

Security-oriented string operations include password hashing, strength  
validation, and data masking for protecting sensitive information in  
applications and logs.  

## String math and number parsing

Parse mathematical expressions and numbers from string representations.  

```groovy
def numbers = '12.34 56.78 90.12'
def values = numbers.split(' ').collect { it as Double }
println values.sum()

def expression = '2 + 3 * 4'
def result = Eval.me(expression)
println result

def currencies = ['$123.45', '€67.89', '¥1000']
def amounts = currencies.collect { 
    it.replaceAll(/[^\d.]/, '') as Double 
}
println amounts

def scientific = '1.23E-4'
println scientific as Double

def fraction = '3/4'
def parts = fraction.split('/')
def decimal = parts[0] as Double / parts[1] as Double
println decimal

def binary = '1101'
println Integer.parseInt(binary, 2)

def hex = 'FF'
println Integer.parseInt(hex, 16)
```

String-based mathematical operations parse numbers in various formats  
including decimals, scientific notation, fractions, and different number  
bases for calculation and data conversion purposes.  

## String date parsing

Parse and format dates from string representations.  

```groovy
import java.time.LocalDate
import java.time.LocalDateTime
import java.time.format.DateTimeFormatter
import java.text.SimpleDateFormat

def dateString = '2023-12-25'
def date1 = LocalDate.parse(dateString)
println date1

def dateTimeString = '2023-12-25T10:30:00'
def dateTime1 = LocalDateTime.parse(dateTimeString)
println dateTime1

def customFormat = 'Dec 25, 2023'
def formatter = DateTimeFormatter.ofPattern('MMM dd, yyyy')
def date2 = LocalDate.parse(customFormat, formatter)
println date2

def oldFormat = '25/12/2023 10:30'
def sdf = new SimpleDateFormat('dd/MM/yyyy HH:mm')
def date3 = sdf.parse(oldFormat)
println date3

def relative = '2023-12-25'
def parsed = LocalDate.parse(relative)
def daysFromNow = java.time.temporal.ChronoUnit.DAYS.between(
    LocalDate.now(), parsed)
println "Days from now: $daysFromNow"
```

Date parsing operations convert string representations to date objects  
using various formats and patterns, enabling temporal calculations and  
date manipulations in applications.  

## String localization

Handle internationalization and locale-specific string operations.  

```groovy
import java.text.Collator
import java.text.NumberFormat
import java.util.Locale

def words = ['café', 'naïve', 'résumé', 'apple']

// Locale-aware sorting
def collator = Collator.getInstance(Locale.FRENCH)
def sorted = words.sort(collator)
println sorted

// Number formatting
def amount = 1234567.89
def usFormat = NumberFormat.getCurrencyInstance(Locale.US)
def frFormat = NumberFormat.getCurrencyInstance(Locale.FRANCE)

println usFormat.format(amount)
println frFormat.format(amount)

// Case conversion with locale
def turkish = 'İstanbul'
println turkish.toLowerCase(Locale.forLanguageTag('tr'))
println turkish.toLowerCase(Locale.ENGLISH)

// Message formatting
def name = 'Alice'
def count = 3
def pattern = '{0} has {1,choice,0#no items|1#one item|' + 
    '1<{1,number,integer} items}'
def formatter = java.text.MessageFormat.getInstance(Locale.ENGLISH)
println formatter.format([name, count] as Object[])
```

Localization operations handle text sorting, number formatting, and  
case conversions according to specific locales for international  
application support and cultural adaptation.  

## String performance optimizations

Optimize string operations for better performance in applications.  

```groovy
// Use StringBuilder for multiple concatenations
def inefficient = ''
def efficient = new StringBuilder()

// Inefficient
(1..1000).each { inefficient += "Item $it\n" }

// Efficient  
(1..1000).each { efficient.append("Item $it\n") }

// String interning for frequent comparisons
def intern1 = 'constant'.intern()
def intern2 = 'constant'.intern()
println intern1.is(intern2)

// Avoid unnecessary string creation
def text = 'Hello World'
def better = text.substring(0, 5) // Creates new string only when needed

// Use join for collections
def words = ['one', 'two', 'three']
def joined = words.join(' ') // Better than manual concatenation

// Cache compiled patterns
def pattern = ~/\d+/
def numbers = ['abc123', 'def456', 'ghi789']
def extracted = numbers.collect { pattern.matcher(it).find() ? 
    pattern.matcher(it).group() : null }.findAll()
println extracted
```

Performance optimization techniques include using StringBuilder for  
concatenation, string interning for comparisons, and pattern caching  
for regular expressions to improve application efficiency.  

## String memory considerations

Understand memory implications of string operations and management.  

```groovy
// String immutability creates new objects
def original = 'Hello'
def modified = original + ' World' // New string created

// Monitor memory usage patterns
def largeString = 'x' * 1000000
def substring = largeString.substring(0, 10) 
// May hold reference to large string

// Use StringBuilder for memory efficiency
def builder = new StringBuilder(1000) // Pre-allocate capacity
(1..100).each { builder.append("Line $it\n") }

// Release references explicitly
largeString = null
System.gc() // Suggest garbage collection

// String pooling considerations
def pooled1 = 'literal'
def pooled2 = 'literal' 
def runtime = new String('literal')

println pooled1.is(pooled2) // true - same reference
println pooled1.is(runtime) // false - different objects

// Weak references for caches
import java.lang.ref.WeakReference
def cache = [:] as WeakHashMap
cache['key'] = 'Cached value'
println cache.size()
```

Memory management awareness helps avoid common pitfalls like substring  
memory leaks, excessive object creation, and inefficient string handling  
in memory-constrained applications.  

## String benchmarking and profiling

Measure and analyze string operation performance for optimization.  

```groovy
import java.util.concurrent.TimeUnit

def benchmark = { name, iterations, closure ->
    def start = System.nanoTime()
    iterations.times { closure() }
    def end = System.nanoTime()
    def duration = TimeUnit.NANOSECONDS.toMillis(end - start)
    println "$name: ${duration}ms for $iterations iterations"
}

// Compare concatenation methods
def iterations = 100000

benchmark('String +', iterations) {
    def result = ''
    (1..10).each { result += "item$it" }
}

benchmark('StringBuilder', iterations) {
    def builder = new StringBuilder()
    (1..10).each { builder.append("item$it") }
    builder.toString()
}

benchmark('Join', iterations) {
    (1..10).collect { "item$it" }.join('')
}

// Memory profiling
def before = Runtime.runtime.totalMemory() - Runtime.runtime.freeMemory()
def strings = (1..10000).collect { "String number $it" }
def after = Runtime.runtime.totalMemory() - Runtime.runtime.freeMemory()
println "Memory used: ${(after - before) / 1024}KB"

// Operation complexity analysis
def testSizes = [100, 1000, 10000]
testSizes.each { size ->
    def data = (1..size).collect { "item$it" }.join('')
    benchmark("indexOf-$size", 1000) {
        data.indexOf('item' + (size / 2 as int))
    }
}
```

Benchmarking string operations helps identify performance bottlenecks  
and choose optimal approaches based on actual measurements rather than  
assumptions about string handling efficiency.
