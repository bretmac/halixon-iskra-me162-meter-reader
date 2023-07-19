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
## Hardware Needed
## Protocol (readout)
## Protocol (Mode C)
## CRC Calculations
## Example Output
