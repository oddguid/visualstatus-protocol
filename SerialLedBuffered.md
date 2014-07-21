SerialLedBuffered
=================

The _SerialLedBuffered_ protocol is a serial protocol that is used to set the color of one or more LEDs that are attached to a serial port. The protocol is binary to allow for compact transfers and uses the data format of the [Arduino EasyTransfer](https://github.com/madsci1016/Arduino-EasyTransfer) library by [Bill Porter](http://www.billporter.info/2011/05/30/easytransfer-arduino-library/).

General
-------

The byte buffer that is transferred consists of:
* 2 header bytes (0x06 and 0x85)
* 1 byte to indicate the size of the payload
* payload (_size_ bytes)
* 1 checksum byte

The maximum payload is 255 bytes.

The payload consists of RGB colors (3 bytes) and can support up to __85__ LEDs.

The order of the RGB color components is red (1 byte), green (1 byte) and blue (1 byte).

The checksum is calculated by taking the _size_ byte and xor-ing it with all payload bytes.

The serial device will reply with either __!\n__ to indicate success or __?\n__ to indicate failure.

After opening the serial port the serial device will sent a message (__!\n__) to indicate that it is ready to receive requests. It is advisable to wait for this message, especially when an Arduino is used (Arduino resets when opening a serial connection).

### Example

Given the following 2 RGB colors:

* color A with red = 0, green = 63, blue = 123
* color B with red = 63, green = 216, blue = 3

The byte buffer will have a payload of 6 bytes:

```
          +----------+
HEADER    |   0x06   |
          |   0x85   |
          +----------+
SIZE      |   0x06   |   2 * 3
          +----------+
PAYLOAD   |   0x00   |   color A, red
          |   0x3F   |   color A, green
          |   0x7B   |   color A, blue
          |   0x3F   |   color B, red
          |   0xD8   |   color B, green
          |   0x03   |   color B, blue
          +----------+
CHECKSUM  |   0xA6   |   
          +----------+
```
