# System for underfloor heating v2.0

The system for regulation of zone heating of family house. Proposed control system is based on Raspberry Pi with the Home Assistant system. The unit is fully controllable via web. Devices for controlling individual parts of the heating zone control and local room temperature sensors were selected or made within the system.

## Description of overall concept
There is a heating system of house in the picture. The source of heat is fireplace in cellar, on the first floor and on the second floor. All the fireplaces have a heat exchanger. The fireplaces with heat exchanger is for heating of heating water which flows through insert of fireplace. This insert of fireplace recharges hot water tank (HWT). In the first floor and in the second floor is distributor of underfloor heating with 12 heating circuit. Every the heating circuit is possible to control independent. There is pump and manual three-way mixing valve for settings optimal temperature into underfloor heating. The second source of heat is gas condensing boiler, which is not currently installed. This gas boiler will use for heating of heating water in the summer season (for heating of domestic hot water (DHW)), where the fireplaces will not to use. The both source of heat are for heating of heating water into central tank (volume us 1 500 l). In the upper one third of the height of the tank is installed container for DHW (volume is 120 l). The system controls pumps at the distributors of underfloor heating, the pumps for fireplaces with exchanger and drives for individual circuit of underfloor heating. If there is request for heating from a room, the pumps will be controlled. If some makes a fire in fireplace, the selected pump at the fireplace will be turn on.

Obrázek 3.1: Otopná soustava v domě

### Hardware part
There is hardware concept in the picture.

Obrázek 3.2: Návrh hardwarové části systému.

Central control unit is single board computer. 
