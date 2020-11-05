# Docker Image with InfluxDB and Grafana

[![Docker Pulls](https://img.shields.io/docker/pulls/dtushar/docker-influxdb-grafana.svg)](https://dockerhub.com/dtushar/docker-influxdb-grafana) [![license](https://img.shields.io/github/license/tushardhadiwal/docker-influxdb-grafana.svg)](https://dockerhub.com/dtushar/docker-influxdb-grafana)

![Grafana][grafana-version] ![Influx][influx-version] ![Chronograf][chronograf-version]


This is a Docker image based on the awesome [Docker Image with Telegraf (StatsD), InfluxDB and Grafana](https://github.com/samuelebistoletti/docker-statsd-influxdb-grafana) from [Samuele Bistoletti](https://github.com/samuelebistoletti).

The main point of difference with this image is:

* Persistence is supported via mounting volumes to a Docker container
* Grafana will store its data in SQLite files instead of a MySQL table on the container, so MySQL is not installed
* Telegraf (StatsD) is not included in this container
* Grafana Version has been updated to 7.1.5 , which supports Flux for InfluxDB.
* Flux endpoint is enabled in influxdb config to support direct access in grafana.
* Chronograf has a explore tab which supports building queries in flux language out of box.

| Description  | Value   |
|--------------|---------|
| InfluxDB     | 1.8.3 |
| ChronoGraf   | 1.8.8  |
| Grafana      | 7.3.1   |

## Quick Start

To start the container with persistence you can use the following:

```sh
docker run -d \
  --name docker-influxdb-grafana \
  -p 3003:3003 \
  -p 3004:8083 \
  -p 8086:8086 \
  -v /path/for/influxdb:/var/lib/influxdb \
  -v /path/for/grafana:/var/lib/grafana \
  dtushar/docker-influxdb-grafana:latest
```

To stop the container launch:

```sh
docker stop docker-influxdb-grafana
```

To start the container again launch:

```sh
docker start docker-influxdb-grafana
```

## Mapped Ports

```
Host		Container		Service

3003		3003			grafana
3004		8083			chronograf
8086		8086			influxdb
```
## SSH

```sh
docker exec -it <CONTAINER_ID> bash
```

## Grafana

Open <http://localhost:3003>

```
Username: root
Password: root
```

### Add data source on Grafana

1. Using the wizard click on `Add data source`
2. Choose a `name` for the source and flag it as `Default`
3. Choose `InfluxDB` as `type`
4. Choose `direct` as `access`
5. Fill remaining fields as follows and click on `Add` without altering other fields

Basic auth and credentials must be left unflagged. Proxy is not required.

Now you are ready to add your first dashboard and launch some queries on a database.

## InfluxDB

### Web Interface (Chronograf)

Open <http://localhost:3004>

```
Username: root
Password: root
Port: 8086
```

### InfluxDB Shell (CLI)

1. Establish a ssh connection with the container
2. Launch `influx` to open InfluxDB Shell (CLI)


[grafana-version]: https://img.shields.io/badge/Grafana-7.1.5-brightgreen
[influx-version]: https://img.shields.io/badge/Influx-1.8.1-brightgreen
[chronograf-version]: https://img.shields.io/badge/Chronograf-1.8.4-brightgreen
