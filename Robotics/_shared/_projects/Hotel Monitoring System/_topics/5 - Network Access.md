

## `sensitiveInformation.h`

First, open `sensitiveInformation.h` and confirm that the Settings are correct.

The Host, SSID and password should be configured for the classroom’s RoboRange network.

```arduino
const char* host = "RMS";
const char* ssid = "RoboRange";       // Wifi Network Name
const char* password = "Password01";  // Wifi Password
```

The serverURL variable should be set to the subject specific URL.

```arduino
String serverURL = "http://10.177.200.71/JEDI2023/dataTransfer.php";
```

Add a new URL called `eventLogURL` to direct the code to the url to store event data, such as “Door Open” etc.

```arduino
String eventLogURL = "http://10.177.200.71/JEDI2023/eventLog.php";
```

The last block of code should change to be specific to your module.

At this stage, leave the `apiKeyValue` and `moduleName` as is. Set the `user` variable to be your name.

```arduino
String apiKeyValue = "api";
String moduleName = "Temperature";
String user = "Ryan Cather";
```

At this end of the process, your `sensitiveInformation.h` file should appear similar to this.

![[loopWhileLoop.png|Untitled]]

$$
\utilde {\color{black} \fcolorbox{darkorange}{darkorange}  {Commit and Push to Github}}
$$

## Connecting to the network

Add the following additional include statements at the top of your code, *after* `#include <Arduino.h>`

```arduino
#include "WiFi.h"
#include <HTTPClient.h>
```

![[loopCSVStructure.png|Untitled]]

Add the following block of code to the `setup()` function.

This code will attempt to connect to the Wifi network with the credentials defined in `sensitiveInformation.h`. IT will then output the IP address to the Serial Monitor.

```arduino
while (!Serial) {
	delay(10);
  }
  delay(1000);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
	delay(1000);
	Serial.println("Connecting to WiFi..");
  }
  Serial.println();
  Serial.print("Connected to the Internet");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
```

![[loopCSVReadData.png|Untitled]]

- Event Logging to the Database


# Event Logging to the Database

Instead of logging events locally (to a SD card or similar), the Arduino can log events to a remote server through the PHP server.

Open `main.cpp` and add the following function near the top of the code.


> [!info] It needs to be stored *after* the include statements and  *prior* to `setup()`
> ![[eventLoggingPosition.png]]



```arduino
void logEvent(String eventData)
{
  if (WiFi.status() == WL_CONNECTED)
  {
	WiFiClient client;
	HTTPClient http;
	Serial.println("Before");
	// Your Domain name with URL path or IP address with path
	http.begin(client, eventLogURL);

	// Specify content-type header
	http.addHeader("Content-Type", "application/x-www-form-urlencoded");

	// Send HTTP POST request, and store response code
	http.addHeader("Content-Type", "application/json");
	String postJSONString = "{\"userName\":\"" + userName + "\",\"eventData\":\"" + eventData + "\"}";

	Serial.print("Debug JSON String: ");
	Serial.println(postJSONString);
	int httpResponseCode = http.POST(postJSONString);

	if (httpResponseCode > 0)
	{
	  Serial.print("HTTP Response code: ");
	  Serial.print(httpResponseCode);
	  Serial.println(".");
	}
	else
	{
	  Serial.print("Error code: ");
	  Serial.println(httpResponseCode);
	}

	// Free resources
	http.end();
  }
  else
  {
	Serial.println("WiFi Disconnected");
  }
}
```

Add two events that are logged. The first in `setup()` to indicate the system has initialised. The second include in the main `loop()` as a test post.

```arduino
logEvent("System Initalised");
```



Upload the code to the Adafruit ESP32 Feather and check the database to ensure that it’s being stored correctly.



![[commonBlocks#Commit & Push]]