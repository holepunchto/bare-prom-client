Forked from https://github.com/siimon/prom-client to make compatible with bare. All pre-fork intellectual property belongs to the original authors

# Prometheus client for node.js [![Actions Status](https://github.com/siimon/prom-client/workflows/Node.js%20CI/badge.svg?branch=master)](https://github.com/siimon/prom-client/actions)

A prometheus client for Node.js that supports histogram, summaries, gauges and
counters.

## Usage

See example folder for a sample usage. The library does not bundle any web
framework. To expose the metrics, respond to Prometheus's scrape requests with
the result of `await registry.metrics()`.

### Usage with Node.js's `cluster` module

Node.js's `cluster` module spawns multiple processes and hands off socket
connections to those workers. Returning metrics from a worker's local registry
will only reveal that individual worker's metrics, which is generally
undesirable. To solve this, you can aggregate all of the workers' metrics in the
master process. See `example/cluster.js` for an example.

Default metrics use sensible aggregation methods. (Note, however, that the event
loop lag mean and percentiles are averaged, which is not perfectly accurate.)
Custom metrics are summed across workers by default. To use a different
aggregation method, set the `aggregator` property in the metric config to one of
'sum', 'first', 'min', 'max', 'average' or 'omit'. (See `lib/metrics/version.js`
for an example.)

If you need to expose metrics about an individual worker, you can include a
value that is unique to the worker (such as the worker ID or process ID) in a
label. (See `example/server.js` for an example using
`worker_${cluster.worker.id}` as a label value.)

Metrics are aggregated from the global registry by default. To use a different
registry, call
`client.AggregatorRegistry.setRegistries(registryOrArrayOfRegistries)` from the
worker processes.

## API

See the [`bare-prom-client` reference][reference].

[reference]: https://docs.pears.com/reference/bare/modules/bare-prom-client
