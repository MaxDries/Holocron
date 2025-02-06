# SQLite Database

There are a number of steps to creating and using a database in your project through Codespaces.

Open the project in Codespaces.

1. Click on the extensions tab in the VScode instance that opens.
2. Search for “database” in the search box.
3. Install the extension called “Database Client JDBC”.
4. Click on the install button.

This may take a minute or two and you may be required to reload the VScode instance for it to be enabled.

![[vscodeSQL0.png]]

After that’s installed, click on the Terminal tab at the bottom of the screen and enter the following command and then hit enter.

`sudo apt update -y | sudo apt install -y sqlite3 libsqlite3-dev`

> [!important] You may see different output that shown here. That’s because I’ve already installed it.


![[vscodeSQL1.png]]

1. Click on the Database extension icon on the left hand side.
2. Then click Create Connection.
3. Choose SQLite in the database options.

![[vscodeSQL2.png]]

1. Click on the Browse button
2. Choose the database file supplied in your project.
3. Then click Ok.

![[vscodeSQL3.png]]

Click Connect.

![[vscodeSQL4.png]]

You’ll now be able to access the database tables in the supplied database.

> [!important] Your database may have a different structure to the one shown in the image. This is just an example of how the extension shows the table structure.


![[vscodeSQL5.png]]