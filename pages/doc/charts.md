---
title: Charts
keywords: getting started
tags: [getting started, charts]
sidebar: doc_sidebar
toc_level: 2
permalink: charts.html
summary: Learn about Wavefront chart types and chart configuration options.
---

Charts allow you to view and examine your metrics. You can interact directly with charts--zoom in, zoom out, change the time window, and so on. You don't receive a static image of your data but you can work with your charts in real time, asking questions and receiving answers.

Wavefront supports a rich set of chart types and chart configuration options.

* You add [Wavefront Query Language](query_language_getting_started.html) queries to the chart to view and perform operations on metrics.
* You configure chart options to show what's important to you.

## Common Options

Common Options are available for all chart types. They include Query, General, Axis, Unit, Style, Description, and Legend.

### Query

For each query displayed in a chart you can set the axis and color:

- Axis - whether the axis displays on the left or right
- Color - the color of the query points

To display the options, hover over the query line.

![chart query](images/chart_query.png)

The image above shows the query line with Query Builder disabled. The options are the same with Query Builder enabled.

### General

Options that support general customization.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Name</td>
<td>Name of the chart. When you enter a chart name, it displays in the top right of the chart. There's no restriction on what you can enter as a chart name.
</td>
</tr>
<tr>
<td>Point Tag Display Options</td>
<td>The <a href="query_language_point_tags.html">point tags</a> to display in the chart legend or a Tabular View chart.
<ul>
<li><strong>Show all</strong> - Show all point tags</li>
<li><strong>Top</strong> - Show top N most frequent point tags</li>
<li><strong>Custom</strong> - Show point tags of specific point tag keys</li>
</ul></td>
</tr>
<tr>
<td>Summarize By</td>
<td>When displaying metrics, Wavefront determines the chart resolution and summarizes data into point buckets based on a summarization method. You can choose the method to apply to the data values within each point bucket. The summarization method displays in all caps next to the chart time bar.
<div>
You can summarize the raw data values within each point bucket by <strong>Average</strong>, <strong>Median</strong>, <strong>Min</strong>, <strong>Max</strong>, <strong>Count</strong>, and <strong>Sum</strong>. Suppose the horizontal scale for your chart is "240 point buckets across, 1 bucket – 30 sec (est)". When you choose the <strong>Median</strong> summarization method, the raw data values reported in each 30 second interval are aggregated, and the median value displays as a point.</div>

<div>The <strong>Count</strong> summarization method counts the <em>number</em> of data values reported in each 30 second interval, and displays that value to represent the point bucket. <strong>First</strong> assigns a value to each point bucket based on the <em>first</em> data value reported within the interval. <strong>Last</strong> works in a similar manner, but the point bucket value is based on the <em>last</em> data value reported within the interval.</div></td>
</tr>
<tr>
<td>Display Source Events</td>
<td>Whether to display events generated by a firing alert associated with a source displayed on the chart.
You can also use events() queries to <a href="charts_events_displaying.html">Display events in charts</a>.</td>
</tr>
<tr>
<td>Interpolate Points</td>
<td>Whether to interpolate points that exist only in the past or future into the current time window.
</td>
</tr>
<tr>
<td>Include Obsolete Metrics</td>
<td>Whether to display metrics that have not reported data values in the last 4 weeks. Selecting this option is useful if you are looking at data from 4 or more weeks ago, however, performance is slower when this option is turned on.</td>
</tr>
</tbody>
</table>

### Axis

Options that control the chart axis or axes.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Y-Axis</td>
<td>The scale of the Y-axis: linear or logarithmic. Default is linear. In most cases, linear is sufficient as long as there is not a large difference in measurement between the reported data points. If there's a large difference in measurement scale, use the logarithmic scale. By default, the logarithmic scale is set to the power of 10. You can configure the scale in the adjacent text field.
</td>
</tr>
<tr>
<td>Min/Max</td>
<td>The minimum and maximum value on the Y-axis. If you are using a double Y-axis, you can specify min/max values for each Y-axis (By default, set to auto).</td>
</tr>
<tr>
<td>Unit</td>
<td>The unit of measurement to assign to the reported chart values label that appears along the Y-axis of the chart. The supported units are:
<ul>
<li>Time - Ranges from yoctoseconds (ys) to years (yr)</li>
<li>IEC/Binary - data size in IEC/Binary units. Ranges from B (bytes) to YiB</li>
<li>SI - data rate in SI units. Ranges from bps (bits/s) to Ybps</li>
</ul>

For example, if the data for <pre>ts("requests.latency")</pre> is in milliseconds, you can either enter <strong>ms</strong> in the text field or click the <strong>Unit</strong> down-arrow and select <strong>Time &gt; ms</strong>.

The specified unit is merely a label and <em>does not</em> change the unit of measurement for the given expression. If you are using a double Y-axis, you can specify a unit for each Y-axis.</td>
</tr>
</tbody>
</table>

For information on unit prefixes and dynamic units, see [Units in Chart Axes and Legends](charts_customizing.html#units_in_chart_axes_and_legends).

### Style

Options that control the style of the chart.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Gap Threshold</td>
<td>Controls when data is considered missing when there are gaps in the reporting of the data. The gap threshold is expressed in seconds and defaults to 60 seconds. Data considered missing based on the threshold are shown as dotted lines. 
</td>
</tr>
<tr>
<td>Interpolation</td>
<td>The function used to join points between each point bucket:
<ul>
<li><strong>Linear</strong> - a straight line.</li>
<li><strong>Step Before</strong> - a step value at the beginning of the bucket.</li>
<li><strong>Step Before</strong> - a step value at the end of the bucket.</li>
<li><strong>Basis</strong> - a B-spline.</li>
<li><strong>Cardinal</strong> - a Cardinal spline.</li>
<li><strong>Monotone</strong> - a cubic interpolation that preserves monotonicity.</li>
</ul>
</td>
</tr>
</tbody>
</table>

### Description
A description of the chart.

### Legend

Controls the legend displayed for the chart.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Fixed Legend</td>
<td>Whether to display a fixed legend.
</td>
</tr>
<tr>
<td>Non-summarized Stats</td>
<td>Whether to report summarized or raw values for all metric values and statistics. When this setting is disabled, the legend reports summarized values according to the <strong>Summarize By</strong> setting.
</td>
</tr>
<tr>
<td>Disable Legend on Hover</td>
<td>Whether to display the legend when hovering over the chart.
</td>
</tr>
<tr>
<td>Position</td>
<td>Position of the fixed legend on screen.
</td>
</tr>
<tr>
<td>Display</td>
<td>The values and statistics to display in the legend: current, mean, median, sum, min, max, and count.
</td>
</tr>
<tr>
<td>Filter</td>
<td>The value and number of metrics displayed in the legend. Specify:
<ul>
<li>Top or Bottom</li>
<li>Number of metrics</li>
<li>Value or statistic</li>
</ul>
</td>
</tr>
</tbody>
</table>


## Line Plot

![line plot](images/line_plot.png)

A **line plot** chart represents interpolated point buckets. The X axis represents the amount of time in your time window and the Y axis represents the value associated with the data based on that time.

In a line chart, missing data is represented by a dashed line. The dashed line only gives a visual representation of the data stream; it _does not_ represent values of the missing data. If you hover over the chart, you won't see values where the gap threshold has been applied. Use the Gap Threshold property to set the amount of time before gaps of missing data display as dashed lines.

## Point Plot

![point plot](images/point_plot.png)

A **point plot** chart displays point buckets *without* any interpolation. Like a line chart, the X-axis represents the amount of time in your time window, and the Y-axis represents the value associated with the data during that time window.

## Stacked Area

![stacked area](images/stacked_area.png)

A **stacked area** chart is based on the line chart and behaves similarly except that:

The magnitude of each line is filled in as a solid block with blocks stacked one on top of each other. With the default Stack Type of zero, the peak of the chart at any time is the sum of the magnitudes of all  sources at that time.

The stacked area chart is a great way to visualize data when you want to determine at a glance which queries have the largest magnitude at any point in time. It is most commonly used to visually compare two or more quantities.

The **Stack** Type option controls the style of a Stacked Area chart.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Stack Type</td>
<td>
The following stack types are supported.
<ul>
<li><strong>Zero</strong> - Displays the chart as from 0 up to the sum of all points at that time interval. Default.</li>
<li><strong>Normalize to 0-1</strong> - Results in a similar shape to <strong>Zero</strong> except that the values are normalized so that they fill the range between 0 and 1 with the peak of the chart always a solid line drawn at magnitude 1.</li>
<li><strong>Minimize Weighted Change</strong> - Plots the area while attempting to minimize the weighted change in the slope of the lines. Both this and the <strong>Center the Stream</strong> option tend to result in similar shapes in which the chart does not show a solid area beginning at 0.</li>
<li><strong>Center the Stream</strong> - Represents the collective magnitude of the queries displayed on the chart with the band narrowing or widening as the metrics fluctuate over time.</li>
</ul>
</td>
</tr>
</tbody>
</table>

Here's an example of **Center the Stream**:

![center stream](images/center_stream.png)

## Scatter Plot

![scatter plot](images/scatter_plot.png)

A **scatter plot** differs from all other Wavefront charts in that it compares time series expressions against one another. All other Wavefront charts compare time series against time.

The scatter plot is useful for examining whether two (or more) data sets are positively, negatively, or not correlated. Each point on a scatter plot represents a summary of points over a specified amount of time. In Wavefront terminology, these summarized points are called buckets. You can view the extent of the summarization applied in the bottom left of each chart.

When you create a Wavefront scatter plot, you use two queries. One specifies the expression that is mapped to the X axis, the other the expression that is mapped to the Y axis.

Series matching for the scatter plot ensures that reporting sources are actively reporting metrics for all specified time series expressions.
* If a unique series (metric + source + point tags) is actively reporting for only one time series expression, it is not displayed.
* If no sources are reporting for all time series expressions, then NO DATA displays on the chart.
* If multiple X and Y-axes are defined, ensure that each time series expression associated to an axes has at least one common source reporting. Otherwise NO DATA is shown on the chart.

Only unique series that are reporting for every defined time series expression display.

The following option controls the style of a Scatter Plot chart.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Use Time-based Coloring</td>
<td>Modify the color of the data points so that darker colors represent more recent data, and lighter colors represent older data.
</td>
</tr>
</tbody>
</table>

## Tabular View

![tabular view](images/tabular_view.png)

A **tabular view** chart displays data per stream in a table format. A tabular view displays only one data point value per source. The value is a summary of all of available data points, as set in the Summarized By field, based on the configured time window.

The following options control the style of a Tabular View chart.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Columns</td>
<td>Configure options for table columns.
</td>
</tr>
<tr>
<td>Show Sources/Labels</td>
<td>Whether to display the sources and labels in the chart. A label is the metric name associated with the data or the query expression resulting from an aggregation.
</td>
</tr>
<tr>
<td>Show Raw Values</td>
<td>Whether to display the raw metric values or values using scientific notation (SI).
</td>
</tr>
<tr>
<td>Group by Sources</td>
<td>Whether to group the column results by source.  If you select <strong>Group by Source</strong>, the table lists the grouped sources in the first column and the values for each metric in separate columns.
</td>
</tr>
<tr>
<td>Sort Values Descending</td>
<td>Whether to sort the value column by descending value.
</td>
</tr>
</tbody>
</table>


## Single Stat View

![single stat](images/single_stat.png)

A **single stat** chart plots a single series on a chart and displays a summarized value for that series in large font on the chart. The font size and placement of the displayed value can be unique for each chart. A common use case is displaying instantaneous values of critical metrics on an overhead display.

In addition to general options, a Single Stat chart supports Sparkline options and Single Stat options:

#### Sparkline Options

Options controlling how the graph that summarizes the series displays.

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Sparkline</td>
<td>
Position of the sparkline. The options are:
<ul>
<li><strong>Bottom</strong> which means below the single stat.</li>
<li><strong>Background</strong> which places the sparkline in the background of the single stat</li> <li><strong>None</strong> for no sparkline on the chart.</li>
</ul>
</td>
</tr>
<tr>
<td>Line Color</td>
<td>The position of the sparkline. The options are <strong>Bottom</strong> which means below the single stat, <strong>Background</strong> which places the sparkline in the background of the single stat, or <strong>None</strong> for no sparkline on the chart.
</td>
</tr>
<tr>
<td>Value/Color Mapping</td>
<td>The color of the single stat based on its value. You add and remove color thresholds using the plus and minus buttons. Click each field to specify the colors and threshold values.
</td>
</tr>
<tr>
<td>Color Applies To</td>
<td>Whether the Value/Color Mapping applies to the color of the single stat text or the chart background.
</td>
</tr>
<tr>
<td>Fill Color</td>
<td>The fill color for the chart area below the sparkline.
</td>
</tr>
</tbody>
</table>

#### Value/Color Mapping Example:
In the following example the single stat is green if its value is less than 150, yellow when the value is between 150 and 200, and orange when the value is greater than 200:

![value_color](images/value_color.png)

#### Single Stat Options

A Single Stat chart supports the following options:

<table>
<tbody>
<thead>
<tr><th width="20%">Option</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>Display Value</td>
<td>The position of the sparkline. The options are
<ul>
<li><strong>Bottom</strong> which means below the single stat </li>
<li><strong>Background</strong> which places the sparkline in the background of the single stat</li>
<li><strong>None</strong> for no sparkline on the chart</li>
</ul>
</td>
</tr>
<tr>
<td>Horizontal Position</td>
<td>The horizontal pisition of the single stat. The options are Left, Middle, or Right of the chart.
</td>
</tr>
<tr>
<td>Text Font Size</td>
<td>The font size of the single stat.</td>
</tr>
<tr>
<td>Text Color</td>
<td>The color of the single stat.
</td>
</tr>
<tr>
<td>Prefix</td>
<td>A string to prefix the single stat.
</td>
</tr>
<tr>
<td>Value/Text Mapping</td>
<td>Strings to display instead of the metric value based on the value. You add and remove value thresholds using the plus and minus buttons. Click each field to specify the strings and threshold values.
</td>
</tr>
<tr>
<td>Decimal Precision</td>
<td>Number of digits to display after the decimal point in the summarized value.
</td>
</tr>
<tr>
<td>Value/Text Mapping</td>
<td>A string to append to the single stat.
</td>
</tr>
</tbody>
</table>

#### Value/Text Mapping Example

In the following example:

![value_text](images/value_text.png)

the string **lower** displays when the metrics value is below 150, **middle** displays when the value is between 150 and 200, and **upper** displays when the value is above 200.

## Markdown

![markdown](images/markdown.png)

A **Markdown** chart allows you to provide in-depth text descriptions of a dashboard and individual charts. In addition to Markdown formatted text, you can use links, images hosted outside Wavefront, and [dashboard variables](dashboards_variables.html). You can refer to the value of a dashboard variable with the query variable syntax **${var_name}** and the label of the variable using **%{var_name}**. Using a label instead of the variable value could be useful for list variables that might show the dropdown labels such as: Any, 1 Year, 3 Years which could map to opaque values such as -1, 1, 3.

The [Getting Started Dashboards](dashboards_getting_started.html) contain many examples of Markdown charts.

A Markdown chart only has General options and Markdown options.
