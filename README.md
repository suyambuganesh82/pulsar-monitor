# Operation Monitoring for Pulsar

This is a ops monitoring tool to
- [x] monitor Pulsar admin REST API endpoint
- [x] measure a single message latency from producing to consuming
- [x] ability to produce a list of messages with user specified payload size and the list size
- [x] measure average latency over a list of messages
- [x] report out of order delivery
- [x] measure a single message latency over the websocket interface
- [x] measure message latency generated by Pulsar function
- [x] measure Pulsar cluster availability
- [ ] Pulsar function trigger over HTTP interface
- [x] incident alert with OpsGenie
- [x] tracking analytics and usage
- [x] dead man's snitch heartbeat monitor with OpsGenie
- [x] alert on Slack

This is a data driven tool. The configuraion is a yaml or json file. Here is a [template](../config/runtime_template.json).
The configuration json file can be specified in the overwrite order of 
- an environment variable `PULSAR_OPS_MONITOR_CFG`
- an command line argument `./pulsar-monitor -config /path/to/pulsar_ops_monitor_config.yml`
- A default path to `../config/runtime.yml`

## Docker compose
`./config/runtime.yml` or `./config/runtime.json` must have a Pulsar jwt and configured properly.

``` bash
$ docker-compose up
```

## Docker example
The runtime.json file must be mounted as /app/runtime.json

This runs a multi stage build that produces a 18MB docker image.
```
$ sudo docker build -t pulsar-monitor .
```

Run docker container with Pulsar CA certificate and expose Prometheus metrics for collection.

``` bash
$ sudo docker run -d -it -v $HOME/go/src/github.com/kafkaesque-io/pulsar-monitor/config/runtime.yml:/config/runtime.yml -v /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem:/etc/ssl/certs/ca-bundle.crt -p 8080:8080 --name=pulsar-monitor pulsar-monitor
```

## Prometheus
This program exposes a Prometheus `\metrics` endpoint to allow measured Pulsar latency to be collected by Prometheus.

## Helm chart
Here is Pulsar Monitor's [helm chart](https://github.com/kafkaesque-io/pulsar-helm-chart/tree/master/helm-chart-sources/pulsar-monitor)
