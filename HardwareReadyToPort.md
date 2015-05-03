# Introduction #

_Open PHD Guiding_ got a big boost by the INDI driver Geoff wrote as this gives something akin to ASCOM for the Linux crowd.  But, more can be done here.  There are a number of hardware devices supported by PHD that either have some form of Linux driver already or that have drivers I (Craig) have written for the Mac and that I've confirmed may be included with _Open PHD_ without violating any agreements (permission of the manufacturer).

Any takers?


# Details #

Hardware with Mac drivers I've written and that is ready to port:
  * Shoestring GCUSB - guide port adapter that uses a simple HID protocol
  * GC USB ST4 - another guide port adapter, this one using a serial port protocol

Hardware with Linux drivers already
  * Firewire / 1394 / DCAM cameras - I use libdc1394 for the OS X support and this is on Linux.  Perhaps this is already in INDI or an INDI driver should be written