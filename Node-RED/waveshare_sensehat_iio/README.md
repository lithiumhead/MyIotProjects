# Waveshare electronics Sense HAT (B)

Sense HAT (B) is a plugin board for Raspberry Pi that has a bunch of sensors for measuring
temperature, pressure, humidity, orientation and color.

Links to official documentation:

- https://www.waveshare.com/wiki/Sense_HAT_(B)
- https://www.waveshare.com/w/upload/4/43/Sense-HAT-B-Schematic.pdf


## List of I²C Sensors on the Sense HAT (B)

```
pi@IotRpi0wSenseHat:~ $ sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- 29 -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- 48 -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- 5c -- -- --
60: -- -- -- -- -- -- -- -- 68 -- -- -- -- -- -- --
70: 70 -- -- -- -- -- -- --
```

 1. Texas Instruments ADS1015
    - 3.3-kSPS, 12-Bit ADC, 4 Channel
    - I²C address: 0x48
 2. TDK InvenSense ICM-20948
    - Accelerometer
    - Gyroscope
    - Magnetometer
    - I²C address: 0x68
 3. STMicroelectronics LPS22HB
    - 24 bit MEMS 260-1260 hPa absolute digital output barometer
    - 16 bit Temperature Sensor
    - I²C address: 0x5C
 4. Sensirion AG SHTC3
    - Temperature
    - Relative Humidity
    - I²C address: 0x70
 5. Taos TC34725
    - Color sensor
    - I²C address: 0x29

## Basic Setup for IIO + Node-RED

 1. Prepare microSD Card with Lite edition of Raspberry Pi OS 32 bit
 2. Create blank file named `ssh` in boot partition before moving microSD card to RPi
 3. Connect RPi to network using USB Ethernet Dongle and access it over LAN
 4. Using using `raspi-config` configure country, Timezone and WiFi and enable I²C
 5. Disable WiFi Power Saving
 6. Change hostname (`/etc/hostname` and `/etc/hosts`) - don’t change it after this otherwise NodeRED flows disappear.
 7. Set new password
 8. Reboot
 9. Update OS
 10. Install Node-RED
 11. `sudo apt-get install i2c-tools`

## Enabling Industrial I/O (IIO) Kernel Drivers

### ADS1015

Raspberry Pi OS ships with the kernel module blob for ADS1015, we just have to enable it by editing `/boot/config.txt` and append `dtoverlay=ads1015,addr=0x48` to the end. Save and Reboot.

Test:

```
pi@IotRpi0wSenseHat:~ $ ls /sys/bus/iio/devices/iio:device0
buffer                                   in_voltage2_raw
current_timestamp_clock                  in_voltage2_sampling_frequency
dev                                      in_voltage2_scale
events                                   in_voltage2-voltage3_raw
in_voltage0_raw                          in_voltage2-voltage3_sampling_frequency
in_voltage0_sampling_frequency           in_voltage2-voltage3_scale
in_voltage0_scale                        in_voltage3_raw
in_voltage0-voltage1_raw                 in_voltage3_sampling_frequency
in_voltage0-voltage1_sampling_frequency  in_voltage3_scale
in_voltage0-voltage1_scale               name
in_voltage0-voltage3_raw                 of_node
in_voltage0-voltage3_sampling_frequency  power
in_voltage0-voltage3_scale               sampling_frequency_available
in_voltage1_raw                          scale_available
in_voltage1_sampling_frequency           scan_elements
in_voltage1_scale                        subsystem
in_voltage1-voltage3_raw                 trigger
in_voltage1-voltage3_sampling_frequency  uevent
in_voltage1-voltage3_scale
```
### ICM-20948

No kernel support exists for ICM-20948 in the mainline source tree.

### LPS22HB

### SHTC3

- Add and activate Device Tree Blob:
  `sudo apt-get install lm-sensors`
- Add and activate Device Tree Blob:
  - Create a file `~/shtc3.dts` with
	  ```
		// Definitions for SHTC3 Humidity and Temperature Sensor from Sensirion AG
		/dts-v1/;
		/plugin/;

		/ {
		        compatible = "brcm,bcm2708";

		        fragment@0 {
		                target = <&i2c1>;
		                __overlay__ {
		                        #address-cells = <1>;
		                        #size-cells = <0>;
		                        clock-frequency = <400000>;
		                        status = "okay";

		                        shtc3@70 {
		                                compatible = "sensirion,shtc3";
		                                reg = <0x70>;
		                                sensirion,blocking-io;
		                        };
		                };
		        };
		};
	  ```
  - Execute the commands:
    - `sudo dtc -I dts -O dtb -o /boot/overlays/shtc3.dtbo -b 0 -@ ~/shtc3.dts`
  - `sudo nano /boot/config.txt` and append `dtoverlay=shtc3` to the end, save and reboot
  - Test:
		```
		pi@IotRpi0wSenseHat:~ $ sensors
		shtc3-i2c-1-70
		Adapter: bcm2835 (i2c@7e804000)
		temp1:        +27.8°C
		humidity1:     72.8 %RH

		cpu_thermal-virtual-0
		Adapter: Virtual device
		temp1:        +30.4°C

		rpi_volt-isa-0000
		Adapter: ISA adapter
		in0:              N/A

		pi@IotRpi0wSenseHat:~ $ cat /sys/bus/i2c/devices/1-0070/hwmon/hwmon2/humidity1_input
		72772
		pi@IotRpi0wSenseHat:~ $ cat /sys/bus/i2c/devices/1-0070/hwmon/hwmon2/temp1_input
		27824
		```
### TC34725

No kernel support exists for TC34725 in the mainline source tree.

### Notes on IIO support for the various sensors

- ADS1015
  - [Raspberry Pi OS Documentation](https://raw.githubusercontent.com/raspberrypi/firmware/master/boot/overlays/README)
- ICM-20948
  - Kernel Driver not part of mainline, try using https://github.com/sittner/icm20948-mod
- LPS22HB
  - [Driver Location in the Kernel](https://github.com/raspberrypi/linux/blob/rpi-5.10.y/drivers/iio/pressure/st_pressure_core.c)
  - [Device Tree Documentation](https://github.com/raspberrypi/linux/blob/rpi-5.10.y/Documentation/devicetree/bindings/iio/st-sensors.txt)
  - [Original Sources from STMicroelectronics](https://github.com/STMicroelectronics/STMems_Linux_Input_drivers)
- SHTC3
  - Location to the driver in the kernel
    - https://github.com/raspberrypi/linux/blob/rpi-5.10.y/drivers/hwmon/shtc1.c
    - https://www.kernel.org/doc/html/v5.5/hwmon/shtc1.html
    - https://www.kernel.org/doc/Documentation/devicetree/bindings/hwmon/sensirion%2Cshtc1.yaml
    - https://www.kernel.org/doc/Documentation/hwmon/sysfs-interface
    - https://developer.sensirion.com/tutorials/raspberry-pi-tutorial-shtc1-weather-station/
- TC34725
  - No Kernel Driver Available, use I²C blocks in Node-RED
- Writing Industrial I/O Drivers
  - [Welcome to IIO tasks page](https://kernelnewbies.org/IIO_tasks)
  - Kernel Documentation
    - [Industrial I/O](https://www.kernel.org/doc/html/latest/driver-api/iio/index.html)
    - [Building External Modules](https://www.kernel.org/doc/Documentation/kbuild/modules.txt)
  - [2019 Simple IIO Driver](https://linux.ime.usp.br/~marcelosc/2019/09/Simple-IIO-driver)
  - [2018 The Industrial IO Subsystem after 10 Years!](https://elinux.org/images/5/53/10-Years-of-the-Industrial-IO-Kernel-Subsystem-Jonathan-Cameron-Huawei.pdf) | [Youtube](https://www.youtube.com/watch?v=644oH1FXdtE)
  - [2015 Industrial I/O Subsystem: The Home of Linux Sensors](https://events.static.linuxfound.org/sites/events/files/slides/lceu15_baluta.pdf)
