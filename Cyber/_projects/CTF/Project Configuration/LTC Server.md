# Login

The login for the server is:
```
Username: ltc
Password: <REDACTED>
```
The unredacted password is on the Shared Google Drive.

# Server Usage

If you need to make any changes to the server, you can either:
1) Use the server directly, or
2) SSH remotely into the server through the terminal.


Open the terminal/console (on the server it's called `Konsole`).
1. (if NOT on the server) ssh into the server following these steps:
	1. type the following command: `ssh ltc@10.177.200.71`
	2. when prompted, entered the password

# Website Update

To update the server with current changes in the Github repository, follow these steps:

1. Open the terminal/SSH into the server. See above.
2. Enter `cd /var/www/CyberCity`. Hit Enter.
3. Type `git pull`

This will pull all the changes to the server. No other changes are necessary - the website is automatically refreshed.

# Server Services

There are two services that run on the LTC server related to this project:
```
DatabaseToDocker.service
DatabaseMQTT.service
```

If you need to restart these services, use the following terminal command:

```
sudo service DatabaseMQTT restart
```
replacing `DatabaseMQTT` with the service you need to restart.