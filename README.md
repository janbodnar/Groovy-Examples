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

grepping/filtering  

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

    new User(name:'John Doe', occupation: 'gardener',   dob: LocalDate.parse('1973-09-07', 'yyyy-MM-dd')),
    new User(name:'Roger Roe',    occupation: 'driver',  dob: LocalDate.parse('1963-03-30', 'yyyy-MM-dd')),
    new User(name:'Kim Smith',  occupation: 'teacher', dob: LocalDate.parse('1980-05-12', 'yyyy-MM-dd')),
    new User(name:'Joe Nigel',    occupation: 'artist', dob: LocalDate.parse('1983-03-30', 'yyyy-MM-dd')),
    new User(name:'Liam Strong',   occupation: 'teacher',   dob: LocalDate.parse('2009-03-06', 'yyyy-MM-dd')),
    new User(name:'Robert Young',   occupation: 'gardener',   dob: LocalDate.parse('1978-11-16', 'yyyy-MM-dd')),
    new User(name:'Liam Strong',   occupation: 'teacher',   dob: LocalDate.parse('1986-10-23', 'yyyy-MM-dd'))
]
 
def res = users.groupBy({ it.occupation })

for (e in res) {
    println e
}
```

Group data by occupation

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


## Iterate over lines of URL

```groovy
def url = new URL("http://www.webcode.me")

url.eachLine { println it }
```

## Socket HEAD/GET request

```groovy
def s = new Socket("webcode.me", 80);

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

## Read file line by line  

```groovy
def fname = new File("words.txt")

fname.eachLine { ln, n ->
    println "line $n: $ln"
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

## LineChart created with JFreeChart

```groovy
package com.zetcode

@Grab('org.jfree:jfreechart:1.5.3')

import org.jfree.chart.ChartFactory
import org.jfree.chart.ChartPanel
import org.jfree.chart.JFreeChart
import org.jfree.chart.plot.PlotOrientation
import org.jfree.chart.plot.XYPlot
import org.jfree.chart.renderer.xy.XYLineAndShapeRenderer
import org.jfree.data.category.DefaultCategoryDataset
import org.jfree.chart.ChartUtils

def getChart() {

    def dataset = createDataset()
    return createChart(dataset)
}

def createDataset() {

    def dataset = new DefaultCategoryDataset();

    dataset.addValue(212, "Classes", "JDK 1.0");
    dataset.addValue(504, "Classes", "JDK 1.1");
    dataset.addValue(1520, "Classes", "SDK 1.2");
    dataset.addValue(1842, "Classes", "SDK 1.3");
    dataset.addValue(2991, "Classes", "SDK 1.4");

    return dataset
}

def createChart(dataset){

    def chart = ChartFactory.createLineChart(
        "Java Standard Class Library", // chart title
        "Release", // domain axis label
        "Class Count", // range axis label
        dataset, // data
        PlotOrientation.VERTICAL, // orientation
        false, // include legend
        true, // tooltips
        false // urls
        )
    
    return chart
}

def chart = getChart()

ChartUtils.saveChartAsPNG(new File("mychart.png"), chart, 450, 400);
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
        setDefaultCloseOperation(EXIT_ON_CLOSE);
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
