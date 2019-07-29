# Datastack for storing, visualizing and analysing data

composed by either an Elastic Stack, or influxDB based approach.

Supported by the [datastack-adapter](https://github.com/iot-salzburg/dtz_datastack/tree/master/datastack-adapter)
to stream data easily from 
[panta-rhei messaging system](https://github.com/iot-salzburg/dtz_datastack/tree/master/elasticStack) into the stack.

InfluxDB is very nice for storing metric data, as we deal with.
However, the open source variant doesn't support scaling on a cluster.
Therefore, the Elastic Stack is our choice.


The `vm.max_map_count` setting should be set permanently 
in `/etc/sysctl.conf`:
```bash
grep vm.max_map_count /etc/sysctl.conf
# -> vm.max_map_count=262144

# or set it temporarily 
sysctl -w vm.max_map_count=262144
```


## How to start:

* Set up the [Elastic Stack](https://github.com/iot-salzburg/dtz_datastack/tree/master/elasticStack)

* Start streaming with [datastack-adapter](https://github.com/iot-salzburg/dtz_datastack/tree/master/datastack-adapter)


*note that the influxDB stack is depricated*

