# System for underfloor heating v2.0

The system for regulation of zone heating of family house. Proposed control system is based on Raspberry Pi with the Home Assistant system. The unit is fully controllable via web. Devices for controlling individual parts of the heating zone control and local room temperature sensors were selected or made within the system.

This version 2.0 is based on [my diploma thesis](https://dspace.cvut.cz/handle/10467/99223). This version is fully redesigned. 

- [Czech manual](https://github.com/RomLab/system-for-underfloor-heating-v2.0/blob/main/manual/manual-cs.pdf)
- [English manual](https://github.com/RomLab/system-for-underfloor-heating-v2.0/blob/main/manual/manual-en.pdf)

I have made all devices in the workroom [MacGyver SH](https://macgyver.siliconhill.cz/) (I used measuring devices, tools and others). Mr. [bkralik](https://github.com/bkralik) helped me with the milling work of a box (the part Installation box – type 2).

## Description of the overall concept
There is a heating system of house in the picture 1. The source of heat is a fireplace in the cellar, on the ground floor and on the first floor. All the fireplaces have a heat exchanger. The fireplaces with heat exchanger are for heating of heating water which flows through the fireplace insert. This fireplace insert recharges a hot water tank (HWT). On the ground floor and on the first floor is a distributor of underfloor heating with 12 heating circuits. Each heating circuit can be controlled independently. There is a pump and a manual three-way mixing valve for settings optimal temperature for underfloor heating. The first source of heat is a heating coil. The heating coil is used for heating of heating water in the summer season (for heating of domestic hot water (DHW)), the fireplaces are not used). Both sources of heat are for heating of heating water in central tank (volume is 1 500 l). In the upper one third of the height of the tank is installed container for the DHW (volume is 120 l). The system controls pumps in the distributors of underfloor heating, the pumps for fireplaces with heat exchanger and drives for individual circuit of underfloor heating. If there is a request for heating in a room, the pumps will be controlled. If somebody makes a fire in a fireplace, the selected pump by the fireplace will be turned on.

<p align="center">
<img src="diagrams/drawio/png/heating-system-rust-of-house.png" width="550px" alt="Heating system in the house.">
</p>
<p align="center">
Picture 1: Heating system in the house.
</p>

### Hardware part
There is a hardware concept in the picture 2.

<p align="center">
<img src="diagrams/drawio/png/hardware-part.png" width="550px" alt="Hardware part.">
</p>
<p align="center">
Picture 2: The hardware concept.
</p>

The central control unit is a single board computer. 

The first is wireless wall-mounted room temperature sensors (WRTS) are powered by local power adapters, every module has its own supply. WRTS is composed of a display for showing current temperature and required temperature and other settings. There are 3 buttons. The first is for increase required temperature. The second is for input into menu and the last is for decrease temperature. The last part is a temperature sensor. The WRTS communicates with the central control unit via WiFi.

The second is cable wall-mounted room temperature sensors (WRTS) are powered by switch with POE. All parts are the same as Wireless WRTS. The cable WRTS communicates with the central control unit via a POE switch.

The status indicator is connected to the central control unit. It is composed of LEDs for individual temperatures which are measured from individual parts in HWT. There is a bus for communication with a LCD and the central control unit for showing temperatures from the tank. The status indicators are located by the fireplaces.

The switches unit is composed of relay modules for controlling individual pumps for circulation of heating water in the heating circuit of underfloor heating on individual floors. There is control for pumps for circulation of water from the fireplace exchanger. The last control is for the heating coil.

The zone controller is located in a distributor for individual heating circuits on the ground floor and on the first floor. The zone controller communicates with the central control unit via a bus. The zone controller controls individual drives. The drives control individual heating circuits. The drives are connected directly to the zone controller.

The network devices are composed of a central switch, a switch with POE and home WiFi router. The central switch merges all communication from cable WRTSs and wireless WRTSs.

The temperature sensors in HWT are located in three parts of the tank (the top part, the middle part and the bottom part). There are temperature sensors in smoke flues at individual fireplaces for detection of heating in a fireplace.

### Communication part
There is communication concept in the picture 3.

<p align="center">
<img src="diagrams/drawio/png/communication-part.png" width="550px" alt="Hardware part">
</p>
<p align="center">
Picture 3: Communication part.
</p>

The communication between the central control unit and wireless/cable WRTS is via a protocol MQTT. The central control unit receives information from individual WRTS. Some settings for WRTS are possible to change in the central control unit and sends these settings to WRTS.

The status indicators communicate with the central control unit via a I<sup>2</sup>C and show values on a display. The indication LEDs are connected to input/output pins of the central control unit.

The switching unit is connected to the central control unit for control of the pumps for underfloor heating, the pumps for fireplace exchangers and the heating coil.

The zone controller communicates with the central control unit via the I<sup>2</sup>C bus. The zone controller controls heating circuits.

The temperature sensors are located in the HWT, on the smoke flue of fireplaces and outdoor temperature. All sensors communicate with the central control unit via a 1-Wire bus.

## Hardware part
In the picture 4 is a drawing of the heating system with individual devices for controlling this system. In the text are described individual selected or designed devices. In the text is a description of WRTS. 

<p align="center">
<img src="diagrams/drawio/png/heating-system-and-electronics-rust-of-house.png" width="700px" alt="Heating system in the house">
</p>
<p align="center">
Picture 4: The Heating system in the house including control electronics.
</p>

### Central control unit Raspberry Pi
In the picture 5 is a cutout of a part from the entire drawing (the picture 4) for the central control unit. The central control unit is a Raspberry Pi model 4b.

<p align="center">
<img src="diagrams/drawio/png/cutout-of-central-control-unit.png" width="250px" alt="The coutout from the picture 4 – the central control unit.">
</p>
<p align="center">
Picture 5: The cutout from the picture 4 – the central control unit.
</p>

<p align="center">
<img src="pictures-of-final-products/central-control-unit/pcb-top.jpg" width="800px" alt="The top part of PCB. The central control unit.">
<img src="pictures-of-final-products/central-control-unit/case-top.jpg" width="800px" alt="The central control unit with a case.">
<img src="pictures-of-final-products/central-control-unit/case-rear.jpg" width="800px" alt="The central control unit with a case - rear side.">
<img src="pictures-of-final-products/central-control-unit/case-front.jpg" width="800px" alt="The central control unit with a case - front side.">
<img src="pictures-of-final-products/central-control-unit/case-left.jpg" width="800px" alt="The central control unit with a case - left side.">
<img src="pictures-of-final-products/central-control-unit/case-right.jpg" width="800px" alt="The central control unit with a case - right side.">
</p>
<p align="center">
Picture 6: The top part of a PCB of the central control unit. The central control unit with a case, rear side, front side, left side and right side.
</p>

---
 [More pictures](pictures-of-final-products/central-control-unit)

---
**PCB and 3D models:**

* [PCB in KiCAD, Gerber data, …](https://github.com/RomLab/pcb-system-for-underfloor-heating-v2.0/tree/main/central-control-unit)
* [3D models of box in FreCAD, STEP files, GCODE for Prusa's printer, …](https://github.com/RomLab/3d-model-system-for-underfloor-heating-v2.0/tree/main/electric-switchboard/central-control-unit)
---

### Temperature sensors
<p align="center">
<img src="diagrams/drawio/png/cutout-of-signalization-by-fireplace-with-temperature-sensor.png" width="250px" alt="The cutout from thr picture 4 – location of temperature sensors by a fireplace.">
</p>
<p align="center">
Picture 7: The cutout from the picture 4 – location of temperature sensors by a fireplace.
</p>

In the picutre 7 is a cutout of a part of the entire drawing (the picture 4) for location temperature sensors by a smoke flue. To obtain the temperature from the smoke flue is a thermocouple 72-21301041 type K from manufacture Güenther. The temperature range is from -100 °C to 400 °C. The thermocouple is in the picture 9.

In the picture 8 is a cutout of part of the entire drawing (the picture 4) for location temperature sensor in the HWT. To obtain the temperature from the central HWT, outdoor temperature and room temperature from individual rooms is temperature sensor DS18B20 from manufacture Maxim. The temperature range is from -55 °C to +125 °C. It is used sensors in a package TO-92 for the WRTS, for the HWT and outdoor temperature. The sensor is stored in protection package.

<p align="center">
<img src="diagrams/drawio/png/cutout-of-hot-water-tank-with-temperatures-sensor.png" width="250px" alt="The cutout from the picture 4 – location of temperature sensors in the heating water tank.">
</p>
<p align="center">
Picture 8: The cutout from the picture 4 – location of temperature sensors in the heating water tank.
</p>



<p align="center">
<img src="pictures-of-final-products/others/thermocouple-72-21301041-type-k.png" width="250px" alt="Thermocouple 72-21301041 type K.">
</p>
<p align="center">
Picture 9: Thermocouple 72-21301041 type K.
</p>

#### 1-Wire bus
The 1-Wire bus is implemented via a UTP cable category 5e. 

<p align="center">
<img src="diagrams/drawio/png/cutout-of-1-wire-bus.png" width="250px" alt="The cutout from the picture 4 – location of merge 1-Wire bus by the HWT.">
</p>
<p align="center">
Picture 10: The cutout from the picture 4 – location of a merge 1-Wire bus by the HWT.
</p>

In the picture 10 is a cutout of part of the entire drawing (the picture 4) for merge 1-Wire bus by HWT. In the picture 11 is a PCB for a temperature sensor by HWT. In the picutre 4.6b is a top part PCB which is located in an installation box. There are 6 position for fastening via a terminal block for temperature sensors. There are 3 temperature sensors connected to obtain the temperature from the top, middle and bottom part of the HWT. The location of the sensor is given by the manufacturer of the tank and sensors are inserted into a cavity. The 1-Wire bus is implemented with a UTP cable of category 5e. Pin 4 is Data, pin 5 is GND and 3 pin is supply voltage. To obtain the temperature is sensor DS18B20 in the package TO-92 which is connected to the UTP cable and covered with plastic material and a shrink protective tube. In the picture 12 are marked location with temperature sensors.

<p align="center">
<img src="pictures-of-final-products/1-wire/hub-1-wire.jpg" width="250px" alt="The merge of 1-Wire bus by the HWT.">
</p>
<p align="center">
Picture 11: The merge of the 1-Wire bus by the HWT.
</p>

<p align="center">
<img src="pictures-of-final-products/others/heating-water-tank-with-temperature-sensors.png" width="250px" alt="The HWT – the red circles indicate the location of the temperature sensors.">
</p>
<p align="center">
Picture 12: The HWT – the red circles indicate the locations of the temperature sensors.
</p>

In the picture 13 is outdoor temperature sensor. I made own protection for DS18B20. I used shrink tubings for protection of pins DS18B20 and cable, [protective varnish](https://www.gme.cz/v/1503503/kontakt-chemie-urethan-71-200ml-ochranny-lak-na-dps) for this connection, [stainless steel case](https://botland.cz/prislusenstvi-pro-teplotni-senzory/15661-kryt-teplotniho-cidla-ds18b20-lm35-6x30-mm-10-ks-5904422377922.html), [termo glue](https://www.tme.eu/cz/details/termoglue-10/lepidla/ag-termopasty/art-agt-116/) between DS18B20 and the stainless steel case, [shrink tubing with glue](https://www.hornbach.cz/p/smrstovaci-buzirka-s-lepidlem-6-2-mm-cerna/8570254/) for outdoor use and outdoor UTP.

<p align="center">
<img src="pictures-of-final-products/outdoor-temperature/outdoor-temperature-full-cable.jpg" width="250px" alt="Outdoor temperature – full cable.">
</p>
<p align="center">
<img src="pictures-of-final-products/outdoor-temperature/outdoor-temperature-top.jpg" width="250px" alt="Outdoor temperature – top.">
</p>
<p align="center">
Picture 13: The outdoor temperature sensor.
</p>

### I<sup>2</sup>C bus

The I<sup>2</sup>C bus is realised with an integrated circuit PCA9615 from manufacture NXP Semiconductors. The signals SCL and SDA are connected directly from the central control unit to input PCA9615, supply voltage is 3.3 V.  The output from PCA9615 is a differential signal. The power supply on this side is 5 V. The bus is implemented via an UTP cable category 5e, output form PCA9615 is implemented via an RJ45 connector. The UTP cable and differential transmission allow reach a long distance bus. The longest point of the bus is about 30 m. The frequency is used 100 kHz. It is therefore the full-fledged I<sup>2</sup>C bus.
The reason for choosing this variant is based on the choice of a display with an I2C bus (simple and cheap solution). It is a classic connection of the display, as if it was located at a small distance from the central control unit and it is not necessary to transfer from RS485 to UART and then to the I<sup>2</sup>C bus. The communication is defined via the I<sup>2</sup>C protocol. The first PCA9615 is located by the central control unit and other PCA9615 is located in the end of connected devices.
The power supply 5 V is implemented via a separate cable. In one UTP are the 1-Wire bus and the I<sup>2</sup>C bus - saving cables. 


## Signalization by fireplace
<p align="center">
<img src="diagrams/drawio/png/cutout-of-signalization-by-fireplace.png" width="250px" alt="The cutout from the picture 4 – signalization by the fireplace.">
</p>
<p align="center">
Picture 14: The cutout from the picture 4 – signalization by the fireplace.
</p>


In the picture 14 is a cutout of part of the entire drawing (the picture 4) for signalization of status by the fireplace. The PCB is composed of an electronic fuse TPS2600 for protection 5 V. All input/output connectors have a ESD protection (TVS diodes). The 1-Wire and the I2C bus are connected via UTP cable (connector RJ45). There are terminal blocks for LED signalization of accumulated HWT. The blue LED is for the top part of the tank, an orange LED is for the middle part of the tank and a red LED is for a bottom part of the tank.

Temperature measurement using a thermocouple and a MAX31850K converter. The temperature sensors connected to the smoke flues of the fireplace are implemented using a thermocouple. The termocouple is connected to the integrated circuit MAX31850K, value from the thermocouple is transferred to a digital value including low temperature compensation end and this value is sent via the 1-Wire bus. The thermocouple is type K.

### LCD display
For showing temperatures from the bottom, middle and top part of HWT is selected 20 characters and 4 rows LCD display with blue backlight and white letters, the picture 14. The HD44780 controller is used to control the display. The I<sup>2</sup>C expander PCF8574 is connected to the controller with eight outputs which are connected on the data bus for control respectively displaying characters on the display. Each display or PCF8574 expander allows to set a unique device address on the bus using jumpers A0, A1, A2.

<p align="center">
<img src="description-pictures/lcd-hd44780-20x4-2004a-rear.png" width="250px" alt="LCD display 20x4 with controller HD44780 - rear">
<img src="description-pictures/lcd-hd44780-20x4-2004a-top.png" width="250px" alt="LCD display 20x4 with controller HD44780 - top">
</p>
<p align="center">
Picture 15: The LCD display for displaying of temperatures from the HWT. The rear and top parts of the LCD.
</p>

### Realized PCB of signalization by fireplace

<p align="center">
<img src="pictures-of-final-products/signalization-by-fireplace/type-1/pcb-without-display-bottom.jpg" width="450px" alt="The bottom part of PCB without the LCD.">
<img src="pictures-of-final-products/signalization-by-fireplace/type-1/pcb-with-display-bottom.jpg" width="450px" alt="The bottom part of PCB with the LCD.">
<img src="pictures-of-final-products/signalization-by-fireplace/type-1/pcb-without-display-top.jpg" width="450px" alt="The top part of PCB.">
</p>
<p align="center">
Picture 16: The realised PCB of signalization by fireplace. The bottom part of PCB without the LCD, the bottom part of PCB with the LCD and the top part of PCB.
</p>

### Installation box
**Type 1**

It is used a [installation box](https://eshop.sez-cz.cz/e-shop/3597-universalni-krabice-pod-omitku-200x200x70mm?cat_id=129) (SEZ manufacturer, EAN 8585027005075) only the (orange) rear part. The top part is printed on a 3D printer.

All electronics are located in a protective installation box (the picture 17). The box includes two wires for voltage 5 V and ground, three cables for controlling the signalling LEDs, an UTP cable with the 1-Wire bus for the temperature sensor (thermocouple) and the I<sup>2</sup>C bus.

<p align="center">
<img src="pictures-of-final-products/signalization-by-fireplace/type-1/panel-with-pcb-bottom.jpg" width="450px" alt="The panel type 1 with bottom PCB.">
<img src="pictures-of-final-products/signalization-by-fireplace/type-1/panel-with-pcb-top.jpg" width="450px" alt="The panel type 1 with top PCB.">
<img src="pictures-of-final-products/signalization-by-fireplace/type-1/all-installation-1.jpg" width="450px" alt="All installation of the panel type 1 by a fireplace.">
</p>
<p align="center">
Picture 17: The panel type 1. The panel with the bottom PCB. The panel with the top PCB. The entire installation by the fireplace.
</p>

**Type 2**

It is used a [installation box](https://www.gme.cz/v/1511578/u-01-26-instalacni-krabice) (Vigan manufacturer, EAN 90317), the picture 18. The holder for the PCB top was printed on a 3D printer. This printed part is pasted on with epoxy adhesive.

<p align="center">
<img src="pictures-of-final-products/signalization-by-fireplace/type-2/panel-with-pcb-bottom.jpg" width="450px" alt="The panel type 2 with bottom PCB.">
<img src="pictures-of-final-products/signalization-by-fireplace/type-2/panel-with-pcb-top.jpg" width="450px" alt="The panel type 2 with top PCB.">
</p>
<p align="center">
Picture 18: The panel type 2. The panel with the bottom PCB. The panel with the top PCB.
</p>

---
 [More pictures](/pictures-of-final-products/signalization-by-fireplace)

---
**PCB and 3D models:**

* [PCB in KiCAD, Gerber data, …](https://github.com/RomLab/pcb-system-for-underfloor-heating-v2.0/tree/main/signalization-by-fireplace)
* [3D models of box in FreCAD, STEP files, GCODE for Prusa's printer, …](https://github.com/RomLab/3d-model-system-for-underfloor-heating-v2.0/tree/main/signalization-by-fireplace)
---

## Zone controller

<p align="center">
<img src="diagrams/drawio/png/cutout-of-zone-controller.png" width="350px" alt="Coutout from picture 4 – the zone controller">
</p>
<p align="center">
Picture 19: The cutout from the picture 4 – the zone controller.
</p>

In the picture 19 is a cutout of part of the entire drawing (the picture 4) for the zone controller. The zone controller is composited from integrated circuit PCA9615 for realisation of the I2C bus via differential pairs. The bus is implemented using category 5e UTP cable. The PCA9615 is connected to the PCA9685 from manufacturer NXP Semiconductors. The outputs from PCA9685 control individual thermoelectric drives (total of 12 drives, each controlled independently). This drives respectively valves control individual circuits. The zone regulators are located in the distributor of the heating circuits on the ground floor and on the first floor of the house.

### Realized PCB of zone controller
<p align="center">
<img src="pictures-of-final-products/zone-controller/pcb-bottom.jpg" width="450px" alt="The bottom part of PCB.">
<img src="pictures-of-final-products/zone-controller/pcb-top.jpg" width="450px" alt="The top part of PCB.">
</p>
<p align="center">
Picture 20: The realised PCB of the zone controller. The bottom part of PCB and the top part of PCB.
</p>

### Case

<p align="center">
<img src="pictures-of-final-products/zone-controller/panel-with-pcb-top.jpg" width="450px" alt=" The panel with the top PCB. ">
<img src="pictures-of-final-products/zone-controller/panel-top.jpg" width="450px" alt="The top of part panel.">
<img src="pictures-of-final-products/zone-controller/panel-top-open-protection.jpg" width="450px" alt="The top of part panel with open protection.">
<img src="pictures-of-final-products/zone-controller/panel-with-shield-top.jpg" width="450px" alt="The panel with the bottom shield."> <br>
<img src="pictures-of-final-products/zone-controller/panel-with-pcb-shield-front.jpg" width="450px" alt="The front side of the panel with the shield.">
<img src="pictures-of-final-products/zone-controller/panel-with-pcb-shield-right.jpg" width="450px" alt="The right side of the panel with the shield.">
</p>
<p align="center">
Picture 21: The panel with the top PCB. The top of part panel. The panel with the bottom shield. The front side of the panel with the shield. The right side of the panel with the shield.
</p>

### Thermoelectric drives Salus T30NC

In the picture 22 is a Salus T30NC thermoelectric drive is used for control valves for individual heating circuits. It is supplied with 24 V DC, maximum current peak when turned on is 250 mA. Operating power is 2 W. The thread size is M30 × 1.5 mm. The maximum valve stem stroke length is 4 mm. The drive force is 100 N (±10%). The opening time is approximately 2 minutes. It is a type of NC (Normally Closed). When the power is off, the valve is closed. Twelve of these drives are used for each floor.

<p align="center">
<img src="pictures-of-final-products/others/thermoelectric-drive-salus-t30nc-24v.png" width="250px" alt="Thermoelectric drive Salus T30NC.">
</p>
<p align="center">
Picture 22: The thermoelectric drive Salus T30NC.
</p>

---
 [More pictures](/pictures-of-final-products/zone-controller)

---
**PCB and 3D models:**

* [PCB in KiCAD, Gerber data, …](https://github.com/RomLab/pcb-system-for-underfloor-heating-v2.0/tree/main/zone-controller)
* [3D models of box in FreCAD, STEP files, GCODE for Prusa's printer, …](https://github.com/RomLab/3d-model-system-for-underfloor-heating-v2.0/tree/main/zone-controller)
---

## Digital corridor thermostats

<p align="center">
<img src="diagrams/drawio/png/cutout-of-local-thermostat.png" width="250px" alt="Coutout from picture 4 – the digital corridor thermostat.">
</p>
<p align="center">
Picture 23: The cutout from picture 4 – the digital corridor thermostats.
</p>

In the picture 23 is a cutout of part of the entire drawing (the picture 4) for digital corridor thermostats. To obtain temperatures from individual floors in corridors are used digital thermostat marked as W3230. The thermostat has one switching output (if heating is required, the output is switched on otherwise it is switched off). It is possible to set hysteresis, time delay, temperature calibration and maximum temperature range. It is also possible to activate a signal that is triggered when the maximum permissible temperature is reached. The power supply is 12 V DC. NTC thermistor is used to obtain temperature. The temperature range is -40 °C to 120 °C. The measurement accuracy is ± 0.1 °C. The thermostat can be replaced by another thermostat with an output relay.

## SSR relay modules
<p align="center">
<img src="diagrams/drawio/png/cutout-of-ssr-relays.png" width="350px" alt="The cutout from picture 4 – SSR relays">
</p>
<p align="center">
Picture 24: The cutout from picture 4 – SSR relays.
</p>

In the picture 24 is a cutout of part of the entire drawing (the picture 4) for SSR relays. For switching pumps by fireplaces and in the distributor of the heating circuits are used SSR relays ASR-M05DA-1 (input volatege 5–32 V DC, output voltage 24–280 V AC, max. output current 5 A, zero-cross), more info at https://www.anly.com.tw/en/productDia.php?id=146. For switching of the heating coil (6 kW, star connection) is used SSR relay SRH3-1430 (input voltage 4–30 V DC, output voltage 48–280 V AC, max. output current 30 A, zero-cross), more info at https://www.autonicsonline.com/product/product&product_id=13943&tag=SRH3-1430.

## Electric switchboard with the central control unit

In the picture 25 is a cutout of part of the entire drawing (the picture 4) for an electric switchboard with the central control unit. In the electric switchboard are located 5 V supply voltage (MDR-60-5) for power supply to the central control unit, the signalization by the fireplaces and zone controllers. The supply voltage 5 V (DR-15-5) is for the status signal for the corridor thermostats. The next supply voltage is 12 V (HDR-30-12) for powering the corridor thermostats and fans inside electric switchboard. The supply voltage 12 V (HDR-15-12) is for switching external devices (SSR relays). The power supply 24 V (S8VK-C12024) for powering the zone controllers (thermoelectric drives). There are circuit breakers for pumps and power supply. There are [breakdown list and dimensional drawing of the electric switchboard](documentation-of-electric-switchboard/export-pdf/en/breakdown-list-and-dimensional-drawing.PDF) and [single pole diagram of the electric switchboard](documentation-of-electric-switchboard/export-pdf/en/single-pole-diagram.pdf) (It was used software from Eaton E-config.).

<p align="center">
<img src="diagrams/drawio/png/cutout-of-electric-switchboard.png" width="450px" alt="The cutout from picture 4 – the electric switchboard">
</p>
<p align="center">
Picture 26: The cutout from picture 4 – the electric switchboard.
</p>

<p align="center">
<img src="pictures-of-final-products/electric-switchboard/electric-switchboard-without-case-of-central-control-unit.jpg" width="450px" alt="The final electric switchboard without case.">
<img src="pictures-of-final-products/electric-switchboard/electric-switchboard-with-case-of-central-control-unit-3.jpg" width="450px" alt="The final electric switchboard with case.">
</p>
<p align="center">
Picture 27: The final electric switchboard.
</p>

## Wall-mounted room temperature sensor

To obtain room temperature is the WRST. These devices serve primarily to measure temperature and send it to the central control unit. There are buttons for settings required temperature (step change is 0.5 °C) for a selected room. On a display is showing currently measured and required temperature. If somebody makes changes in the central system, these changes will transfer to the WRST. The WRST measures temperature in the room every 30 seconds. If the WRST disconnects from network connection, the device will attempt reconnection. This network failure is displayed in the central control unit. The first option for communication with the central control unit is via Ethernet and the end devices are powered by POE. The second option for communication with the central control unit is via WiFi and the end devices are powered by a power adapter. There are 6 devices with Ethernet and 5 devices with WiFi.

### Variant with Ethernet

<p align="center">
<img src="diagrams/drawio/png/cutout-of-wall-mounted-room-temperature-sensor-ethernet.png" width="450px" alt="The cutout from the  picture 4 – the wall mounted room temperature sensor – Ethernet.">
</p>
<p align="center">
Picture 28: The cutout from the picture 4 – the wall mounted room temperature sensor – Ethernet.
</p>

In the picture 28 is a cutout of part of the entire drawing (the picture 4) for WRST (variant with Ethernet). In the picture 27 is a block diagram of the NSPT where it is communication via Ethernet and the end devices are powered by POE. The device is powered by PSE (Power Sourcing Equipment) - POE switch MaxLink PSAT-10-8P-250, the WRST is PD (Powered Device). These PSE and PD devices support standard 802.3af respectively 802.3at. The devices PD are set for the lowest power class 1 (max. power PSE for PD devices is 4 W). For transmission is used Phantom voltage. The input voltage from PSE (44-57 V depending on the length of the UTP cable and loss) passes through a diode rectifier. There is control circuit TPS23753A which provides communication/interface for correct settings and enables voltage from the PSE. It also provides control of the conversion of input voltage to output voltage 5 V (DC-DC converter). It is connected in the Flyback topology. The feedback is solved using optical feedback with an adjustable Zener diode TLV431A in the comparator connection.

The device in operation is primarily powered by 5 V, in the case of programming the device, it is possible to use a programming connector with a power pin for 5 V. If POE is available, power from the programming connector will be blocked (using a P-channel MOSFET). The 5 V voltage is conducted to two LDO (Low-dropout regulator) regulators. The one LDO serves only to power the ESP-32-WROVER-IE (M213EH2864UH3Q0) module, the second LDO is to power the remaining peripherals (display, buttons, temperature sensor, circuit for the physical layer of Ethernet W5500).

For programming the module, there is a connector for connecting an external module, where there are pins for the TX/RX signal from the UART and signals for automatic reset and boot of the module, and pins for the 5 V power supply and ground. In addition, there are buttons for boot and reset of the ESP32 module located directly on the PCB, without depending on the connection of the programming module (so other modules that do not have automatic reset and boot can also be programmed).

The device has protective transils on the connectors and parts which are in direct contact with the user. The circuit for POE has current and temperature protection. The LDO regulators have low input voltage detection for successful start-up, a thermal fuse and protection when the output voltage increases compared to the input voltage.  

There is a colour TFT display of size 2.2" (240×320 pixels) with an ILI9341 controller for displaying the current and required temperature. The display is connected to the ESP32 module using the SPI bus. The display has the option of controlling the backlight using PWM. The W5500 circuit is used for the physical layer which implements an Ethernet controller with integrated TCP/IP. The circuit is connected to the ESP32 module using the SPI bus. To obtain room temperature is used temperature sensor DS18B20. There are three buttons for setting the required temperature and showing the menu.

The ESP32 module has an RMII (Reduced Media-Independent Interface) interface which has a more complex software implementation and uses a larger number of pins. Therefore, the integrated circuit W5500 is chosen. Two independent SPI buses are used for communication. The first is between the ESP32 module and the W5500. The second is between the ESP32 module and the display.

In the picture 30 is the top part of the realised PCB for the WRST, the bottom part of PCB and the PCB with a display. 

<p align="center">
<img src="diagrams/drawio/png/block-diagram-of-wall-mounted-room-temperature-sensor-ethernet.png" width="550px" alt="The cutout from the picture 4 – the block diagram of wall-mounted room temperature sensor – Ethernet">
</p>
<p align="center">
Picture 29: The block diagram of a wall-mounted room temperature sensor – Ethernet.
</p>

<p align="center">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/ethernet/pcb-bottom.png" width="465px" alt="The PCB bottom part.">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/ethernet/pcb-top.png" width="450px" alt="The PCB top part.">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/ethernet/pcb-top-with-display.png" width="450px" alt="The PCB top part with display."> <br>
</p>
<p align="center">
Picture 30: The realised PCB of the wall mounted room temperature sensor – Ethernet. The PCB bottom part. The PCB top part. The PCB top part with a display.
</p>

### Variant with WiFi

<p align="center">
<img src="diagrams/drawio/png/cutout-of-wall-mounted-room-temperature-sensor-wifi.png" width="450px" alt="The cutout from the  picture 4 – the wall mounted room temperature sensor – WiFi.">
</p>
<p align="center">
Picture 31: The cutout from the picture 4 – the wall mounted room temperature sensor – WiFi.
</p>

In the picture 31 is cutout of a part of the entire drawing (the picture 4) for WRST (variant with WiFi). In the picture 32 is a block diagram of the WRST communicates via WiFi and is powered by a power adapter (Mean Well GSM06E05-P1J). This variant does not have power supply by POE and the circuit W5500 has not implemented Ethernet communication as in the variant above. There is used external the antenna [2JF0102P-010MC137-UFL_ TRALO](https://www.tme.eu/cz/details/f0102p010mc137ufl/anteny-wifi-bluetooth/2j/2jf0102p-010mc137-ufl-tralo/)

The top part of realised PCB is shown in the picture 33 for the WRST with WiFi. The bottom part of realised PCB is shown in the picture 33. In the picture 33 is PCB with the display. 

<p align="center">
<img src="diagrams/drawio/png/block-diagram-of-wall-mounted-room-temperature-sensor-wifi.png" width="550px" alt="The cutout from the picture 4 – the block diagram of wall-mounted room temperature sensor – WiFi">
</p>
<p align="center">
Picture 32: The block diagram of wall-mounted room temperature sensor – WiFi.
</p>

<p align="center">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/wifi/pcb-bottom.png" width="465px" alt="The PCB bottom part.">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/wifi/pcb-top.png" width="450px" alt="The PCB top part.">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/wifi/pcb-top-with-display.png" width="450px" alt="The PCB top part with display."> <br>
</p>
<p align="center">
Picture 33: The realized PCB of the wall mounted room temperature sensor – WiFi. The PCB bottom part. The PCB top part. The PCB top part with display.
</p>

#### Box for the wall-mounted room temperature sensor
The box for the WRST is printed on the 3D printer Prusa i3 MK3s (the picture 34). The plastic is PET-G. The box has a size of 130 × 99 × 26 mm. The box for both the version with Ethernet and for WiFi is the same, it differs only in the connector for RJ45 and the DC connector for the adapter.

<p align="center">
<img src="pictures-of-final-products/wall-mounted-room-temperature-sensor/wall-mounted-room-temperature-sensor-with-case.png" width="450px" alt="The Box for the wall-mounted room temperature sensor.">
</p>
<p align="center">
Picture 34: The Box for the wall-mounted room temperature sensor.
</p>

---
 [More pictures](pictures-of-final-products/wall-mounted-room-temperature-sensor)

---
**PCB and 3D models:**

* [Ethernet – PCB in KiCAD, Gerber data, …](https://github.com/RomLab/pcb-system-for-underfloor-heating-v2.0/tree/main/wall-mounted-room-temperature%20sensor-ethernet)
* [Wifi – PCB in KiCAD, Gerber data, …](https://github.com/RomLab/pcb-system-for-underfloor-heating-v2.0/tree/main/wall-mounted-room-temperature%20sensor-wifi)
* [3D models of box in FreCAD, STEP files, GCODE for Prusa's printer, …](https://github.com/RomLab/3d-model-system-for-underfloor-heating-v2.0/tree/main/wall-mounted-room-temperature-sensor)
---

## Converter USB-UART CP2102N

For programming the WRST, modul ESP32 Wrover-IE is used converter USB-UART CP2102N. It is used a module CP2102N MINEK (the picture 35). The module is supplemented with a transistor connection for automatic reset and automatic boot of the module (the picture 35). The DTR (Data Terminal Ready) and RTS (Request To Send) signals are used by the module. If it needs to enter the bootloader to upload new firmware, it is necessary to hold boot and then press reset, the device is thus ready to upload new firmware. From the module are brought out to connectors 5 V, GND, RXD, TXD, EN and IO0. Communication between the CP2102N and the ESP32 is via the RXD and TXD wires.

<p align="center">
<img src="pictures-of-final-products/converter-usb-uart-cp2102n/converter-usb-uart-cp2102n-bottom.png" width="250px" alt="The PCB bottom part.">
<img src="pictures-of-final-products/converter-usb-uart-cp2102n/converter-usb-uart-cp2102n-top.png" width="250" alt="The PCB top part.">
</p>
<p align="center">
Picture 35: The converter USB-UART CP2102N. The bottom part of converter. The top part of converter. 
</p>

## Software part

### Wall-mounted room temperature sensor

The WRST still checks that it is connected to network (it is a connected cable or available WiFi). If it is not connected, it tries to reconnect. The connection status is indicated to the user by an icon in the left corner (green color of the icon indicates a successful connection status, red color indicates a connection problem). The device checks the connection to the MQTT broker (Mosquitto broker), similarly to the network connection, the device tries to restore the connection automatically. The status is again signalled using the icon in the left corner. The current measured temperature is shown in red on the display (measured every 30 seconds), the required temperature is shown in green. The user can increment the temperature by +0.5 °C with the right button, the left button decrements it by -0.5 °C. The middle button has not implemented function yet. It will be for next setting, for example hysteresis. The last line with white text is used to display a message to the user. Currently, it shows a request for flooding in a fireplace. The individual parts described above are shown in the picture 34.

<p align="center">
<img src="description-pictures/software/wall-mounted-temperature-sensor-with-description.png" width="550px" alt="Wall-mounted room temperature sensor – software.">
<p align="center">
Picture 36: Wall-mounted room temperature sensor – software. 
</p>

### Home Assistant - Types of heating control
Within the control system, there are the following control types:
- Heating control according to corridor thermostats.
- Heating control according to wall room temperature sensors.
- Heating control according to temperature plans.

It is assumed that the central the HWT is continuously heated during a day using excess energy through heat exchangers. The central HWT is reheated for any heating needs. Priority is given to obtaining heated water from the heat source mentioned earlier. Users are alerted by signals on the displays both at the fireplaces (the picture 16) and at the WRST (the picture 34). Directly in the control system (notifications to the mobile phone (the picture 37), e-mail are also possible) or by LEDs (lighting of all) by fireplaces, there is a need to flood the fireplaces, if the system evaluates that there is a need for heating. If this does not happen, the heating coil is used, which reheats the HWT (it can be controlled automatically). 

<p align="center">
<img src="description-pictures/software/notification.png" width="400px" alt="Notification for users.">
<p align="center">
Picture 37: Notification for users. 
</p>

The interface of Home Assistant for heating settings is shown in the picture 38. In the left menu are individual floors with thermostats and temperature schedules (described below). In the records tab, history is saved to a database, individual states of the control elements and the history of the data itself, especially of the temperature sensors. There are also settings for the user profile and the entire system. In the top menu are other tabs for heating settings, also described below in the text.

<p align="center">
<img src="description-pictures/software/interface-home-assistant-for-settings.png" width="1024px" alt="Interface Home Assistant for settings of heating">
<p align="center">
Picture 38: Interface Home Assistant for settings of heating.
</p>

Current temperatures are displayed in the overview tab (picture 39), which are used for evaluation in the system Home Assistant. In the section "individual temperatures" are all temperatures measured in the HWT, temperatures on flues in the ground and first floor and outdoor temperature. Temperatures are displayed in a single graph in the section "temperature comparison."

<p align="center">
<img src="description-pictures/software/current-temperatures.png" width="400px" alt="The current temperatures which are used for evaluation in the system Home Assistant.">
<p align="center">
Picture 39: The current temperatures which are used for evaluation in the system Home Assistant.
</p>

In the settings tab (the picture 40) is possible to select one type of heating control in the "temperature control" section. In the "control modes" is a selection of modes - winter, summer or selection according to outdoor temperature. The choice of mode has affect on selection limit temperature for switching the heating coil. Temperature limits can be set in the section "switching of heating coil" (temperature limits for summer and winter). These set limits are used to control the temperature in the upper part of the HWT.

If the temperature in the top part of the HWT is lower than the temperature defined in the part "min. turn on", the heating coil will be switched on to heat the heating water. The boiler switches off at the temperature defined in "max. turn off". When comparing temperature is used hysteresis in the section "other settings". In the mode according to outdoor temperature is selected automatic summer or winter mode. The temperature limit for selection of the summer mode (within the mode according to the outdoor temperature) is defined in the section "min. outdoor temperature for the summer mode". If after warning of users, it won't be flooding in the boiler. The boiler will automatically turn on.

In the section of settings "fireplaces - switching of pump" is defined min. temperature limit when the circulation pumps for heat exchangers of fireplaces are switched on. In case of flooding in the fireplace, the pumps must be started, otherwise water in the heat exchanger will overheat. If overheating occurs, the protection is activated by the fireplaces and an audible alarm will sound.

In the section "LED indication - limit parameters of the heating water tank" are defined the limit temperatures for the top, middle and bottom part of the HWT. This indication is for users to signalization of heating of the HWT. The blue LED defines the minimum temperature that the tank should have in the top part. The orange LED defines the maximum limit temperature when the middle part of the HWT is enough heating. The red LED defines maximum temperature when the bottom part of the HWT is fully heated. Activation of the red LED is before activating of protection of the fireplace.

<p align="center">
<img src="description-pictures/software/tab-settings.png" width="1024px" alt="Tab settings.">
<p align="center">
Picture 40: Tab settings. 
</p>

In the device tab (the picture 41) is shown individual control (turn on/turn off) devices of the heating system - the heating coil, pumps for fireplaces, pumps for underfloor heating and signalization LEDs. The switch "manual device control" is for individual control of devices regardless of automation.

In the section "corridor thermostats - required heating" is shown heating in the ground or in the first floor according to corridor thermostats.

In the section "floor/ground floor - heating circuits (valves)" is shown status of each valve.

<p align="center">
<img src="description-pictures/software/tab-devices.png" width="1024px" alt="Tab devices.">
<p align="center">
Picture 41: Tab devices. 
</p>

In another tab in the part "control of pumps – limescale"  (the picture 42) is used to switch pumps for protection before stiffening shoulder blades. If the pumps are not used for long time, the vanes will become stiff.

<p align="center">
<img src="description-pictures/software/tab-other.png" width="400px" alt="Control of pumps - limescale.">
<p align="center">
Picture 42: Control of pumps – limescale.
</p>

### Heating control according to corridor thermostats

In the ground and first floor are corridor thermostats. This thermostat controls independent on settings in the central control unit turn on/turn off a output relay when it is need to heating. This request is subsequently evaluated in the central system and then the  pump for underfloor heating is turned on or turned off and all underfloor heating circuits are opened. All rooms are heated same according to one thermostat.

### Heating control according to wall room temperature sensors

The given heating circuit is controlled by the current temperature measured from each room. The required temperature is possible to set on the WRST or in the system (the picture 43). The changes are reflected in each other. Room heating control is given by a hysteresis of 0.5 °C. The heating control responds to the current measured temperature.

<p align="center">
<img src="description-pictures/software/thermostat-bathroom.png" width="400px" alt="Thermostat.">
<p align="center">
Picture 43: Thermostat. 
</p>

Local thermostats are sorted to groups according to a given floor (the ground floor or the first floor), the picture 44.

<p align="center">
<img src="description-pictures/software/thermostats.png" width="1024px" alt="Thermostats.">
<p align="center">
Picture 44: Thermostats. 
</p>

Each the thermostat has an indication of network connection to the central control unit. Verification takes based on sending the current time. There is a detection of an open window in a room.

### Heating control according to temperature plans

The heating is controlled according to defined schedules. The user has the option to define time periods with the required temperature for each room for all 24 hours. The time schedules set are continuously checked by the system and the system sets the currently required temperature to local WRSTs. This temperature is shown on the HA thermostat. The interface for setting intervals is in the picture 41. The user can add individual intervals or remove them. The user can choose whether intervals are applied to all days of the week or just working days, the weekend or a selection of specific days of the week. It is possible to define whether the given section should be heated or not.

<p align="center">
<img src="description-pictures/software/temperature-plan-settings.png" width="400px" alt="The interface for settings of intervals.">
<p align="center">
Picture 45: The interface for settings of intervals. 
</p>

For each room is possible to define an individual count of intervals. The overview of individual plans is displayed under each day, the picture 46. 

<p align="center">
<img src="description-pictures/software/more-temperature-plans.png" width="400px" alt="Individual count of intervals.">
<p align="center">
Picture 46: Individual count of intervals. 
</p>

Individual plans can be paused using the slider button on the right. The general overview of the temperature plans of all rooms for the first floor is possible to see in the picture 47, similar overview is for the ground floor.

<p align="center">
<img src="description-pictures/software/all-temperature-plans-for-floor.png" width="1024px" alt="Overview of the temperature plans of all rooms.">
<p align="center">
Picture 47: Overview of the temperature plans of all rooms. 
</p>

### Recharging of domestic hot water
 
Recharging (of the domestic hot water) is similar to heating according to temperature plans. There is one temperature plan for recharging DHW. There is a comparison of the required temperature with the current temperature in the top part of the HWT. If temperature is low, the system will alert users to flood the fireplace. If they do not do so within a certain time, the heating coil will automatically turn on and heat the top part of the HWT.
</p>

### Heating control according to temperature plans with adjustment according to the weather forecast
Description  is in preparation.

---
**Software + settings:**

* [Software (scripts) Home Assistant + AppDaemon](https://github.com/RomLab/home-assistant-for-underfloor-heating-docker-v2.0)
* [Software for the wall-mounted room temperature sensors](https://github.com/RomLab/esp32-local-thermostat)
---
