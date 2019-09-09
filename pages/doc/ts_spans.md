---
title: spans() Function
keywords: data, distributed tracing
tags: [tracing]
sidebar: doc_sidebar
permalink: spans_function.html
summary: Learn how to write spans() queries.
---

## Summary

```
spans("<fullOperationName>" [,|and|or <filterName>="<filterValue>"] ...)

spans(<filterName>="<filterValue>" [,|and|or <filterName>="<filterValue>"] ...)
```
Returns the spans that match the specified operation and filters. You use `spans()` as a parameter of the [`traces()`](traces_function.html) function, typically after combining `spans()` with one or more [spans filtering functions](#spans-filtering-functions).

### Parameters

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="20%">Parameter</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>fullOperationName</td>
<td markdown="span">
Full name of the operation that each matching span must represent. For example, specify `"beachshirts.delivery.dispatch"` to match the spans that represent calls to an operation named `dispatch` in the `delivery` service of the `beachshirts` application. <br> The general format of a `fullOperationName` is `<application>.<service>.<operationName>`, where each component consists of one or more period-delimited components. Replace `operationName` or `serviceName` with an asterisk `*` to match spans for any operation in any service.
</td>
</tr>
<tr>
<td>filterName</td>
<td markdown="span"> A [span filter](#filters) that each matching span must match. Span filters let you limit which spans to return traces for. You can optionally specify multiple span filters combined with Boolean operators (`and`, `and not`, `or`).</td></tr>
<tr>
<td>filterValue</td>
<td markdown="span">Value accepted by a specified `filterName`.</td></tr>
</tbody>
</table>


## Description
The `spans()` function finds spans that match the description you specify. You describe the spans of interest by providing an operation name, one or more filters, or a combination of these, to specify the characteristics that the spans must match.

You use `spans()` by including it as a parameter of the [`traces()`](traces_function.html) function. Doing so allows you to filter traces according to the duration of a particular span. For example, the following query returns a trace only if it has at least one span that both represents a `makeShirts` operation and lasts longer than 11 seconds: 

```
limit(100, traces(highpass(11s, spans("beachshirts.styling.makeShirts"))))
```

This is different from a query that wraps `highpass` around `traces()`. For example, the following query returns a trace only if the entire end-to-end trace is longer than 11 seconds.

```
limit(100, highpass(11s, traces(spans("beachshirts.styling.makeShirts"))))
```
 

**Note:** the following two queries are equivalent:

```
limit(100, traces("beachshirts.styling.makeShirts"))          //  spans() is implicit
limit(100, traces(spans("beachshirts.styling.makeShirts")))   //  spans() is explicit
```


## Examples

Assume your team has instrumented an application called `beachshirts` for tracing. This application has a service called `styling` that executes an operation called `makeShirts`. The application is deployed on several hosts in each of several clusters.

**Note:** To keep query execution manageable, these examples use `traces()` with the `limit()` function.

To display the traces that include long spans for calls to `makeShirt`:
- `limit(100, traces(highpass(11s, spans("beachshirts.styling.makeShirts"))))`

To display the traces that include short spans for any operation in the `styling` service:
- `limit(100, traces(lowpass(3ms, spans("beachshirts.styling.*"))))`

To display the traces that include spans for any operation in the `beachshirts` application executing on either of two specified hosts:
- `limit(100, traces(spans("beachshirts.*.*" and source=prod-app1 or source=prod-app10)))`


<a name="filters"></a>

## Span Filters

Span filters allow you to limit which spans to return traces for. Span filters are key/value pairs associated with spans. Developers define the available span filters (also called *application tags*) as part of instrumenting the application code for tracing. Even if you did not instrument the code yourself, you can use autocompletion in the Query Editor to discover the available span filters.

The general format for a span filter is `<filterName>="filterValue"`.


<table style="width: 100%;">
<colgroup>
<col width="13%" />
<col width="53%" />
<col width="33%" />
</colgroup>
<thead>
<tr><th>Filter</th><th>Description</th><th>Example</th></tr>
</thead>
<tbody>
<tr>
<td>application</td>
<td markdown="span">Name that identifies an instrumented application, for example, `beachshirts`. Matches the spans that represent operations that are  performed by the specified application.</td>
<td><code>spans(application="beachshirts")</code></td>
</tr>
<tr>
<td>service</td>
<td markdown="span">Name that identifies an instrumented microservice, for example, `delivery`. Matches the spans that represent operations that are  performed by the specified microservice.</td>
<td><code>spans(service="delivery")</code></td>
</tr>
<tr>
<td>cluster</td>
<td markdown="span">Name of a group of related hosts that serves as a cluster or region in which an instrumented application runs, for example, `us-west-2`. Matches the spans that represent operations that are executed on the specified cluster.</td>
<td><code>spans(cluster="us-west-2")</code></td>
</tr>
<tr>
<td>shard</td>
<td markdown="span">Name of a subgroup of hosts within a cluster, for example, `secondary`. Matches the spans that represent operations that are executed on the specified shard.</td>
<td><code>spans(shard="secondary")</code></td>
</tr>
<tr>
<td>source</td>
<td markdown="span">Host or container that an instrumented application is running on. Matches the spans that represent operations that are executed on the specified source.</td>
<td><code>spans(source="prod-app1")</code></td>
</tr>
<tr>
<td>traceId</td>
<td markdown="span">Unique identifier for a trace. Matches all spans that belong to the specified trace.</td>
<td><code>spans(traceId="5b309723-fb83-4c9c-afb6-f0dc4b2bff13")</code></td>
</tr>
<tr>
<td>&lt;spanTag&gt;</td>
<td>Custom tag defined in your application code. Matches spans that are associated with the specified custom span tag.</td>
<td><code>spans(environment="production")</code></td>
</tr>

</tbody>
</table>

## Spans Filtering Functions

You can use spans filtering functions to provide additional levels of filtering for the set of spans that are matched by the `spans()` function. 

Each spans filtering function has a **spansExpression** parameter, which can be a `spans()` function or one of the other spans filtering functions. 


<table style="width: 100%;">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
<thead>
<tr><th>Spans Filtering Function</th><th>Description and Example</th></tr>
</thead>
<tbody>

<tr>
<td>highpass(<strong>&lt;spanDuration&gt;</strong>, <strong>&lt;spansExpression&gt;</strong>)</td>
<td markdown="span">Limits the set of spans that are matched by **spansExpression** to include only spans that are longer than **spanDuration**.  Specify **spanDuration** as an integer number of milliseconds, seconds, minutes, hours, days or weeks (1ms, 1s, 1m, 1h, 1d, 1w). <br><br>
**Example:** Return the qualifying spans that are longer than 3 seconds: <br>
`highpass(3s, spans("makeShirts", application="beachshirts"))`
</td>
</tr>
<tr>
<td>lowpass(<strong>&lt;spanDuration&gt;</strong>, <strong>&lt;spansExpression&gt;</strong>)</td>
<td markdown="span">Limits the set of spans that are matched by **spansExpression** to include only spans that are shorter than **spanDuration**.  Specify **spanDuration** as an integer number of milliseconds, seconds, minutes, hours, days or weeks (1ms, 1s, 1m, 1h, 1d, 1w). <br><br>
**Example:** Return the qualifying spans that are shorter than 10 milliseconds: <br>
`lowpass(10ms, spans("makeShirts", application="beachshirts"))`
</td>
</tr>

</tbody>
</table>

<!--- May include eventually. Currently traces(limit(spans())) is equivalent to limit(traces(spans())), but works less well.
<tr>
<td>limit(<strong>&lt;numberOfSpans&gt;</strong>, <strong>&lt;spansExpression&gt;</strong>)</td>
<td markdown="span">Limits the set of spans that are matched by **spansExpression** to the specified **numberOfSpans**.  <br>
**Note:** Because the ordering of matched spans is unpredictable, you cannot use `limit()` to page through a set of results to obtain the traces that contain the next group of spans. 
</td>
</tr>
--->
<!--- May include eventually. Currently internal only, with no compelling use case for users.

<tr>
<td>rootsOnly(<strong>&lt;spansExpression&gt;</strong>)</td>
<td markdown="span">Limits the set of spans that are matched by **spansExpression** to include only spans that are the root spans of a trace, i.e., spans without any ancestor. <br><br>
**Example:**  <br>
`rootsOnly(spans(traceId="707261fc-d412-4926-b6f6-c2ca1053c914"))`
</td>
</tr>
<tr>
<td>childrenOnly(<strong>&lt;spansExpression&gt;</strong>)</td>
<td markdown="span">Limits the set of spans that are matched by **spansExpression** to include only spans that are the child spans of a trace, i.e., no root spans. <br> <br>
**Example:**  <br>
`childrenOnly(spans(traceId="707261fc-d412-4926-b6f6-c2ca1053c914"))`
</td>
</tr>
--->