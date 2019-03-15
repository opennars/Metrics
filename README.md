# Metrics
Time-Series repository and metric dashboards for measuring system performance

<img src="https://github.com/opennars/Metrics/blob/master/Img/Pong%20Metrics.png">

## Config
Install docker containers
```
Install docker on your platform of choice (the following config has been tested on Windows 10)
Once installed:
docker run -d --name graphite -p 80:80 -p 2003-2004:2003-2004 -p 2023-2024:2023-2024 -p 8125:8125/udp -p 8126:8126 graphiteapp/graphite-statsd
docker run -d --name=grafana -p 3000:3000 grafana/grafana

Create a bridge network for docker containers
---------------------------------------------
docker network create metric-net
docker network connect metric-net graphite
docker network connect metric-net grafana
docker network inspect metric-net
```
Configure Grafana data source
```
In your browser enter http://localhost:3000

Default user/password is: admin/admin

Select add new data source and choose Graphite.

Set the URL to the virtual IP address from the inspect command above, using port 80:
example: http://172.18.0.2:80 

Import dashboard
Download the Pong dashboard .json file from the Github repository (Dashboards)
In Grafana navigate to 'import dashboard' and select 'upload .json file' then select the download .json file.
Select the Pong dashboard on the Home page.
```
## Run
Now run Open-NARS v3.0.2 LAB and select Pong application from the launcher menu to start collecting metrics.

After a few seconds the sessionID and version fields (top left) will be populated and data will start to appear on the graphs.

Each Pong session will generate a unique session id and can be selected from the sessionId drop down. Different Open-NARS versions can be compared by running the respective versions and selecting the relevant version number in the drop down.
```
At the end of your metric sesssion the docker contianers can be stopped
docker stop grafana
docker stop graphite

and restarted on subsequent sessions
docker restart graphite
docker restart grafana

Your data will be persisted between sessions
```

## Analysis
In the initial Pong Dashboard there are a few interesting points to be aware of:
1. Learning breakeven point - when Hits = Misses - The time to reach this point can be interpretated as the initial learning rate. A few seconds at the start should be allowed to overcome any initial 'lucky' bias.
2. Convergence to Hit/Miss ratio - This can be interpreted as a capability level of the current version
3. Miss distance - this is a form of reliability metric in that the lower the miss distance (Total) the better the long term reliability.

