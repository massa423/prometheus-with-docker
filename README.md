# Prerequisites

* Docker
* Docker Compose V2

# Usage

## common

### create data directory

#### Prometheus

```
mkdir /prometheus
chown nobody:nogroup /prometheus
```

#### Alertmanager

```
mkdir /alertmanager
chown nobody:nogroup /alertmanager
```

#### Grafana

```
groupadd -g 472 grafana
useradd -u 472 -g 472 -M -s /sbin/nologin grafana
mkdir /grafana
chown -R grafana:grafana /grafana
```

#### Loki

```
groupadd -g 10001 loki
useradd -u 10001 -g 10001 -M -s /sbin/nologin loki
mkdir /loki
chown -R loki:loki /loki
```

### get repository

```
git clone https://github.com/massa423/prometheus-with-docker.git
cd prometheus-with-docker
```

### create alertmanager.yml from sample

```
cp -p alertmanager/alertmanager{_sample,}.yml
```

## use docker compose

```
docker compose up -d
docker compose ps
```

## create container individually

### create docker network

```
docker network create prometheus-network
```

### blackbox_exporter

#### build image

```
cd blackbox_exporter
docker build -t <tag> .
```

#### run container

```
docker run --name blackbox_exporter --network prometheus-network -d -p 9115:9115 <tag>
```

#### checking the results

```
curl 'http://localhost:9115/probe?target=8.8.8.8&module=icmp'
```

### Prometheus

#### build image

```
docker build -t <tag> .
```

#### run container

```
docker run --name prometheus --network prometheus-network -d -p 9090:9090 -v /prometheus:/prometheus <tag>
```

### Alertmanager

#### build image

```
docker build -t <tag> .
```

#### run container

```
docker run --name alertmanager --network prometheus-network -d -p 9093:9093 -v /alertmanager:/alertmanager <tag>
```

### Grafana

#### run container

```
docker run --name grafana -d -p 3000:3000 grafana/grafana:7.1.4
```
