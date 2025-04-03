

## Database Information

The project up to this point involves uploading data from the ESP32 to the PHP server and then stored in the database. 

The database tables are shown here for the project so far.

![[dataReadERD.png]]

A new table needs to be added which will store a command (LED on, LED off etc) as well as the module information.

This table needs to be created so that the ESP32’s can connect to the server to retrieve the correct data, with the PHP page confirming that the ESP32 is attempting to access the correct data.

Each ESP32 should only be able to access the specific command, and be denied access to another other ESP32’s data.

The new table - `moduleCommands` - will contain the fields to store data, so that ESPs must submit the correct actuator name and associated password before receiving the command. The PHP files will handle the logic of the process, the database tables simply stores the data.

The password is stored in `hashedPassword` . Passwords should never be stored in plain text in a database in case unauthorised users (bad actors) get access to the database. Hashing passwords involves encrypting the password prior to storing the password in the database. 

![[dataReadModuleCommands.png]]
