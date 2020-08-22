# Usage

## create docker network

```
docker network create prometheus-network
```

## blackbox_exporter

### build image

```
cd blackbox_exporter
docker build -t massa423/blackbox_exporter:v0.2.0 .
```

### run container

```
docker run --name blackbox_exporter --network prometheus-network -d -p 9115:9115 massa423/blackbox_exporter:v0.2.0
```

### checking the results

```
curl 'http://localhost:9115/probe?target=8.8.8.8&module=icmp'
```

## Prometheus

### create data directory

```
mkdir /prometheus
chown nobody:nogroup /prometheus
```

### run container

```
docker build -t massa423/prometheus:v0.2.0 .
```

```
docker run --name prometheus --network prometheus-network -d -p 9090:9090 -v /prometheus:/prometheus massa423/prometheus:v0.2.0
```

## Grafana

### run container

```
docker run -d -p 3000:3000 grafana/grafana:7.1.4
```
