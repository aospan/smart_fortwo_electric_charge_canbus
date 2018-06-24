# Electric Smart fortwo car CAN bus dump

http://canable.io/ USB dongle used. Access to CAN bus obtained through OBD port (pins 6 and 14).
Host machine is Nvidia Jetson TX2 with Ubuntu 16.04 LTS Linux.

## Charging from 94.2% to 100%
CAN bus dump was created with command:
```tshark  -i 2 -w smart-fortwo-electric-charge-obd-can-soc94.2-to-soc100-240V-AC-14A.pcap```
Wireshark GUI can be used to examine this file.

In the same time charging current monitored using OpenEVSE (over http API):
```charge_current-soc94.2-to-soc100-240V-AC-14A.txt```

charging voltage 240V AC, charging current limited to 14A.

## Useful information for CAN messages decoding
BMS, cooling, charging, etc info can be decoded using information from following C source file (from ED BMSdiag project):
[canDiag.cpp](https://github.com/MyLab-odyssey/ED_BMSdiag/blob/master/ED_BMSdiag/canDiag.cpp)

For example, battery charge level (SOC) can be obtained from CAN message ID 0x2D5.
![CAN 0x2D5 in Wireshark](smart-fortwo-electric-charge-obd-can-wireshark.png)

using this values we can get SOC state:
```((0x03 & 0x03)*256 + 0xe5)/10 = 99.7%```
