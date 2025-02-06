---
tag: Robotics
---
# SPI

# Theory

[https://core-electronics.com.au/tutorials/spi-arduino-tutorial.html](https://core-electronics.com.au/tutorials/spi-arduino-tutorial.html)

```
SPI stands for Serial Peripheral Interface and it is a way to send data between microcontrollers and other small devices. It is a synchronous data bus, meaning it uses a clock to regulate the data transfer.

SPI is also Full-Duplex communication meaning we can have data being sent and received simultaneously.

The SPI Master is the one that generates the clock (in our case this will be the Arduino). The SPI restricts us to having 1 Master and multiple slaves, meaning the master controls communication for all the slaves. There are 4 data lines that SPI uses:

When the Master sends data to the Slave it uses a MOSI (Master out Slave In) Line
When the Slave wants to send data back to the Master it uses a MISO (Master in Slave Out) Line
These 2 lines are regulated by the SCK, which is Serial Clock.
SS Line is the slave select line. It is held high by the master until it has data to be sent. In which case, the Master drops this line low. We call this Active Low logic as the line goes low causing the slave to become active. This is sometimes referred to as an Enable line.
```

[https://www.arduino.cc/en/reference/SPI](https://www.arduino.cc/en/reference/SPI)

# Practice

So, all this means that when you need to wire a component that uses SPI as it's communication protocol, you need to wire the pins to specific pins on the Arduino. For the Arduino Uno, they are shown as follows.

![[SPIArduinoUnoPins.png|SPI%20f22a74da520349d290428dc5c53ae8c2/Arudiuno_SPI_pins.png]]