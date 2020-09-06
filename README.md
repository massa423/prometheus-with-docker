# Usage

## common

### create data directory

#### Prometheus

```
mkdir /prometheus
chown nobody:nogroup /prometheus
```

### Alertmanager

```
mkdir /alertmanager
chown nobody:nogroup /alertmanager
```

### Grafana

```
useradd -u 472 -U -M -s /sbin/nologin grafana
mkdir /grafana
chown -R grafana:grafana /grafana
```

## use docker compose

```
docker-compose up -d
docker-compose ps
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
