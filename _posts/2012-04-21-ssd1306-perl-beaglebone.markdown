---
layout: post
title:  "An SSD1306 LCD controller in Perl for the Beaglebone"
date:   2012-04-21 12:09:35 +1100
categories: perl beaglebone embedded
---

I've been hacking away on the BeagleBone some more, and I've come up with a controller for the common SSD1306 LCD controller, using the SPI interface. This controller is behind a bunch of the available small graphical LCD panels out there.

I've also written supporting libraries to display both text and PNG files, although don't expect too much from a 128x32 monochrome image!

I've uploaded my notes and libraries here:
https://github.com/TJC/BeagleBone

Coding it is as simple as:

```
my $lcd = BeagleBone::SSD1306::Text->new(
  rst_pin => 'P9_15',
  dc_pin => 'P9_16',
  # remainder of pins must be connected to SPI pins
);
$lcd->display_string("Hello world!");
```
