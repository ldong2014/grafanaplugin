TDengine Datasource - build by Taosdata Inc. www.taosdata.com

TDengine backend server implement 2 urls:

*   `/heartbeat` return 200 ok. Used for "Test connection" on the datasource config page.
*   `/query` return data based on input sqls.

## Installation

To install this plugin:  
Copy the data source you want to /var/lib/grafana/plugins/. Then restart grafana-server. The new data source should now be available in the data source type dropdown in the Add Data Source View.

```
cp -r <grafanaplugin-dir> /var/lib/grafana/plugins/
# if you use TDengine repo's grafana plugin submodule, use "cp -r <tdengine-extrach-dir>/connector/grafana/tdengine /var/lib/grafana/plugins/" instead of above command.
sudo service grafana-server restart
```

### Query API

Example request

```javascript
[{
    "refId": "A",
    "alias": "taosd-memory",
    "sql": "select avg(mem_taosd) from sys.dn where ts > now-5m and ts < now interval(500a)"
},
{
    "refId": "B",
    "alias": "system-memory",
    "sql": "select avg(mem_system) from sys.dn where ts > now-5m and ts < now interval(500a)"
}]
```

Example response

```javascript
[{
    "datapoints": [
        [206.488281, 1538137825000],
        [206.488281, 1538137855000],
        [206.488281, 1538137885500],
        [210.609375, 1538137915500],
        [210.867188, 1538137945500]
    ],
    "refId": "A",
    "target": "taosd-memory"
},
{
    "datapoints": [
        [2910.218750, 1538137825000],
        [2912.265625, 1538137855000],
        [2912.437500, 1538137885500],
        [2916.644531, 1538137915500],
        [2917.066406, 1538137945500]
    ],
    "refId": "B",
    "target": "system-memory"
}]
```

### Heartbeat API

Example request

```javascript
 Get request /heartbeat
```

Example response

```javascript
{
    "message": "Grafana server receive a quest from you!"
}
```

### Dev setup

This plugin requires node 6.10.0

```javascript

npm install -g yarn 
yarn install
npm run build
```

### Import Dashboard

after login `http://localhost:3000` , then you can import the tdengine demo dashboard to monitor the system metrics.

you can import the `dashboard/tdengine-grafana.json`:

![import_dashboard](https://raw.githubusercontent/taosdata/grafanaplugin/master/dashboard/import_dashboard.png)

after finished import:

![import_dashboard](https://raw.githubusercontent/taosdata/grafanaplugin/master/dashboard/tdengine_dashboard.png)
