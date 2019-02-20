---
title: Visualizing Traces, Spans, and Metrics
keywords: data, distributed tracing
tags: [tracing]
sidebar: doc_sidebar
permalink: tracing_ui_overview.html
summary: Explore your apps and services from a browser.
---

The Wavefront tracing UI enables exploration of your apps and services. Once your application is instrumented for tracing, you can examine traces, spans, and RED metrics, moving easily from one browser page to the next. This doc page gives an overview of the different UI pages. 

<!---
Exploring Traces, Spans, and Metrics gives more detail.
--->

## Navigate to the Tracing UI

To navigate to the tracing UI:
1. Log in to your Wavefront cluster.
2. From the task bar:

    - Select **Applications > Inventory** to view all application services and drill down from there. 
    - Select **Applications > Traces** to start by querying for traces and drill down from there. 

    ![tracing menu](images/tracing_menu.png)

## Get Started: Application Services

The Application Services page is your entry point for examining your application. 

![app services](images/tracing_app_services.png)

On the Application Services page, you can:
* Select an application from the **Jump To** pulldown.
* Search for a service.
* Click inside a service box to go to the dashboard for that service.


## Examine Service Metrics and Drill Down

When you select a service, you can examine the corresponding metrics and potential hot spots in detail, and then drill down to the **Traces** page. 

![examine services](images/tracing_services.png)

**Note:** The charts on this dashboard are read only.

On the page for a particular service, you can:
* Select the time, timezone, and other chart attributes in the task bar to customize the chart display. These selections are the same as for other dashboards.
* Use the **Jump To** pulldown to select a dashboard section:
  - Select Overall to examine the RED metrics that are derived from all of the spans for the service.
  - Select an individual component to examine metrics for just that component of the service. A component could be an instrumented framework or the runtime system.
* Filter the metrics based on the cluster, shard, or source.
* Select **Detailed View** or **Summarized View** to change the level of detail for charts.
* Examine the TopK charts to find out which operations are potential hot spots. The bars represent operations that execute in this service component.
* Navigate to the **Traces** page:
  - Select **See All Traces** to display all traces that include a span from this service component. 
  - Click one of the bars in a TopK chart to display only traces that include a span for the selected operation.


## Explore Traces

On the **Traces** page, you can view the traces that include spans for particular operations, and you can drill down to get trace details. 

The way you access the **Traces** page determines its initial content: 

* Navigate from the Service page to automatically display traces for operations you selected.
* Select **Applications > Traces** from the task bar for an empty page that you populate by [querying](trace_data_query.html).

![explore trace page](images/tracing_traces_page.png)

On the **Traces** page, you can:
* Apply filters and selectively narrow down the scope of the trace query.
* Use the query editor to limit the scope even further (advanced users).
* Examine the scatter plot to clearly see the duration of each trace over time, and to see traces with errors (red circles).
* Sort traces with matching spans using different criteria.
* Click any trace to display a pop-up that shows trace details.



## Examine Trace Details

On a trace details pop-up, you can examine the spans that belong to a single trace. Some of these spans may represent operations executed by other services.

![trace pop-up](images/trace_popup_simple.png)

On a trace details pop-up, you can:
* Move the edges of the overview panel to zoom in on a narrower time window. 
* Examine the call hierarchy.
* Click any span to expand and see more detail.
* Explore the trace’s [critical path](#a-closer-look-at-critical-paths). This is an end-to-end combination of spans that are the most blocking.


## Drill Down Into Traces and View Related Metrics

You can click any span in a trace details pop-up to view details about that span. If that span came from another service, you can navigate to the dashboard for that service.

![trace pop-up expanded](images/trace_popup_expanded.png)

Span details include:
* Application tags. These are the application, service, cluster, and shard, as selected on the Services page.
* Other tags including the trace ID.
* A clickable link to the corresponding dashboard that lets you examine the metrics associated with the call.


## A Closer Look at Critical Paths

A [trace details pop-up](#examine-trace-details) uses an orange line to show the critical path through a trace. You can think of the critical path as an end-to-end combination of spans that are the most blocking. This is the combination of spans that determines the minimum length of the trace they belong to.

We use the following rules to determine which spans to include in a critical path (in order of applicability):
1. Ignore asynchronous spans (spans tagged with `followFrom`). 
2. Ignore spans that end before their parent starts.
3. If a child span continues after its parent, ignore that continuation period.
4. Choose longer spans over shorter siblings.
5. Choose later spans over earlier spans.
6. Choose child spans instead of their parent spans.