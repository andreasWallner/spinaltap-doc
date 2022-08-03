USB
===

Main concerns:
 - a single USB configuration (on Windows no configuration switch is possible via libusb)
 - split USB interface descriptors for reader and analyzer interaction (only a single application can request access to an interface at a time and we want to be able to use sigrok for the analyzer while the reader is being used)

Device
------

The device enumerates as
 - USB 2.0 Device
 - VID: 0x1209
 - PID: 0x1337
 - Device Class: Vendor (0xff)
 - Device Sub Class: spinalTap (0x00)
 - Device Protocol: No class specific protocol on device basis (0x00)
 - Device: Release Version (e.g. 0x0010 for v0.1)
 - Serial Number: serial number of device

As noted above, the device provides two interfaces, one for the reader and another one for the logic analyzer.

Device Commands
...............

Reader
......

The reader interface enumerates as vendor interface class (0xff) and interface subclass 0x01.
The interface protocol gives information about the capabilities and version of the interface.

See the appropriate page for any given protocol.

Analyzer
........

The analyzer interface enumerates as vendor interface class (0xff) and interface subclass 0x02.
The interface provides one in endpoint for captured data, commands are sent as interface requests to the interface.

Development Notes
-----------------

- On Windows the "USB Device Tree Viewer" can be quite helpful in debugging USB, especially enumeration issues.
