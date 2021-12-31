# Winners_TemperatureHumidity
reading a temerature and humidity modbus sensor from 'Winners' with
- openHAB

![Winners_TemperatureHumidity](https://user-images.githubusercontent.com/51090559/147771810-cbe96977-82fe-4374-9a76-127db4025c80.jpg)

## Serial parameters
baud rate       : 9600\
data bits       : 8\
stop bit        : 1\
parity          : no\
Device address  : can be set to 1-255, the default is 1

## wirering
Gelb    :   9..36V\
Black   :   GND\
Red     :   A-\
Green   :   B-\

## Processing
### Change client id
Changing the client id can be easily done with SerialPortTester.
To change the client id following command has to be send (in the example the client id is changed to 2):

    send        00 06 00 00 00 02 09 DA
    receive     00 06 00 00 00 02 09 DA

The byte values have the follwing meanings

    Station     0x00        ->  0   -> BoradCast
    CMD         0x06        ->  FC6 -> Preset Single Register
    StartAddr   0x00 0x00   ->  0
    RegValue    0x00 0x02   ->  2   -> New client id
    CRC         0x09 0xDA

please note when changing the address, there can be only one slave device on the bus, otherwise all slave devices will be changed.

If you want to use a different id, you have to change the command as follows
  send      00 06 00 00 00 <new ID> <CRC>

To receive the corresponding CRC, you can use https://crccalc.com/?crc=000600000002&method=crc16&datatype=hex&outtype=hex
The result (CRC-16/ModBus) for client id 2 should be 0xDA09. Swap bytes and append to cmd.

