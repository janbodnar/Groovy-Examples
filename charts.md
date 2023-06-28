# Charts 

JFreeChart & XChart libraries  

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
