# MPL115A1 barometer for the Pycom LoPy

## Credits

Based on original code from the [hackscribble microbit-MPL115A1-barometer repo](https://github.com/hackscribble/microbit-MPL115A1-barometer) and it's for for the [ESP8266 by flynnguy](https://github.com/flynnguy/esp8266-MPL115A1-barometer)

## Usage

Very little has changed to adapt for the LoPy. Currently the code uses the SPI default pins for CLK, MOSI and MISO (`P10`, `P11` and `P14`).

If using the default pins, the connections are as follows

| MPL115A1 Pin | Sparkfun MPL115A1 | LoPy Pin | Exp Board Pin |
| ---- |-----| ----| --- |
| SCLK | SCK | P10 | G17 |
| MISO | SDO | P11 | G4  |
| MOSI | SDI | P14 | G22 |
| CS   | CSN | P12 | G20 |
| SHDN | SDN | 3V  | 3V  |
| VDD  | VDD | 3V  | 3V  |
| GND  | GND | GND | GND |


Since the Sparkfun MPL115A1 breakout was being used for this project there is an additional `SDN`/`SHDN` pin. This pin can be tied to `VDD` to avoid confusion. The `SDN`/`SHDN` pin activates the breakout of voltage is `HIGH` and disables if `LOW`

If using Pymakr, you should be able to simply run the `mpl115a1.py` script.


### Editing SPI setup

If you wish to use another SPI setup, then edit `line 33`. [Using the SPI documentation from pycom](https://docs.pycom.io/firmwareapi/pycom/machine/spi.html), it should roughly look like

```python
spi = SPI(0, mode=SPI.MASTER, baudrate=2000000, polarity=0, phase=0, pins=(sclk, mosi, miso))
```

This will likely changed after the initial commit of this repo.

## Notes

The MPL115A1 datasheet says ...

> The sensor die is sensitive to light exposure. Direct light exposure through the port hole can lead to varied accuracy of pressure
measurement. Avoid such exposure to the port during normal operation.

Two items need further investigation:

- Forum threads about the MPL115A1  somewhere that claimed the temperature calculation algorithm in the datasheet is wrong, giving consistently low readings.  See https://github.com/hackscribble/microbit-MPL115A1-barometer/issues/1

- This version of the code retains the formula for calculating sea level pressure (P0) that was used in the original program.  It is different from that shown in source [3] below, which factors in temperature as well as altitude.  See https://github.com/hackscribble/microbit-MPL115A1-barometer/issues/2

## Sources

1. Datasheet: http://www.nxp.com/assets/documents/data/en/data-sheets/MPL115A1.pdf

2. Background on MicroPython SPI: https://learn.adafruit.com/micropython-hardware-spi-devices/spi-master.

3. Information about adjusting pressure for altitude: http://keisan.casio.com/exec/system/1224575267
