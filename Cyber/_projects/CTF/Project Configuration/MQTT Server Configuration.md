`Mosquitto` is installed on the server.

The script `DatabaseToMQTT.py` (stored in the root of the project) runs on a continual loop on the server.

You can use MQTT explorer to access all topics.

MQTT Broker:

Server IP: `10.177.200.71`
Port: `1883`

![[mqttExplorer.png]]


# Service

There is a custom service on the server located:

`/usr/lib/systemd/system/DatabaseToMQTT.service"

which contains the following info:

```bash
[Unit]
Description=Keep python script that dumps the SQL binlog running
After=network.target

[Service]
Type=idle
Restart=on-failure
#User=root
ExecStart=/usr/bin/python3 /var/www/CyberCity/DatabaseToMQTT.py

[Install]
WantedBy=multi-user.target
```

There is an equivalent `DatabaseToDocker.service`, containing
```bash
[Unit]
Description=Keep python script that runs dynamic docker containers running
After=network.target

[Service]
Type=idle
Restart=on-failure
#User=root
ExecStart=/usr/bin/python3 /var/www/CyberCity/DatabaseToDocker.py

[Install]
WantedBy=multi-user.target
```

The `DatabaseToMQTT.py` script runs continuously, and keeps the topics and data up to date.

