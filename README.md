# EBSI Node Monitoring Tool

## What's Included?

This repository includes the needed scripts for the automatic deployment of a monitoring system (Consisted of a telegraf, influxDb and a grafana - in form of docker containers) along with custom exporters for the exposure of monitoring data towards the monitoring system.

## How it works?

A **telegraf** agent is running inside a **machine** (e.g **Virtual Machine**) as a **docker container** and it collects **data** such as performance etc, and it transfer them to **influxDB**, where influxdb is connected with a **grafana** and they interchange data, in order for **grafana** to display data in different ways (such as Graphs, Pies etc)

## Brief Description for each element

### Telegraf

Is an agent collecting system data and be able to transfer them to other entities (in our case influxDB).

### InfluxDb

Is a time series database for storage and retrieval of time series data in fields such as operations monitoring, application metrics, etc.

### Grafana

Is a multi-platform analytics and interactive visualization web application.

## File `telegraf.conf`

- Create `telegraf.conf` file and write your tags and parameters, take as an example the `telegraf-example.conf` in this **repo**.

- You can also follow telegraf docs for more info about tags to be used in InfluxDb: https://docs.influxdata.com/telegraf/v1.21/administration/configuration/

- `token`, `organization` and `bucket` will be created and generated inside influxDB, please follow this guide to find out how.

## File `.env`

- Create `.env` file and set environment variables by using `.env-example` in this **repo**.

## Start the tool

Use `docker-compose up -d` in order to start the tool.

You need to install docker to start this tool:

- For windows: https://docs.docker.com/desktop/windows/install/
- For linux/ubuntu: https://docs.docker.com/engine/install/ubuntu/
- For mac: https://docs.docker.com/desktop/mac/install/

### Check if monitoring system is up and running

1. Navigate to [http://localhost:8086]() for InfluxDb.
2. Navigate to [http://localhost:9091]() for the Push Gateway.
3. Navigate to [http://localhost:3000]() for Grafana.

### InfluxDB Initial Setup

Among other fileds you will be asked to set `organization` name as well as `bucket` name, those two will be used in `telegraf.conf` file.

Next step is to generate a `token` in order for **telegraf** and **grafana** to be able to send or receive data to/from influxDB.

You can do that by:

1. **Navigate to**: Data -> Tokens -> + Generate -> Read/Write Token.
2. Select **bucket** for both **Read** and **Write**.
3. Copy token and paste it on telegraf.conf.
4. run the command `docker-compose restart` to restart the tool.

### Grafana Initial Setup

#### After loging in with initial credentials (username: admin, pass: admin) InfluxDB needs to be added as Data Source

- Navigate to **Settings** -> **Data Sources** -> **Add data source** -> **Influx DB**
- Choose Query Language (For this example use Flux)
- HTTP Url set Influxdb address (http://influxdb:8086) note: path is marked as influxdb because is running under a container in the same network as grafana and telegraf agent configured in docker-compose.yml
- Disable all Auth
- Under InfluxDB Details specify **Organization** as added on **InfluxDB** Initial Setup
- Under InfluxDB Details specify **Token** (to be **configured** on **InfluxDB** as described above)
- Under InfluxDB Details specify **Bucket** as added on **InfluxDB** Initial Setup
- Save & test

## General Architecture

```
 +--------------------------+                              +----------------------------+
 |                          |                              |                            |
 |         Grafana          |   monitoring data            |         InfluxDb           |
 |                          |<- - - - - - - - - - - - - - >|                            |
 +--------------------------+                              +----------------------------+
                                                                    ^
                                                                    |
                                        + - - - - - - - - - - - - - +
                                        | monitoring data
                                        |
                    +--------------------------------------------+
                    |                Host Machine                |
                    |        +---------------------------+       |
                    |        |                           |       |
                    |        |      Telegraf Agent       |       |
                    |        |                           |       |
                    |        +---------------------------+       |
                    |            |        ^                      |
                    |            |        | system data          |
                    |            |        |                      |
                    |            +--------+                      |
                    +--------------------------------------------+
```

## Acknowledgement

- This project is funded by European Blockchain Services Infrastructure (EBSI).
- This repository is based on [https://github.com/evnsio/prom-stack](https://github.com/evnsio/prom-stack).

## Contributors

- Elias Iosif (iosife@unic.ac.cy)
- Klitos Christodoulou (christodoulou.kl@unic.ac.cy)
- Christos Michael (michael.christos@unic.ac.cy)
