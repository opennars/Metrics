# Metrics
Time-Series repository and metric dashboards for measuring system performance

## Config
Install docker containers
-------------------------
docker run -d --name graphite -p 80:80 -p 2003-2004:2003-2004 -p 2023-2024:2023-2024 -p 8125:8125/udp -p 8126:8126 graphiteapp/graphite-statsd
docker run -d --name=grafana -p 3000:3000 grafana/grafana

Create a bridge network for docker containers
--------------------------------------------
docker network create metric-net
docker network connect metric-net graphite
docker network connect metric-net grafana

docker network inspect metric-net

Configure Grafana data source
-----------------------------
Set the Grafana datasource to the virtual IP address from the inspect command above, using port 80:
example: http://172.19.0.2:80 

## Run
In your browser enter http://localhost:3000

Import dashboard
----------------
