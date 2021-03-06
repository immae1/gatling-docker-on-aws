# Realtime monitoring loadtest
To be able to realtime monitor our loadtest, we have a seperate InfluxDB and Grafana stack that can be deployed on a AWS cluster.
The Gatling tests will then use graphite to report their status/results to this cluster and we can view this in the included Grafana dashboard.

## Run locally
Run: `docker-compose up`.

## Run on AWS
Use the `gatling-runner-aws` project for this.

### Gatling runners
The Gatling runners need to write their data to the InfluxDB.
Edit the `graphite` section in the `gatling.conf`. Edit the host to where InfluxDB lives, use either the ip address or the dns from the load balancer.
```
    graphite {
      light = false                            # only send the all* stats
      host = "<<insert influxdb host here>>"   # The host where the Carbon server is located
      port = 2003                              # The port to which the Carbon server listens to (2003 is default for plaintext, 2004 is default for pickle)
      protocol = "tcp"                         # The protocol used to send data to Carbon (currently supported : "tcp", "udp")
      rootPathPrefix = "gatling"               # The common prefix of all metrics sent to Graphite
      bufferSize = 8192                        # GraphiteDataWriter's internal data buffer size, in bytes
      writePeriod = 1                          # GraphiteDataWriter's write interval, in seconds
    }              
``` 

## Making changes
Both InfluxDB and Grafana are fully configured using Docker files.
Make the needed changes and use the `aws-cdk` project to rebuild the docker images.
