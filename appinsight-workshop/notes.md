Agenda
======

* Open Telemtry Architecture



Open Telemetry Architecture
===========================

https://opentelemetry.io/docs/

https://dzone.com/refcardz/getting-started-with-opentelemetry

Origin

Open Telemetry (2019) = OpenTracing (in 2016 became Cloud Native Computing Foundation (CNCF))
                      + OpenCensus (opensourced by Google in 2018)


Goals:
  * Monitoring    - collects and analyzes telemetry data and acts according to
                    the objectives defined (e.g., alerts, notifies)
  * Observability - is the ability to ask questions about the holistic state
                    of a system through the signals it generates.

Main Compoments: https://opentelemetry.io/docs/what-is-opentelemetry/#main-opentelemetry-components

Components
  * App
    - API (SDK)
    - Processing
    - Exporter
  * Collector
    - Receivers
    - Processors
    - Exporters
  * Vendor backends for storing and visualising data

API -> Processing -> Exporter ------> Receivers -> Processors -> Exporters -> telemtry db

Telemetry [signals](https://opentelemetry.io/docs/concepts/signals/)
  — logs
  - metrics
  - traces
  - events (specific type of logs)
  - metadata
  - baggage

Trace
=======

Traces in OpenTelemetry are defined implicitly by their Spans. In particular, a
Trace can be thought of as a directed acyclic graph (DAG) of Spans, where the
edges between Spans are defined as parent/child relationship.

* Components
  - Tracer Provider
  - Tracer
  - Trace Exporters
  - [Context Propagation (traceId, span id)](https://opentelemetry.io/docs/concepts/context-propagation/)
  - Spans
    + Name
    + Parent span ID (empty for root spans)
    + Start and End Timestamps
    + [Span Context](https://opentelemetry.io/docs/concepts/signals/traces/#span-context)
    + Attributes
      - [Semantic conventions](https://github.com/open-telemetry/semantic-conventions/tree/main)
    + Span Events (structured log message)
      - [event vs attribute](https://opentelemetry.io/docs/concepts/signals/traces/#when-to-use-span-events-versus-span-attributes)
    + [Span Links (link related spans)](https://opentelemetry.io/docs/concepts/signals/traces/#span-links)
    + Span Status
      - Unset
      - Error
      - Ok
    + [Span Kind](https://opentelemetry.io/docs/concepts/signals/traces/#span-kind)
      - Client
      - Server
      - Producer
      - Consumer

```
        [Span A]  <-- (the root span)
            |
     +------+------+
     |             |
 [Span B]      [Span C] <-- (Span C is a `child` of Span A)
     |             |
 [Span D]      +---+-------+
               |           |
           [Span E]    [Span F]
```


> hello span

```json
{
  "name": "hello",
  "context": {
    "trace_id": "0x5b8aa5a2d2c872e8321cf37308d69df2",
    "span_id": "0x051581bf3cb55c13"
  },
  "parent_id": null,
  "start_time": "2022-04-29T18:52:58.114201Z",
  "end_time": "2022-04-29T18:52:58.114687Z",
  "attributes": {
    "http.route": "some_route1"
  },
  "events": [
    {
      "name": "Guten Tag!",
      "timestamp": "2022-04-29T18:52:58.114561Z",
      "attributes": {
        "event_attributes": 1
      }
    }
  ]
}
```

> hello-greetings

```json
{
  "name": "hello-greetings",
  "context": {
    "trace_id": "0x5b8aa5a2d2c872e8321cf37308d69df2",
    "span_id": "0x5fb397be34d26b51"
  },
  "parent_id": "0x051581bf3cb55c13",
  "start_time": "2022-04-29T18:52:58.114304Z",
  "end_time": "2022-04-29T22:52:58.114561Z",
  "attributes": {
    "http.route": "some_route2"
  },
  "events": [
    {
      "name": "hey there!",
      "timestamp": "2022-04-29T18:52:58.114561Z",
      "attributes": {
        "event_attributes": 1
      }
    },
    {
      "name": "bye now!",
      "timestamp": "2022-04-29T18:52:58.114585Z",
      "attributes": {
        "event_attributes": 1
      }
    }
  ]
}
```

> hello-salutations

```json
{
  "name": "hello-salutations",
  "context": {
    "trace_id": "0x5b8aa5a2d2c872e8321cf37308d69df2",
    "span_id": "0x93564f51e1abe1c2"
  },
  "parent_id": "0x051581bf3cb55c13",
  "start_time": "2022-04-29T18:52:58.114492Z",
  "end_time": "2022-04-29T18:52:58.114631Z",
  "attributes": {
    "http.route": "some_route3"
  },
  "events": [
    {
      "name": "hey there!",
      "timestamp": "2022-04-29T18:52:58.114561Z",
      "attributes": {
        "event_attributes": 1
      }
    }
  ]
}
```

Metrics
=======

A measurement captured at runtime

https://opentelemetry.io/docs/concepts/signals/metrics

Components
  * Meter Provider
  * Meter
  * Metric Exporter

Metric Instruments
  * Counter - value that accumulates (don't goes down)
  * Asynchronous Counter (collected once by each export)
  * UpDownCounter (like counter but can goes down)
  * Asynchronous UpDownCounter
  * Gauge - measures a current value at the time it is read
  * Histogram -  a client-side aggregation of values
                (How many requests take fewer than 1s?)

Logs
====

A log is a timestamped text record, either structured (recommended) or
unstructured, with metadata.

In OpenTelemetry, any data that is not part of a distributed trace or a metric
is a log.

Events are a specific type of log

Baggage
======

TODO

Resources
=========

* [What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)
* [Semantic Conventions for HTTP Spans](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/http/http-spans.md)
* [Semantic Conventions for HTTP Metrics](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/http/http-metrics.md)
* [Semantic Conventions for Exceptions on Spans](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/exceptions/exceptions-spans.md)
* [Semantic Conventions for Database Client Calls](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/database/database-spans.md)
* [Semantic Conventions for Database Metrics](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/database/database-metrics.md)
* [Semantic Conventions for JVM Metrics](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/runtime/jvm-metrics.md)


