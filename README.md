This repo is Work In Progress.  Watch for updates.


# Iskra ME162 Optical Meter Reader

This project contains everything you need to know to read the import and export registers on the Iskra ME162 electricty meter.  It should work for other brands too but this is the only meter it has been tested with.  Get in touch if you have got it working on other makes and models.

## Background

After getting solar panels installed in October 2022 the DNO (Distribution Network Operator) replaced by old rotational disc electric meter with an Iskra ME162 digital one that records both import and export usage.  I had noticed an optical interface on it and thought it would be pretty cool to read it and send the data via MQTT, to Home Assistant, for example.

This project is not a complete system, but documentation of the parts you need to get it working with your MCU of choice.

Inparticular, it will detail:

- Hardware required
- Some protocol information
- Code snippets (in C)
- Lessons Learned

There are off the shelf cables and software available (where is the fun in that?).  They are pricey for what they are and a particular free Android app is just a complete ruse for selling cables at rip-off prices (£100 or more).   You have been warned.

## The Optical Port

The optical port is an international standard: IEC 62056-21.


## Hardware Required

I made up these PCBs, but you will require a round enclosure with some neodimium magnets.  This was not done as part of this project but it's possible to 3D print then, or perhaps turn them on wood lathe.

<img src="https://github.com/bretmac/halixon-iskra-me162-meter-reader/assets/44399243/a94541fe-627d-4af4-a6b4-9aaf8cf2de7b" width=25% height=25%>
<img src="https://github.com/bretmac/halixon-iskra-me162-meter-reader/assets/44399243/3c82cd81-d4f8-4d46-882d-2e1977566824" width=25% height=25%>

You can buy the boards above from my eBay listing [here](https://www.ebay.co.uk/itm/204371156344).

The IR LED and IR Photodiode are 5mm in diameter.

The LED centres are 6.5mm apart.

The PCB is 28mm in diameter.

The enclosure's external diameter should be 32mm.


## Circuit Diagram

![image](https://github.com/bretmac/halixon-iskra-me162-meter-reader/assets/44399243/c1cf090b-b5fe-4a04-be6e-29267755cb27)

The circuit has echo cancellation (receiver does not work whilst data is being transmitted).  R2 has been chosen for 3.3V operation; the IR LED forward current is just under 20mA @ 1.27V.  The circuit has not been tested on 5V but R2 will need to be changed (should be around 185 Ohms).


## Protocol (readout)

The simplest way to receive meter readings is to just send a sign-on request and wait for a few seconds.

The UART settings you need are as follows:

Name|Value
---|---
Baud Rate|300
Data Bits|7
Parity|Even
Stop Bits|1

## Protocol (Mode C)
## CRC Calculations

```
unsigned char calculate_bcc(unsigned char *packet, int length)
{
    unsigned char checksum = 0;

    for (int i = 1; i < length - 1; i++)
    {
        checksum ^= packet[i];
    }

    return checksum;
}
```

## Example Output

This is the JSON output from my own ESP32-S3 implementation.  This is being sent to an MQTT broker every five seconds.


```
{
  "uart_channel_number:": 1,
  "meter_serial_number": "12345678",
  "meter_import": "0001362.414*kWh",
  "meter_export": "0001031.432*kWh",
  "timestamp": 1689836885,
  "timestamp_msec": 345
}
```
