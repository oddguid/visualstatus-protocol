SerialLed
=========

The _SerialLed_ protocol is a serial protocol that is used to set the color of one or more LEDs that are attached to a serial port. The protocol is plain text to allow for easier testing with a terminal program.

Up to 256 LEDs can be addressed and for each LED two RGB colors can be set. The serial device will toggle between these two colors at a given rate. This rate can be set for all LEDs, not individual LEDs.

General
-------

This protocol uses a request-reply mechanism, which means that after sending a request (command) the system should wait for a reply from the serial device.

Every request and reply must end with __\n__.

The serial device will reply with either __!\n__ to indicate success or __?\n__ to indicate failure.

After opening the serial port the serial device will sent a message (__!\n__) to indicate that it is ready to receive requests. It is advisable to wait for this message, especially when an Arduino is used (Arduino resets when opening a serial connection).

Commands
--------

### Set color

Sets the 2 colors of a single LED. Command string:

```
wXXX-R1R1R1G1G1G1B1B1B1-R2R2R2G2G2G2B2B2B2\n
```

* w : command to write color, may be upper case.
* XXX : LED number, range is [0, 255].
* R1R1R1 : value of red component of first color, range is [0, 255].
* G1G1G1 : value of green component of first color, range is [0, 255].
* B1B1B1 : value of blue component of first color, range is [0, 255].
* R2R2R2 : value of red component of second color, range is [0, 255].
* G2G2G2 : value of green component of second color, range is [0, 255].
* B2B2B2 : value of blue component of second color, range is [0, 255].

Example:

```
w000-000128256-256000128\n
```

### Set delay

Sets the delay between the color toggles in milliseconds. The minimum allowable delay is determined by the serial device. Command string:

```
dTTTTT\n
```

* d : command to set delay, may be upper case.
* TTTTT : delay in milliseconds, range is [0, 65535].

Example with a delay of 500ms which gives 2 toggles per second:

```
d00500\n
```

### Clear all colors

Clears the colors of all LEDs. Command string:

```
c\n
```

* c : command to clear colors, may be upper case.

### Set display mode

Turns the LEDs of the serial device on or off. Command string:

```
dX\n
```

* d : command to turn LEDs on/off, may be upper case.
* X : 0 to turn LEDs off, 1 to turn LEDs on.

Example:

```
d1\n
```
