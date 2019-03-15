---
title: Alert Recipes
keywords: alerts
tags: [alerts, best practice]
sidebar: doc_sidebar
permalink: alerts_recipes.html
summary: Queries for common alert scenarios
---

The Wavefront Customer Success team has found that customers use certain alerts frequently. For example, customers want to alert on point rate drops or on between specific times.

This page gives some recipes. You can generate several of these recipes interactively using the [Query Wizard](query_language_query_wizard.html).

**Note:** For improved legibility, we've included line breaks in some of the query examples.

## Alert on Point Rate Drop

Define an alert that compares the number of data points reported in the last time window to the number of data points reported in the window before that. If the last time window is X percent less, return 1 to alert.

For example, here's the query for an alert that fires if the number of processes for `app-5` drops by 5% in a 30 minute time window:

```
mcount(30m, (ts(~sample.process.num, source="app-5"))) <
mcount(30m,lag(30m,(ts(~sample.process.num, source="app-5")))) * 5/100
```

## Alert on Missing Data

Define an alert that compares the number of data points reported in the last time window to the number of data points reported in the window before that. An alert like that is useful for example, if you want to be notified when a service stops sending points.

For example, here's the query for an alert that fires if `app-2` sent bytes more than 30 seconds ago, but stopped sending bytes in the last 30 seconds.

```
mcount(30s, (ts(~sample.network.bytes.sent, source=app-2))) = 0 and mcount(30s,lag(30s,(ts(~sample.network.bytes.sent, source=app-2)))) != 0
```

## Alert on Exceeding a Threshold

Define an alert that fires when data exceeds a threshold.

For example, here's the query for an alert that fires if the number of sample processes for `app-5` is greater than 100.

`(ts(~sample.process.num, source="app-5")) > 100`

You can use a query like this with a classic alert or with threshold alert. A [threshold alert](alerts.html#creating-a-threshold-alert) allows you to set multiple thresholds associated with different severities.

## Alert Only Between Specific Times

Define an alert that fires only between specified times.

For example, here's the query for an alert that fires only on Thursdays between 8 and 5 PST.

~~~
(ts(~sample.process.num, source="app-5")) > 100
and between(hour("US/Pacific"),8,5)
and between(weekday("US/Pacific"),4,4)
~~~

The following diagram shows the corresponding query in a chart.

![alert times](images/alert_recipe_time.png)

## Alert When There Are More than X Points in a Time Window

Define an alert when there are more than a specified number of points in a specified time window.

For example, here's the query for an alert that fires if the number of sample processes for `app-5` is more than 100 in a 30 minute time window.

`mcount(30m, (ts(~sample.process.num, source="app-5"))) > 100`