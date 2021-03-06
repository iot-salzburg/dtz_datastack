# InfluxDB and Grafana

## Setup

Update the configurations in the environment file `InfluxDB_Grafana/.env`.
**Change the password immediately and never commit this file if the service is available from other 
nodes!** The `.env`-file could look like this:

```.env
INFLUXDB_DB=at.srfg.iot.dtz
INFLUXDB_ADMIN_ENABLED=true
INFLUXDB_ADMIN_USER=admin
INFLUXDB_ADMIN_PASSWORD=admin
INFLUXDB_USER=iotdev
INFLUXDB_USER_PASSWORD=iotdev
```

To start InfluxDB and also Grafana, run`
```bash
cd InfluxDB_Grafana
docker-compose up --build -d
``` 

To validate, if InfluxDB is running correctly, curl the service 
using:

```bash
curl -sl -I http://localhost:8086/ping
# Expected result, note the status code 204
HTTP/1.1 204 No Content
Content-Type: application/json
Request-Id: 2f7091fb-9daa-11ea-8002-0242ac110002
X-Influxdb-Build: OSS
X-Influxdb-Version: 1.8.0
X-Request-Id: 2f7091fb-9daa-11ea-8002-0242ac110002
Date: Sun, 24 May 2020 10:34:45 GMT
```

Grafana should run with access to InfluxDB on [localhost:3000](http://localhost:3000).


To investigate the services or 
to stop them, run:

```bash
docker-compose ps
docker-compose logs -f
docker-compose stop
```

## First steps in InfluxDB

InfluxDB provides a RestAPI that can be executed via `curl`

```bash
curl -XPOST 'http://localhost:8086/query' --data-urlencode 'q=CREATE DATABASE "mydb"'
curl -XPOST 'http://localhost:8086/query?db=mydb' --data-urlencode 'q=SELECT * INTO "newmeas" FROM "mymeas"'
curl -G 'http://localhost:8086/query?db=mydb&pretty=true' --data-urlencode 'q=SELECT * FROM "mymeas"'

# or for this database:
curl -G 'http://localhost:8086/query?db=at.srfg.iot.dtz' --data-urlencode 'q=SELECT * FROM "at.srfg.iot.dtz"'
```
More API interface examples can be found [here](https://docs.influxdata.com/influxdb/v1.8/tools/api/).

It is important, that the attributes `time` and all `tags` are
the primary key and must be unique. Please avoid a high number of different values
for a InfluxDB tag, e.g., a secondary timestamp must not be set as a tag.



## First steps in Grafana

Grafana is started with InfluxDB and is reachable on
[localhost:3000](http://localhost:3000).

To retrieve data from the InfluxDB, add a new data source by 
clicking on `Configuration -> Data Sources -> Add data source`.
Then fill the fields as shown in this screenshot:

![source](grafana_source.png)   

The password is set in the environment file `InfluxDB_Grafana/.env`.
As you can see in the green box, the test was successful!

Afterwards, a dashboard can be created that retrieves data from
InfluxDB. Therefore, click on `+ -> Dashboard` and then build a
dashboard from scratch. However, it is also possible to import
a dashboard that was previously exported. 
Here is an example of how to access the data from InfluxDB.
![grafana_Influx_connection](grafana_Influx_connection.png)

