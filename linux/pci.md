## PCI介绍

### 概念

* PCI - Peripheral Component Interconnect(外围设备互联)
* 一种外设总线规范

### PCI寻址

每个PCI设备由总线号(bus number)、设备号(device number)、功能号(function number)标识

bus 8 bits 0~ff
device 5 bits 0~1f
function 3 bits 0~7

示例: 03:00.0 (bus number=03 device number=00 function number=0)
```
root@dev:/opt/bash# lspci -n | tail -n 1
03:00.0 0107: 15ad:07c0 (rev 02)
```

### Linux中设备代表

Class ID + Vendor ID + Device ID

示例: 03:00.0 (Class ID=0107 Vendor ID=15ad Device ID=07c0)
```
root@dev:/opt/bash# lspci -n | tail -n 1
03:00.0 0107: 15ad:07c0 (rev 02)
```

### 怎样确认驱动

src/linux/drivers
