# Install Prometheus_Grafana docker image in ubuntu server Z600 workstation
- prometheus port: 9090	monitoring server pull data 
- node-exporter port: 9100	collect hardware/os metrics
- grafana port 3000	data visualation
- cadvisor 9595	collect all containers that running on server

## Installation using docker
- install prometheus and run this container
```
docker run -d --name prometheus_con prom/prometheus
docker cp -a prometheus_con:/etc/prometheus/ $PWD/prometheus
docker run -d --name prometheus_con -p 9090:9090 -v $PWD/prometheus:/etc/prometheus prom/prometheus
ufw allow 9090/tcp
```
- install node-exporter and run this container
```
docker run -d --name node-exporter_con -p 9100:9100 -v "/proc:/host/proc:ro" -v "/sys:/host/sys:ro" -v "/:/rootfs:ro" --net="host" prom/node-exporter
ufw allow 9100/tcp
```
- install grafana
```
mkdir grafana
chmod 777 -R ./grafana/
docker run -d --name=grafana_con -p 3000:3000 -v $PWD/grafana:/var/lib/grafana grafana/grafana
ufw allow 3000/tcp
```
- install cadvisor
```
docker run -d \
--name=cadvisor_con \
--volume=/:/rootfs:ro \
--volume=/var/run:/var/run:rw \
--volume=/sys:/sys:ro \
--volume=/var/lib/docker/:/var/lib/docker:ro \
--volume=/cgroup:/cgroup:ro \
--publish=9595:8080 \
--detach=true \
--privileged=true \
google/cadvisor

ufw allow 3000/tcp
```
