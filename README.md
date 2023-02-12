# System for underfloor heating v2.0

The system for regulation of zone heating of family house. Proposed control system is based on Raspberry Pi with the Home Assistant system. The unit is fully controllable via web. Devices for controlling individual parts of the heating zone control and local room temperature sensors were selected or made within the system.

## Description of overall concept
There is a heating system of house in the picture. The source of heat is fireplace in cellar, on the ground floor and on the first floor. All the fireplaces have a heat exchanger. The fireplaces with heat exchanger is for heating of heating water which flows through insert of fireplace. This insert of fireplace recharges hot water tank (HWT). In the ground floor and in the first floor is distributor of underfloor heating with 12 heating circuit. Every the heating circuit is possible to control independent. There is pump and manual three-way mixing valve for settings optimal temperature into underfloor heating. The first source of heat is gas condensing boiler, which is not currently installed. This gas boiler will use for heating of heating water in the summer season (for heating of domestic hot water (DHW)), where the fireplaces will not to use. The both source of heat are for heating of heating water into central tank (volume us 1 500 l). In the upper one third of the height of the tank is installed container for DHW (volume is 120 l). The system controls pumps at the distributors of underfloor heating, the pumps for fireplaces with exchanger and drives for individual circuit of underfloor heating. If there is request for heating from a room, the pumps will be controlled. If some makes a fire in fireplace, the selected pump at the fireplace will be turn on.

<p align="center">
<img src="diagrams/drawio/png/heating-system-rust-of-house.png" width="550px" alt="Heating system in the house">
</p>
<p align="center">
Picture 1: Heating system in the house.
</p>

### Hardware part
There is hardware concept in the picture.

<p align="center">
<img src="diagrams/drawio/png/hardware-part.png" width="550px" alt="Hardware part">
</p>
<p align="center">
Picture 2: Hardware part.
</p>

Central control unit is single board computer. 

Wireless wall-mounted room temperature sensors (WRTS) are powered from local poer adapter, every module has own supply. WRTS is consisted from display for showing current temperature and required temperature and other settings. There are 3 buttons, one is for input into menu, first is for increase temperature required and last is for decrease temperature. The last part is temperature sensor. WRTS communicates with central control unit via WiFi through WiFi router.

Cable wall-mounted room temperature sensors (WRTS) are powered from switch with POE. All parts are same as Wireless WRTS. Cable WRTS communicates with central control unit via POE switch.

The status indicator is connect with central control unit. It is composite from LED for individually temperatures, which are measured from individually parts in HWT. There is bus for communication with LCD and central control unit for showing temperatures from tank. The status indicators are located at fireplace.

Switches unit is composite  from relay module for control individually pumps for circulation heating water into heating circuit of underfloor heating in individually floors. There is controlling for pumps for circulation of water from fireplace exchanger. Last control is for gas boiler.

Zone controller si located in the ground floor and in the first floor in distributor for individually heating circuit. The cone controller communicates with central control unit via bus. The zone controller control individually drives, the drives control individually heating circuits. The drives are connected directly into zone controller.

Networdk devices are composite from central switch, switch with POE abd home WiFi router. The central switch merge all communication from cable WRTSs and from wireless WRTSs.

The temperature sensors in HWT are deployed in three parts of tank (top, middle and bottom part). There are temperatures sensor in smoke flues at individually fireplaces for detection of heating in a fireplace. 

### Communication part
There is communication concept in the picture.

<p align="center">
<img src="diagrams/drawio/png/communication-part.png" width="550px" alt="Hardware part">
</p>
<p align="center">
Picture 3: Communication part.
</p>

The communication between central control unit and wireless/cable WRTS is via protocol MQTT. The central control unit receives information from individual WRTS. Some settings for WRTS is possible to change in the central control unit and sends this settings into WRTS.

The status indicators communicates with the central control unit via I2C bus for showing values on a display. Indicated LED are connect in input/output pins of the central control unit.

The switching unit is connect with the central control unit for control of pumps underfloor heating, pumps for fireplace exchangers and gas condensing boiler.

The zone controller communicates with the central control unit via I2C bus. The zone controller controls heating circuits.

The temperature sensors are located in HWT, in smoke flue of fireplaces and outdoor temperature. All sensors communicate  with the central control unit via 1-Wire bus.

