# EBSI Node Monitoring Tool :link:

## What's Included?

This repository includes: 
* the needed scripts for the automatic deployment of a monitoring system (Consisted of a telegraf, influxDb and a grafana - in form of :whale: docker containers) 
* along with custom exporters for the exposure of monitoring data towards the monitoring system.

## How it works? :gear:

A **telegraf** agent is running inside a **machine** (e.g **Virtual Machine**) as a **:whale: docker container** and it collects **data** such as performance etc, and it transfer them to **influxDB**, where influxdb is connected with a **grafana** and they interchange information, in order for **grafana** to display machine monitoring data in different ways (such as Graphs, Pies, Timeseries graphs etc)

## Brief Description for each element

### Telegraf

Is an agent, collecting system data and be able to transfer them to other entities (in our case influxDB).

### InfluxDb :file_cabinet:

Is a time series database for storage and retrieval of time series data in fields such as operations monitoring, application metrics, etc.

### Grafana :chart_with_upwards_trend:

Is a multi-platform analytics and interactive visualization web application.

## File `telegraf.conf` :spiral_notepad:

- Create `telegraf.conf` file and write your tags and parameters, take as an example the `telegraf-example.conf` in this **repo**.

- You can also follow telegraf docs for more info about tags to be used in InfluxDb: https://docs.influxdata.com/telegraf/v1.21/administration/configuration/

- `token`, `organization` and `bucket` will be created and generated inside influxDB, please follow this guide to find out how.

## File `.env` :spiral_notepad:

- Create `.env` file and set environment variables by using `.env-example` in this **repo**.

## Start the tool :rocket:

Use `docker-compose up -d` in order to start the tool.

You need to have :whale: docker installed to start this tool :whale: :

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
- HTTP Url set Influxdb address (e.g: http://influxdb:8086) note for this example: path is marked as influxdb because is running under a container in the same network as grafana and telegraf agent configured in docker-compose.yml, this can be changed based on the url where influxDB is running
- Disable all Auth
- Under InfluxDB Details specify **Organization** as added on **InfluxDB** Initial Setup
- Under InfluxDB Details specify **Token** (to be **configured** on **InfluxDB** as described above)
- Under InfluxDB Details specify **Bucket** as added on **InfluxDB** Initial Setup
- Save & test

## General Architecture

### Note/Reminder :warning: : 
* Grafana and InfluxDb can be running on completely different machine than telegraf agent
```
 +--------------------------+                              +----------------------------+
 |                          |                              |                            |
 |         Grafana          |   monitoring data            |         InfluxDb           |
 |   (data visualization)   |<- - - - - - - - - - - - - - >|                            |
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

## Running **telegraf agent** on different machine than Grafana and InfluxDB
- Place `telegraf.conf` file in the machine you want to monitor its data. 
- Add the influxDB `token` inside `telegraf.conf` for authentication. 
- Add influxDb `url` inside `telegraf.cong`.
- Create a `docker-compose.yml` on the machine you want telegraf agent to run file and include same info as in `docker-compose.yml` in this repo.
- Run `docker-compose up -d` to run the telegraf agent.


## Acknowledgement

- This project is funded by European Blockchain Services Infrastructure (EBSI).
- This repository is based on [https://github.com/evnsio/prom-stack](https://github.com/evnsio/prom-stack).

## Contributors

- Klitos Christodoulou (christodoulou.kl@unic.ac.cy)
- Elias Iosif (iosife@unic.ac.cy)
- Christos Michael (michael.christos@unic.ac.cy)
