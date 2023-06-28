# Charts 

JFreeChart & XChart libraries  

## LineChart 

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

    def dataset = new DefaultCategoryDataset()

    dataset.addValue(212, "Classes", "JDK 1.0")
    dataset.addValue(504, "Classes", "JDK 1.1")
    dataset.addValue(1520, "Classes", "SDK 1.2")
    dataset.addValue(1842, "Classes", "SDK 1.3")
    dataset.addValue(2991, "Classes", "SDK 1.4")

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

ChartUtils.saveChartAsPNG(new File("mychart.png"), chart, 450, 400)
```

## Bar chart

```groovy
@Grab('org.jfree:jfreechart:1.5.3')

import org.jfree.chart.ChartFactory
import org.jfree.chart.ChartPanel
import org.jfree.chart.JFreeChart
import org.jfree.chart.plot.PlotOrientation
import org.jfree.chart.title.TextTitle
import org.jfree.data.category.DefaultCategoryDataset
import org.jfree.chart.ChartUtils

import java.awt.Font

def getBarChart() {

    def dataset = createBarDataset()
    return createBarChart(dataset)
}

def createBarDataset() {

    def dataset = new DefaultCategoryDataset()
    dataset.setValue(46, "Gold medals", "USA")
    dataset.setValue(38, "Gold medals", "China")
    dataset.setValue(29, "Gold medals", "UK")
    dataset.setValue(22, "Gold medals", "Russia")
    dataset.setValue(13, "Gold medals", "S. Korea")
    dataset.setValue(11, "Gold medals", "Germany")

    return dataset
}

def createBarChart(dataset) {

    def barChart = ChartFactory.createBarChart(
            "Olympic gold medals in London",
            "",
            "Gold medals",
            dataset,
            PlotOrientation.VERTICAL,
            false, true, false)

    barChart.setTitle(new TextTitle("Olympic gold medals in London",
        new Font("Serif", Font.BOLD, 16))
    )

    return barChart
}

def barchart = getBarChart()
ChartUtils.saveChartAsPNG(new File("barchart.png"), barchart, 550, 350)
```

## Area chart 

```groovy
@Grab('org.jfree:jfreechart:1.5.3')

import org.jfree.chart.ChartFactory
import org.jfree.chart.ChartPanel
import org.jfree.chart.JFreeChart
import org.jfree.chart.plot.CategoryPlot
import org.jfree.chart.plot.PlotOrientation
import org.jfree.chart.renderer.AreaRendererEndType
import org.jfree.chart.renderer.category.AreaRenderer
import org.jfree.chart.title.TextTitle
import org.jfree.data.category.DefaultCategoryDataset
import org.jfree.chart.ChartUtils

import java.awt.Font

def getAreaChart() {

    def dataset = createAreaDataset()
    return createAreaChart(dataset)
}

def createAreaDataset() {

    def dataset = new DefaultCategoryDataset()

    dataset.addValue(82502, "Oil", "2004")
    dataset.addValue(84026, "Oil", "2005")
    dataset.addValue(85007, "Oil", "2006")
    dataset.addValue(86216, "Oil", "2007")
    dataset.addValue(85559, "Oil", "2008")
    dataset.addValue(84491, "Oil", "2009")
    dataset.addValue(87672, "Oil", "2010")
    dataset.addValue(88575, "Oil", "2011")
    dataset.addValue(89837, "Oil", "2012")
    dataset.addValue(90701, "Oil", "2013")

    return dataset
}

def createAreaChart(dataset){

    def areaChart = ChartFactory.createAreaChart(
        "Oil consumption",
        "Time",
        "Thousands bbl/day",
        dataset,
        PlotOrientation.VERTICAL,
        false,
        true,
        true)

    def plot = (CategoryPlot) areaChart.getPlot()
    plot.setForegroundAlpha(0.3f)

    def renderer = (AreaRenderer) plot.getRenderer()
    renderer.setEndType(AreaRendererEndType.LEVEL)

    areaChart.setTitle(new TextTitle("Oil consumption",
        new Font("Serif", Font.BOLD, 16))
    )
    
    return areaChart
}


def areachart = getAreaChart()
ChartUtils.saveChartAsPNG(new File("areachart.png"), areachart, 550, 350)
```

## Candlestick

```groovy
@Grab(group='org.knowm.xchart', module='xchart', version='3.8.4')

import java.text.DateFormat
import java.text.SimpleDateFormat
import org.knowm.xchart.OHLCChartBuilder
import org.knowm.xchart.BitmapEncoder
import org.knowm.xchart.BitmapEncoder.BitmapFormat
import org.knowm.xchart.style.Styler
import org.knowm.xchart.style.Styler.ChartTheme


def getChart() {

    def chart = new OHLCChartBuilder().width(450).height(400)
        .title("My Chart").theme(ChartTheme.GGPlot2).build()

    chart.getStyler().setLegendPosition(Styler.LegendPosition.OutsideS)
    chart.getStyler().setLegendLayout(Styler.LegendLayout.Horizontal)

    DateFormat sdf = new SimpleDateFormat("yyyy-MM-dd")

    def xData = [sdf.parse("2017-01-01"), sdf.parse("2017-01-02"), sdf.parse("2017-01-03"), sdf.parse("2017-01-04"),
        sdf.parse("2017-01-05"), sdf.parse("2017-01-06"), sdf.parse("2017-01-07"), sdf.parse("2017-01-08"), 
        sdf.parse("2017-01-09"), sdf.parse("2017-01-10"), sdf.parse("2017-01-11"), sdf.parse("2017-01-12"), 
        sdf.parse("2017-01-13"), sdf.parse("2017-01-14"), sdf.parse("2017-01-15"), sdf.parse("2017-01-16"),
        sdf.parse("2017-01-17"), sdf.parse("2017-01-18"), sdf.parse("2017-01-19"), sdf.parse("2017-01-20")]
    def openData = [5000.0, 4757.2473929793405, 4937.933153377687, 4850.117798755733, 4680.445170834381, 
        4728.6033945787485, 4639.04671671885, 4530.096339147777, 4315.229765115681, 4388.220975399254,
        4489.697288087602, 4732.2132126939905, 4702.3966247309845, 4791.640117313282, 4901.38575875206, 
        5125.005575124998, 5339.946396170943, 5514.203125103359, 5605.7120913762365, 5658.4641692460655 ]
    def highData = [5011.625115831823, 5015.598840107868, 5004.555292655227, 4946.208284989638, 4800.7266299680105, 
        4733.951285736023, 4682.965749348238, 4593.042839842684, 4477.12221217068, 4503.283591544297, 
        4746.430219157109, 4814.011792774473, 4847.995393440874, 4904.50276031226, 5200.3865578332025, 
        5426.345431443595, 5516.270559972647, 5646.1703261538005, 5732.382218841784, 5694.726095511361]
    def lowData = [4756.08711241483, 4713.0075280141755, 4815.154612252833, 4613.815119457544, 4676.900203675839, 
        4548.156136279956, 4474.001485725063, 4306.370773477324, 4287.219701792791, 4348.675798536698, 
        4413.906124958164, 4657.285803043788, 4624.954232028435, 4787.258755266257, 4852.463837292509, 
        5101.189240790134, 5258.967294465301, 5505.353185225293, 5542.616442576277, 5429.70901528972]
    def closeData = [4757.2473929793405, 4937.933153377687, 4850.117798755733, 4680.445170834381, 4728.6033945787485, 
        4639.04671671885, 4530.096339147777, 4315.229765115681, 4388.220975399254, 4489.697288087602, 
        4732.2132126939905, 4702.3966247309845, 4791.640117313282, 4901.38575875206, 5125.005575124998, 
        5339.946396170943, 5514.203125103359, 5605.7120913762365, 5658.4641692460655, 5468.595045517461]

    chart.addSeries("Values", xData, openData, highData, lowData, closeData)
    return chart
}
  

def chart = getChart()
BitmapEncoder.saveBitmap(chart, "candlestick", BitmapFormat.PNG)
```

