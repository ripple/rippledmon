# rippledmon
Keep an eye on your rippled server with this drop-in monitoring solution.


# Background
[rippled](https://github.com/ripple/rippled) is equipped to export metrics via statsd when "insights" are enabled. This monitoring tool consumes and presents those metrics via statsd-graphite and Grafana docker containers. New versions of rippled will export more metrics and new dashboards will be added to this repository. 

![Dashboard](dashboard.png)


# Prerequisites 
Docker and docker-compose must be installed on your system. For more details on this see [here](https://docs.docker.com/install/) for docker and [here](https://docs.docker.com/compose/install/) for docker-compose.


# Installation

1. Clone this repo and start the docker containers with docker-compose

```
$ git clone https://github.com/rippled/rippledmon.git
$ cd rippledmon
$ docker-compose up
```
Note: you can use the -d flag in the docker-compose command to run it in the background.

2. Add the [insight] stanza to your rippled.cfg file, and restart your rippled server.

```
[insight]
server=statsd
address=127.0.0.1:8125
prefix=my_rippled
```

This will enable insights on your rippled server and send metrics via UDP to a statsd server listening on port 8125 of your localhost. The metrics will be prefixed with 'my_rippled'. **NOTE: If you are using this tool to monitor a production grade validator, you should run it on a separate machine and change the address above** 

3. Login to Grafana and view dashboards

```
In your browser go to http://localhost:3000 
```
![Login](Login.png)

The default username and password are 'admin'. Dashboard 1 is an example displaying a few of the metrics exported from rippled. It can take a few minutes for these metrics to populate so be patient. 

4. Make new dashboards

You can make new dashboards with the specific metrics that you want by selecting the plus icon and choosing "Add Query" in the New Panel. 

![Add Dashboard](New_Dashboard.png)

Choose the appropriate tags to query the metric you want and adjust the graphing parameters.

![New Query](New_Query.png)

For more information on how to use Grafana see [here](https://grafana.com/docs/grafana/latest/guides/getting_started/).

# Metrics

Metrics will be added to rippled over time and new dashboards will be added to this repo. These are the metrics that are currently visible: 

| Timer Metric Tags |
|----------|
| jobq.untrustedProposal | 
| jobq.trustedProposal |
| jobq.untrustedValidation |
| jobq.trustedValidation |
| jobq.heartbeat | 
| jobq.ledgerRequest |  
| jobq.acceptLedger |
| jobq.advanceLedger |
| jobq.clientCommand |
| jobq.ledgerData |
| jobq.transaction | 
| jobq.fetchTxnData | 
| jobq.ledgerData |
| jobq.writeObjects |
| jobq.batch |
| jobq.sweep |
| jobq.clientCommand |
| jobq.clusterReport |

Timers with the jobq tag report the amount of time it took to execute the job in ms. You will also see the metrics above with an "_q" suffix. The corresponding "_q" metrics report the amount of time it took to dequeue the job. 

| Counter Metric Tags |Description|
|---------------------|:---------:|
| ledger_fetches | Number of ledger fetches in the last collection interval|
| jobq.job_count | Number of jobs in the job queue |
| full_below.size | Size of the [FullBelowCache](https://github.com/ripple/rippled/blob/develop/src/ripple/shamap/FullBelowCache.h)
| full_below.hit_rate | Hit Rate of the [FullBelowCache](https://github.com/ripple/rippled/blob/develop/src/ripple/shamap/FullBelowCache.h) 

## Acknowledgements:
Inspired by [lndmon](https://github.com/lightninglabs/lndmon)  
Uses [graphite-project](https://github.com/graphite-project/docker-graphite-statsd) and [Grafana](https://github.com/grafana/grafana)




    












