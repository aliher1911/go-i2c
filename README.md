I2C bus setting up and usage for linux on RPi device and respective clones
==========================================================================

[![Build Status](https://travis-ci.org/d2r2/go-i2c.svg?branch=master)](https://travis-ci.org/d2r2/go-i2c)
[![Go Report Card](https://goreportcard.com/badge/github.com/d2r2/go-i2c)](https://goreportcard.com/report/github.com/d2r2/go-i2c)
[![GoDoc](https://godoc.org/github.com/d2r2/go-i2c?status.svg)](https://godoc.org/github.com/d2r2/go-i2c)
[![MIT License](http://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

This library written in [Go programming language](https://golang.org/) intended to activate and interact with the I2C bus by reading and writing data.

Compatibility
-------------

Tested on Raspberry PI 1 (model B) and Banana PI (model M1).

Golang usage
------------

```go
func main() {
  // Create new connection to I2C bus on 2 line with address 0x27
  i2c, err := i2c.NewI2C(0x27, 2)
  if err != nil { log.Fatal(err) }
  // Free I2C connection on exit
  defer i2c.Close()
  ....
  // Here goes code specific for sending and reading data
  // to and from device connected via I2C bus, like:
  _, err := i2c.Write([]byte{0x1, 0xF3})
  if err != nil { log.Fatal(err) }
  ....
}
```

Tutorial
--------

You can find a certain number of my projects here, which use i2c library as a starting point to interact with various peripheral devices and sensors for use in embedded Linux devices. All of these libraries start with a standard call to open I2C-connection to specific bus line and address, than pass i2c instance to device.

By default, go-i2c use [go-logger](https://github.com/d2r2/go-logger) library to output lines with debug and other notification's lines which produce all necessary levels of verbosity. You can manage what level of verbosity you would like to see, by adding call:
```go
// Uncomment/comment next line to suppress/increase verbosity of output
logger.ChangePackageLogLevel("i2c", logger.InfoLevel)
```
Once you put this call, it will decrease verbosity from default "Debug" up to next "Info" level, reducing the number of low-level console outputs that occur during interaction with the I2C bus. Please, find examples in corresponding I2C-driven sensors among my projects.

Here I will list all devices and sensors supported by me that reference this library:
- [Liquid-crystal display driven by Hitachi HD44780 IC](https://github.com/d2r2/go-hd44780).
- [Bosch Sensortec BMP180/BMP280/BME280 temperature and pressure sensors](https://github.com/d2r2/go-bsbmp).
- [Aosong Electronics DHT12/AM2320 humidity and temprature sensors](https://github.com/d2r2/go-aosong).
- [Silicon Labs Si7021 relative humidity and temperature sensor](https://github.com/d2r2/go-si7021).
- [Sensirion SHT3x humidity and temperature sensor's family](https://github.com/d2r2/go-sht3x).


Getting help
------------

GoDoc [documentation](http://godoc.org/github.com/d2r2/go-i2c)

Troubleshoting
--------------

- *How to obtain fresh Golang installation to RPi device (either any RPi clone):*
If your RaspberryPI golang installation taken by default from repository is outdated, you may consider
to install actual golang mannualy from official Golang [site](https://golang.org/dl/). Download
tar.gz file containing armv6l in the name. Follow installation instructions.

- *How to enable I2C bus on RPi device:*
If you employ RaspberryPI, use raspi-config utility to activate i2c-bus on the OS level.
Go to "Interfaceing Options" menu, to active I2C bus.
Probably you will need to reboot to load i2c kernel module.
Finally you should have device like /dev/i2c-1 present in the system.

- *How to find I2C bus allocation and device address:*
Use i2cdetect utility in format "i2cdetect -y X", where X may vary from 0 to 5 or more,
to discover address occupied by peripheral device. To install utility you should run
`apt install i2c-tools` on debian-kind system. `i2cdetect -y 1` sample output:
	```
	     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
	00:          -- -- -- -- -- -- -- -- -- -- -- -- --
	10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	70: -- -- -- -- -- -- 76 --    
	```

License
-------

Go-i2c is licensed inder MIT License.
