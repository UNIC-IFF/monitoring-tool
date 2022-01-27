# Monitoring Tool System

## What's Included?

This repository includes the needed scripts for the automatic deployment of a monitoring system (Consisted of a telegraf, influxDb and a grafana - in form of docker containers) along with custom exporters for the exposure of monitoring data towards the monitoring system.

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

1. Navigate to [http://<your-address>:8086] for InfluxDb.
2. Navigate to [http://<your-address>:9091] for the Push Gateway.
3. Navigate to [http://<your-address>:3000] for Grafana.

### InfluxDB First Setup

Among other fileds you will be asked to set `organization` name as well as `bucket` name, those two will be used in `telegraf.conf` file.

Next step is to generate a `token` in order for telegraf to be able to send data to influxDB.

You can do that by:

1. **Navigate to**: Data -> Tokens -> + Generate -> Read/Write Token.
2. Select **bucket** for both **Read** and **Write**.
3. Copy token and paste it on telegraf.conf.
4. run the command `docker-compose restart` to restart the tool.

## General Architecture

```
 +--------------------------+            connection                 +----------------------------+
 |                          |< - - - - - - - - - - - - - - - - - - -|                            |
 |         Grafana          |            data                       |         InfluxDb           |
 |                          |- - - - - - - - - - - - - - - - - - - >|                            |
 +--------------------------+                                       +----------------------------+
                                                                                  ^
                                                                                  |
                                                      + - - - - - - - - - - - - - +
                                                      | data
                                       +---------------------------+
                                       |                           |
                                       |          Telegraf         |
                                       |                           |
                                       +---------------------------+
                                                      ^
                                                      | system data
                                                      |
                                       +---------------------------+
                                       |                           |
                                       |           System          |
                                       |                           |
                                       +---------------------------+
```
