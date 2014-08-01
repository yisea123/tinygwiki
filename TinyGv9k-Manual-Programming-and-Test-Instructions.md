##Setup
The assembled board should be set up with the following:
* Atmel-ICE programmer
* Bench power supply - set to 24volts, current limit about 1.5 amps - OBSERVE POLARITY WHEN CONNECTING TO BOARD
* Four stepper motors
* USB connected to host computer

![setup](https://farm4.staticflickr.com/3910/14770638616_fa3c1c8794_b.jpg)
![power](https://farm4.staticflickr.com/3902/14791273484_149bdaa802_b.jpg)

The Atmel-ICE should be plugged into the SAM port
![atmel-ice](https://farm3.staticflickr.com/2912/14813475953_7781856e74_b.jpg)

The JTAG (SWD) connector needs to be properly seated. It's all too easy to plug this connector into only one row of pins.
![jtag-connector](https://farm3.staticflickr.com/2927/14607120307_1fdab4157f_b.jpg)

The motor connectors should plug in as shown. The 5th pin (grounding pin) is left unconnected.
![motor-connectors](https://farm4.staticflickr.com/3898/14606999538_19c8b88de2_b.jpg)

The blue power light should light when the power supply is turned on.

##Programming
Start Atmel Studio 6.2
![studio6.2](https://farm4.staticflickr.com/3847/14790500471_6c7aba38db_b.jpg)

Select /Tools/Device Programming 
![device-programming](https://farm4.staticflickr.com/3902/14606994178_5385b2c3fe_b.jpg)

Select the Atmel ICE. Select the device to be ATSAM3X8C. Select SWD programming mode. Hit APPLY.
![setup-programming](https://farm6.staticflickr.com/5596/14793276122_775356456f_b.jpg)