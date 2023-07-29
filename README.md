# Groovy-Examples
Groovy code examples

`println System.getProperty("user.dir")` print current working directory  
`println "ls -la".execute().text` execute shell command  

```groovy
def isOdd = { int i -> (i & 1) as boolean }
def isEven = {int i -> ! isOdd(i) }
```
even/odd  

`def evens = [1, 2, 3, 4, 5].findAll{ it % 2 == 0 }` - filter out even values  

## Debug macros 

```groovy
def name = 'John Doe'
def vals = [1, 2, 3, -4, 5]
def words = ['game', 'cup', 'water'] as String[]
def r = 1..10

println SV(name, vals, words, r)
println SVI(name, vals, words, r)

println NV(r)
println NVL(r)
```

## Import Groovy code in script

```groovy
package com.zetcode 

// User.groovy
record User(String name) {}
```

```groovy
package com.zetcode

// main.groovy
this.class.classLoader.parseClass("User.groovy")

def u = new User('John Doe')
println u
```

Run the app `$ groovy com\zetcode\main.groovy`  

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

## Steps

```groovy
1.step(4, 0.5) { print "$it "}
println "\n-----------------------"

1.step(10, 1) { 
    
    println it
}
```

## Pick random element from list

```groovy
def words = ["sky", "cup", "tall", "falcon", "cloud"]

def rnd = new Random()

def ri = rnd.nextInt(words.size())
println words[ri]
```


## Unix pipes

```groovy
def p = 'ls -l'.execute()  | ['awk', '{ print $1, $NF }'].execute()
p.waitFor()
println p.text
```

## Download image

```groovy
import java.nio.file.Files
import java.nio.file.Paths

def data = "http://webcode.me/favicon.ico".toURL().bytes
Files.write(Paths.get('favicon.ico'), data)
```

## Iterate runes

```groovy
import java.text.BreakIterator

def text = "🐜🐬🐄🐘🦂🐫🐑🦍🐯🐞"

def it = BreakIterator.getCharacterInstance()
it.setText(text)

def start = it.first()
def end = it.next()

while (start < end) { 

    println(text.substring(start, end))
    start = end 
    end = it.next()
}
```

## Create a deck of cards

```groovy
def signs = (2..10) + ['J', 'Q', 'K', 'A']
def symbols = ['♣', '♦', '♥', '♠']
 
// [♣2, ♦2, ♥2, ♠2, ..., ♣A, ♦A, ♥A, ♠A]
def cards = [symbols, signs].combinations().collect { it.join() }
println cards

cards.shuffle()

println cards.take(5)
```

## Shuffle emoji cards

```groovy

def cards = [
    "🂡", "🂱", "🃁", "🃑",
    "🂢", "🂲", "🃂", "🃒",
    "🂣", "🂳", "🃃", "🃓",
    "🂤", "🂴", "🃄", "🃔",
    "🂥", "🂵", "🃅", "🃕",
    "🂦", "🂶", "🃆", "🃖",
    "🂧", "🂷", "🃇", "🃗",
    "🂨", "🂸", "🂸", "🂸",
    "🂩", "🂩", "🃉", "🃙",
    "🂪", "🂺", "🃊", "🃚",
    "🂫", "🂻", "🃋", "🃛",
    "🂭", "🂽", "🃍", "🃝",
    "🂮", "🂾", "🃎", "🃞"
]

def show(cards) {

    cards.eachWithIndex { e, i -> 

        if (i != 0 && i % 13 == 0) {
            print "\n"
        }

        print "${e} "
    }

    print "\n"
}


cards.shuffle()
show(cards)
```

## Ranges 

```groovy
import java.time.LocalDate
import java.time.Month

def vals = 1..10
println vals.from
println vals.to
println vals.toList()

// steps
def vals2 = (1..7).step(2)
println vals2

// reverse range
def vals3 = 10..1
println vals3

// exclusive ranges
def vals4 = 1..<10
println vals4.toList()

def vals5 = 1<..<10
println vals5.toList()

def chars = 'a'..'z'
println chars 
println chars.size()
println chars.contains('c')
println chars.getFrom() 
println chars.getTo()

// date & time 
def months = Month.JANUARY..Month.DECEMBER

for (def m in months) {

    println m
}

def days = LocalDate.now()..LocalDate.now().plusDays(17)

for (def d in days) {
    
    println d
}
```


## grepping/filtering  

```groovy
def res = [1, 15, 16, 30, 12].grep(12..18)
println res

def res2 = [1, 2, -1, 0, 2, 3, -3, 4].grep({it > 0})
println res2

def words = ['sky', 'cloud', 'war', 'wasp', 'water', 'coin']
def res3 = words.grep(~/.../)
println res3
```

```groovy
def gcd(a,b) {
  if (!b) a
  else gcd(b, a%b)
}
```
Custom GCD with recursion

## Filter lines

```groovy
def fname = 'words.txt'
def f = new File(fname)

// def res = f.filterLine { it =~ /^w.*/ }
def res = f.filterLine { it =~ /^w.*/ }
println res
```

## Process

```groovy
def process = "ls -l".execute()

process.in.eachLine { line ->
    println line
}
```
Runs external program

```groovy
def c1 = "perl --version"
def c2 = "perl -e 'system(\"echo Hello there!\")'"

println c1.execute().text
println "--------------------------------"
println c2.execute().text
```
Runs Perl one liners  

## Grouping

```groovy
import java.time.LocalDate
import groovy.transform.Immutable

@Immutable
class User {
    String name
    String occupation
    LocalDate dob
}
 
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

Group data by occupation

## Records 

Groovy 4 introduced records  

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



## Find GCD from the max & min in a list 

```groovy
def vals = [8, 5, 10]

def min = vals.min() as BigInteger
def max = vals.max() as BigInteger

println min.gcd(max)
```
Casting to BigInteger

```groovy
def vals = [8g, 5g, 10g]

def min = vals.min()
def max = vals.max()

println min.gcd(max)
```
Using BigInteger literals

## Read CSV file

```groovy
@Grab(group='com.opencsv', module='opencsv', version='5.5.2')

import java.nio.file.Files
import java.nio.file.Paths
import com.opencsv.CSVParserBuilder
import com.opencsv.CSVReaderBuilder

def fname = 'src/resources/cars.csv'
def br = Files.newBufferedReader(Paths.get(fname))

def parser = new CSVParserBuilder().withSeparator(',' as char).build()
def reader = new CSVReaderBuilder(br).withCSVParser(parser).build()

def lines = reader.readAll()

for (line in lines) {
    println line.join(" ")
}
```
reads CSV file with OpenCSV library

## Convert CSV file to HTML table

```groovy
package com.zetcode

@Grab(group='org.codehaus.groovy', module='groovy-xml', version='3.0.9')
@Grab(group='com.opencsv', module='opencsv', version='5.5.2')

import java.nio.file.Files
import java.nio.file.Paths
import com.opencsv.CSVParserBuilder
import com.opencsv.CSVReaderBuilder
import groovy.xml.MarkupBuilder

def fname = 'cars.csv'
def br = Files.newBufferedReader(Paths.get(fname))

def parser = new CSVParserBuilder().withSeparator(',' as char).build()
def reader = new CSVReaderBuilder(br).withCSVParser(parser).build()

def lines = reader.readAll()

def buildTable(rows, n) {
    
    def sw = new StringWriter()
    
    new MarkupBuilder(sw).table() {
        thead {
            tr {
                rows[0].each { f -> th(f)}
            }
        } 
        tbody {
            (1..n).each { row ->
                tr {
                    rows[row].each { f -> td(f) }
                }
            }
        }
    }
    
    sw.toString()
}

println buildTable(lines.toList(), lines.size()-1)
```

## Parse XML

```groovy
import groovy.xml.XmlSlurper

def data = '''<?xml version="1.0" encoding="utf-8"?>
<products>
    <product>
        <id>1</id>
        <name>Product A</name>
        <price>780</price>
    </product>

    <product>
        <id>2</id>
        <name>Product B</name>
        <price>1100</price>
    </product>

    <product>
        <id>3</id>
        <name>Product C</name>
        <price>1050</price>
    </product>

    <product>
        <id>4</id>
        <name>Product D</name>
        <price>950</price>
    </product>
</products>
'''

def products = new XmlSlurper().parseText(data)

def id1 = products.product[0].id
def name1 = products.product[0].name
def price1 = products.product[0].price

println "$id1 $name1 $price1"

def names = products.'**'.findAll { node -> node.name() == 'name' }*.text()
println names

def prices = products.product.'*'.find { node ->

    node.name() == 'price' && node.text() as Integer < 1000
}

print prices
```


## Iterate over lines of URL

```groovy
def url = new URL("http://www.webcode.me")

url.eachLine { println it }
```

## Socket HEAD/GET request

```groovy
def s = new Socket("webcode.me", 80)

s.withStreams { sin, sout ->
  
  sout << "HEAD / HTTP/1.1\r\nConnection: close\r\nHost: webcode.me\r\nAccept: text/html\r\n\r\n" 
  
  def reader = sin.newReader()
  def lines = reader.readLines()
  
  for (line in lines) {
      println line
  }
}
```

```groovy
def host = "webcode.me"
int port = 80

def s = new Socket(host, port)

s.setSoTimeout(18000)

s.withStreams { sin, sout ->

    sout << "GET / HTTP/1.1\r\n"
    sout << "Connection: close\r\n"
    sout << "Host: www.webcode.me\r\n\r\n"

    def text = sin.text
    println text
}
```

## Web scraping with JSoup  

```groovy
@Grab(group='org.jsoup', module='jsoup', version='1.10.1')
import org.jsoup.Jsoup

def doc = Jsoup.connect("http://webcode.me").get()
def title = doc.select('title')
def elements = doc.select('*')

println doc
println title

elements.each { e ->
    println e
}
```

## Files

```groovy
def f = new File("words.txt")

f.eachLine { ln, n ->
    println "line $n: $ln"
}
```
Read file line by line

---

```groovy
def fname = 'src/resources/words.txt'
def f = new File(fname)

def res = f.filterLine { it =~ /^w.*/ }
println res
```
filter lines; include only that start with 'w'

---

```groovy
def fname = 'words.txt'
def f = new File(fname)

println f.text 
```
read contents

---

```groovy
def fname = 'words.txt'
def f = new File(fname)

// def lines = f as String[]
// println lines

def lines = f.readLines()
println lines
```
read lines into list

---

```groovy
def fname = 'words.txt'
def f = new File(fname)

f.withWriterAppend('utf-8') {
	writer -> writer.append('falcon\n').append('owl')
}
```
append to file  

## Undertow server 

```groovy
package com.zetcode

@Grab(group='io.undertow', module='undertow-core', version='2.3.7.Final')

import io.undertow.Undertow
import io.undertow.util.Headers

def server = Undertow.builder()
        .addHttpListener(8080, "localhost")
        .setHandler(exchange -> {
            exchange.getResponseHeaders().put(Headers.CONTENT_TYPE, "text/plain")
            exchange.getResponseSender().send("Hello there")
        }).build()

server.start()
```

## Working with H2 in-memory database

```groovy
@GrabConfig(systemClassLoader=true)
@Grab(group='com.h2database', module='h2', version='1.4.200')

import groovy.sql.Sql

def createTable = '''
CREATE TABLE cars(id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(255), price INT);
INSERT INTO cars(name, price) VALUES('Audi', 52642);
INSERT INTO cars(name, price) VALUES('Mercedes', 57127);
INSERT INTO cars(name, price) VALUES('Skoda', 9000);
INSERT INTO cars(name, price) VALUES('Volvo', 29000);
INSERT INTO cars(name, price) VALUES('Bentley', 350000);
INSERT INTO cars(name, price) VALUES('Citroen', 21000);
INSERT INTO cars(name, price) VALUES('Hummer', 41400);
INSERT INTO cars(name, price) VALUES('Volkswagen', 21600);
'''

def url = "jdbc:h2:mem:"
def sql = Sql.withInstance(url) { sql ->

    sql.execute("DROP TABLE IF EXISTS cars")
    sql.execute(createTable)

    def query = "SELECT * FROM cars"

    sql.eachRow(query) { row ->
      println "$row.id  ${row.name} $row.price"
    }
}
```

## Fetch all rows from db  

```groovy
@Grab(group='org.postgresql', module='postgresql', version='42.2.24')
@GrabConfig(systemClassLoader=true)

import groovy.sql.Sql

def url = 'jdbc:postgresql://localhost:5432/testdb'
def user = 'postgres'
def password = ''
def driver = 'org.postgresql.Driver'

Sql.withInstance(url, user, password, driver) { sql ->

    def res = sql.rows('SELECT * FROM cars')
    println res
}
```

## Find all text files  

```groovy
import static groovy.io.FileType.FILES

def f = new File('/home/jano7/Documents')

f.eachFileRecurse(FILES) {
    if(it.name.endsWith('.txt')) {
        println it
    }
}
```

```groovy
import static groovy.io.FileType.FILES

def f = new File('/home/jano7/Documents')

f.traverse(type: FILES, nameFilter: ~/.*.txt/) { it ->
	println it
}
```

The *nameFilter* is an exact match   

## Iterate over range of datetime values

```groovy
@Grab('org.codehaus.groovy:groovy-datetime:3.0.9')

import java.time.Instant
import java.time.temporal.ChronoUnit

def now = Instant.now()
println now

def d2 = now.plus(3, ChronoUnit.DAYS)

println "--------------"

now.upto(d2, ChronoUnit.DAYS) {
	
	println it
}
```

## SFTP list directory

```groovy
@Grab(group='com.hierynomus', module='sshj', version='0.35.0')

import net.schmizz.sshj.SSHClient
import net.schmizz.sshj.transport.verification.PromiscuousVerifier
import net.schmizz.sshj.sftp.SFTPClient
import net.schmizz.sshj.sftp.RemoteResourceInfo

try {

    def client = new SSHClient()

    client.addHostKeyVerifier(new PromiscuousVerifier())
    client.connect("hostname")
    client.authPassword("username", "s$cret")
    
    SFTPClient sftp = client.newSFTPClient()

    List<RemoteResourceInfo> resourceInfoList = sftp.ls("/web/perl/")

    for (def e : resourceInfoList) {
        println(e.getName())
    }

} finally {
    sftp.close() 
    client.disconnect()
}
```


## TableSaw dataframe 

```groovy
package com.zetcode

@Grab(group='tech.tablesaw', module='tablesaw-core', version='0.41.0')
@Grab(group='org.slf4j', module='slf4j-simple', version='1.7.32', scope='test')

import tech.tablesaw.api.Table
import static tech.tablesaw.aggregate.AggregateFunctions.max
import static tech.tablesaw.aggregate.AggregateFunctions.max
import static tech.tablesaw.aggregate.AggregateFunctions.mean
import static tech.tablesaw.aggregate.AggregateFunctions.median
import static tech.tablesaw.aggregate.AggregateFunctions.count

def tbl = Table.read().csv("src/resources/cars.csv")

println tbl.shape()
println '---------------------------'

println tbl.structure()
println '---------------------------'

println tbl.first(3)
println '---------------------------'

println tbl.last(2)
println '---------------------------'

println tbl.column('price').min()
println tbl.column('price').max()
println tbl.column('price').asList()

println tbl.where { it.numberColumn(2).isGreaterThan(50_000) }
println tbl.where { it.numberColumn(2).isLessThan(25_000) }.rowCount()

println '---------------------------'

def price = tbl.nCol('price')
println tbl.summarize(price, count, max, min, mean, median).apply()

println '---------------------------'

println tbl.select("name", "price")


println '---------------------------'
println tbl.sampleN(4)

println '---------------------------'

println tbl.sortAscendingOn("price")
```

## Swing example

The first example uses Swing directly, without the builder.

```groovy
package com.zetcode

import javax.swing.GroupLayout
import javax.swing.JButton
import javax.swing.JComponent
import javax.swing.JFrame
import java.awt.EventQueue

class QuitButtonEx extends JFrame {

    QuitButtonEx() {

        initUI()
    }

    def initUI() {

        def quitButton = new JButton("Quit")
        quitButton.addActionListener { e -> System.exit(0) }

        createLayout(quitButton)

        setTitle("Quit button")
        setSize(350, 250)
        setLocationRelativeTo(null)
        setDefaultCloseOperation(EXIT_ON_CLOSE)
    }

    private void createLayout(JComponent... arg) {

        def pane = getContentPane()
        def gl = new GroupLayout(pane)
        pane.setLayout(gl)

        gl.setAutoCreateContainerGaps(true)

        gl.setHorizontalGroup(gl.createSequentialGroup()
                .addComponent(arg[0]))

        gl.setVerticalGroup(gl.createSequentialGroup()
                .addComponent(arg[0]))
    }
}


EventQueue.invokeLater {

    def ex = new QuitButtonEx()
    ex.setVisible(true)
}
```

## Playwright  

Browser automation with Playwright  

```groovy
@Grab(group='com.microsoft.playwright', module='playwright', version='1.19.0')

import com.microsoft.playwright.Playwright

Playwright.create().withCloseable { client -> 

    def browser = client.chromium().launch()

    def page = browser.newPage()

    page.navigate("http://webcode.me")
    println page.title()
}
```

## Groovy template engine

### Simple

```groovy
import groovy.text.SimpleTemplateEngine

def text = 'Hello $name!'

def ctx = ["name": "John Doe"]

def engine = new SimpleTemplateEngine() 
def res = engine.createTemplate(text).make(ctx)

println res
```

### From file

```jsp
${name} is a ${occupation}
```

This is `message.txt` file.

```groovy
import groovy.text.SimpleTemplateEngine

def text = new File("message.txt").text
def ctx = ["name": "John Doe", "occupation": "gardener"]

def engine = new SimpleTemplateEngine() 
def res = engine.createTemplate(text).make(ctx)

println res
```

### Loop 

```groovy
import groovy.text.SimpleTemplateEngine

def words = [ "sky", "blue", "cran", "cotton", "wood" ]

def ctx = [words: words]

def engine = new SimpleTemplateEngine()
def text = '''<% words.each { println "${it} has ${it.length()} characters" } %>'''

def res = engine.createTemplate(text).make(ctx)
println res
```

### Records

```groovy
import groovy.text.SimpleTemplateEngine

record User(String name, String occupation) {}

def text = '<% users.each { println "${it.name} is a ${it.occupation}" } %>'

def users = [ 
    new User("John Doe", "gardener"), new User("Roger Roe", "driver"), 
    new User("Peter Smith", "teacher")
]

def ctx = [users: users]

def engine = new SimpleTemplateEngine()

def res = engine.createTemplate(text).make(ctx)
println res
```

### If condition

```jsp
<% for (task in tasks) { %>
    <% if (task.done) { %>
        <%= task.title %>
    <% } %>
<% } %>
```

```groovy
import groovy.text.SimpleTemplateEngine
import groovy.transform.Immutable

@Immutable
class Task {
    String title
    boolean done
}

def text = new File("tasks.txt").text

def tasks = [ 
    new Task("Task 1", true), new Task("Task 2", true), 
    new Task("Task 3", false), new Task("Task 4", true), 
    new Task("Task 5", false) 
]

def ctx = ["tasks": tasks]

def engine = new SimpleTemplateEngine()

def res = engine.createTemplate(text).make(ctx)
def out = res.toString()

println out.replaceAll("\s{2,}", "").replaceAll("\n{2,}", "\n").strip()
```


## Jinjava template engine

```jinja
{%- for task in tasks -%}
    {% if task.done %}
        {{ task.title }}
    {% endif %}
{%- endfor %}
```
This is `tasks.jinja` template file.

```groovy
@Grab(group='com.hubspot.jinjava', module='jinjava', version='2.6.0')

import com.hubspot.jinjava.Jinjava
import groovy.transform.Immutable

@Immutable
class Task {
    String title
    boolean done
}

def jnj = new Jinjava()

def tasks = [ new Task("Task 1", true),
    new Task("Task 2", true), new Task("Task 3", false),
    new Task("Task 4", true), new Task("Task 5", false) ]

def context = ["tasks": tasks]

def fileName = "tasks.jinja"
def template = new File(fileName).text

def res = jnj.render(template, context)
println res

// record Task(String title, boolean done) { }
```

Records are not working, we use `@Immutable` instead.  

## JasperReports simple example

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<!DOCTYPE jasperReport PUBLIC "//JasperReports//DTD Report Design//EN"
        "http://jasperreports.sourceforge.net/dtds/jasperreport.dtd">

<jasperReport xmlns = "http://jasperreports.sourceforge.net/jasperreports"
              xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation = "http://jasperreports.sourceforge.net/jasperreports
                                    http://jasperreports.sourceforge.net/xsd/jasperreport.xsd"
              name="simple" topMargin="20" bottomMargin="20">

    <detail>
        <band height="70">

            <staticText>
                <reportElement x="5" y="50" width="100" height="14"/>
                <textElement />
                <text><![CDATA[This is my first report!]]></text>
            </staticText>

        </band>
    </detail>

</jasperReport>
```
`simple.jrxml` file  

```groovy
@Grab(group='net.sf.jasperreports', module='jasperreports', version='6.17.0')

import net.sf.jasperreports.engine.*

def xmlFile = 'simple.jrxml'
def jreport = JasperCompileManager.compileReport(xmlFile)

def params = [:]
def jsPrint = JasperFillManager.fillReport(jreport, params,
        new JREmptyDataSource())

JasperExportManager.exportReportToPdfFile(jsPrint, 'simple.pdf')
```

`report2.gvy` file  
