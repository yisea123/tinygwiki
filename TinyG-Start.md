If you have a TinyG v8 this is the place to start. The board revision is printed on the silkscreen at the edge of the board. If you do not have a v8 board you may want one of these links instead:

* [TinyG version 7](https://github.com/synthetos/TinyG/wiki/TinyG-Start-v7/)
* [TinyG version 6 or earlier](https://github.com/synthetos/TinyG/wiki/TinyG-Start-v6-and-Earlier)

Here's some background if you want to know [about TinyG](https://github.com/synthetos/TinyG/wiki/What-is-TinyG)<br>
Here is a list of [differences between the v7 and v8 boards](https://github.com/synthetos/TinyG/wiki/TinyG-Start#changes-in-v8-from-v7)

Here are the steps to get started. We recommend following them in order.
* [What You Need](#what-you-need)  (TinyG board, motors, power supply, fan (optional))
* [Connecting TinyG](connecting-tinyG)
* [Configuring TinyG](tinyg-configuration)

## Read This First
The getting started page is your first place to go to figure out what you need to get to get your TinyG up and running quickly. However before we dive into hooking up wires, configuring, and running Gcode files the image below is a "diagram" of the important sections / parts of your TinyG board.

![TinyG diagram version 8](http://farm3.staticflickr.com/2873/10863830183_579999a30c_o.png)

To highlight a few things in the above diagram:

* The **MOST IMPORTANT** thing to do is to wire your power input correctly. The input will take up to 30 volts, but most people use 24, 19 or 12 volt power supplies. Double check that the polarity for the GND and Vmot are correct BEFORE plugging in your TinyG board. If you have ANY doubt about the power supply output please check it with a volt meter first.
* Power output for the PC fan is important to make sure you have right! Failure to set your fan jumper may result in providing 24v to a 12v fan and maybe blowing it up. See more about this below.
* All logic input voltages are limited to 3.3v MAX!
* Start with the current setting trim pots in the middle, 6:00, straight-up-and-down position. These are single-turn trim pots that travel about 270 degrees. DO NOT over torque the trim pots!

### What You Need
Here is what you are going to need in order to use TinyG:
* [**TinyG board**](http://synthetos.myshopify.com/products/tinyg)
* [**Stepper motors**](#stepper-motors)
* [**Power supply**](#power-supply)

See also: [Stepper Motors and Power Supplies](Stepper-Motors-and-Power-Supplies)

Optional
* **Fan** - A 12VDC or 24VDC fan is recommended if the motors are pulling more than 2 amps per winding, especially if the board is in an enclosure.
* **Programmer/Debugger** You can use TinyG's [Bootloader](TinyG-Boot-Loader) for firmware updates, so you don;t need a programmer. But if you want to do real-time debugging or serious development we recommend picking up a programmer.

### TinyG Board
You can get the TinyG controller board fully assembled from the [Synthetos Store](https://synthetos.myshopify.com/products/tinyg). Details of the board are in the diagram at the beginning of this page.

### Stepper Motors
The next thing you chose (or are chosen by your machine) are the stepper motors. TinyG will work with  most NEMA17 and NEMA23 motors, and will work with both bipolar and unipolar stepper motors. This covers most small motors you are likely to encounter. It will not work with motors wired as 5 wire "star configurations", so avoid these.

We have never found a NEMA17 that would not work with TinyG, and almost every NEMA23 we have tried will work if rated up to about 3 amps per winding. The drivers on the TinyG v8 are rated to 2.5A per winding per motor, but will actually do up to 3 amps with proper cooling. Almost every NEMA17 motor we have seen draws less than 2 amps. The exceptions are some of those very long NEMA17s with torque ratings above 90 Oz-in. Most NEMA 23's we are familiar with are between 2 and 3 amps (and should therefore be fan cooled).

We also routinely run NEMA34's, but not in high mechanical load situations. The motor's rated voltage is irrelevant and can be ignored. When running NEMA23's or any motor that draws more than 2 amps we recommend fan cooling. Note that most of the heat comes off the bottom copper, so be sure to provide air circulation for the **bottom of the board** as well as the top.

Some of out favorite sources for stepper motors are:

* [Automation Technology Inc. (Keling)](http://www.automationtechnologiesinc.com/)
* [Alltronics](http://www.alltronics.com/cgi-bin/category/55)
* [MPJA](http://www.mpja.com/Stepper-Motors/products/101/ www.mpja.com/Stepper-Motors/products/101/)
* [All Electronics](http://www.allelectronics.com/make-a-store/category/400/Motors/1.html)
* [Sparkfun](https://www.sparkfun.com/categories/178)
* [Phidgets](http://www.phidgets.com/products.php?category=23)
* [Oriental Motor Company](http://www.omc-stepperonline.com/)

### Power Supply
We highly recommend a 24v-30v power supply. While 12 volt operation is possible and entirely fine, running with a 24v power supply will allow the motors to be more responsive and actually run cooler (ironically).

You might think that 4 motors at 2 amps per winding would require 4 motors * 2 windings * 2 amps = 16 amps to drive, but you'd be wrong. 4 to 4.5 amps or above will handle this as not all motors + phases are ever maxed out at the same time. At 24 volts we like to have at least 4.5 amps for NEMA17's and 6 or more is recommended for NEMA23s.

Here are a couple power supplies we like:
* [Meanwell NES-150-24](http://www.mouser.com/ProductDetail/Mean-Well/NES150-24/?qs=sGAEpiMZZMsPs3th5F8koDNPbuqd%252bfezne6r6bnnXjA%3d)
* [Meanwell NES-350-24](http://www.mouser.com/ProductDetail/Mean-Well/NES-350-24/?qs=%2fha2pyFaduhxfhzsenBkIkgMfhBr0hSVdTJWNZMLFL2wp6eI7VH7oQ%3d%3d)<br>

You can usually hunt around and find them for < $50. They also make lower amperage supplies that are cheaper.

PS Caveat: Be sure to switch the AC input from 240v to 120v if you are in the US or a 120v mains country. Most PSs will still work, but will audibly "click" and lose power if you don't set this correctly. Also, we've gotten Meanwell clones ordering from eBay - like Meigwei. Sheesh - they said I was ordering a Meanwell.

### Cooling
The main heatsinking provided for TinyG is the expanse of 2 oz. copper on the bottom and top of the board. You can see this by inspection. This is usually sufficient for NEMA17 installations and many NEMA23 applications. If you experience thermal shutdown or if you feel the chips are running too hot we recommend fan cooling.

The TI drivers on the TinyG are incredibly robust and will shut down in case of over-current instead of blowing up (unlike some other brands that shall remain nameless). But you don't want to go into thermal shutdown as it will ruin your job even though the board is still OK. Thermal shutdown is evidenced by anything from a slow on-off cycling of the motor power, getting shorter as the current raises, to a stutter in extreme cases. The chips will be quite hot to the touch.

Fan cooling is the most effective way to cool and far more effective than heatsinking. Just putting little heatsinks on the top doesn't do much good. We used to sell TinyG with these but don't any more.

The TinyGv8's come equipped with a 3-pin fan connector that can be used to power a standard 12vdc PC fan, or a 24vdc fan - depending on a jumper setting and your board voltage (Vmot). Please read the silkscreen designators as below:

* The `+12v` jumper position connects the fan header to the on-board 12 volt regulator. This should be used if you are running a 12 volt fan and your board power (Vmot) is greater than 14 volts. This is the default jumper position and your v8 should have been shipped with the jumper in this position.
* The `+Vmot` jumper position connects the fan header directly to the board power (Vmot). In this case your fan voltage **needs to be the same** as your motor voltage, be it 12 volts, 24 volts, or anything else.<br>
**Use this side with caution as applying 24 volts to some 12 volt fans will burn them out.**

### Programmer/Debugger
If you are doing serious development or want to do real-time debugging we recommend getting a programmer debugger. The unit of choice these days is the [ATMEL-ICE-BASIC programmer](http://www.digikey.com/product-detail/en/ATATMEL-ICE-BASIC/ATATMEL-ICE-BASIC-ND/4753381). The BASIC sells for about $50 and will program and debug AVRs (like the Xmega on the TinyG v8) as well as the Atmel ARM chip on the pre-release TinyG v9. Note that as of now AVRdude will not program using the ATMEL-ICE. In addition to multi-chip programming, the Atmel-ICE offers real-time debugging via Atmel Studio or Open OCD.

The older [Atmel AVRISP MKII programmer](http://www.mouser.com/Search/ProductDetail.aspx?qs=sGAEpiMZZMsaJrqdZ%252b6EWyua%252bG%2FwcOQP26MNKN%252bCIDE%3D) will also program the Xmega, and does work with AVRdude. But it doesn't do debugging and won't work with the ARM chips.

Note that older AVR ICSP programmers will not work.

### Connecting TinyG
At this point you can move on to [Connecting TinyG](Connecting-TinyG)


# Changes in v8 from v7
* LEDs moved to a single edge for better mounting / readout
* USB changed to USB-B (from mini) for better mechanical integrity
* IO connections changed from 0.100 headers to screw terminal blocks
* step/direction/enable signals broken out on headers for external stepper drivers
* logic power supply is now a switcher (as opposed to a linear) on the v7 for better thermal characteristics
* FTDI USB chip changed to FT230X from FT232R
* switch inputs have RC circuit built in for better noise rejection
* RS-485 removed and replaced with SPI connection
* transient voltage suppressors added to Vmotor and USB lines
* mounting hole pattern is slightly different to accommodate about 1/4" growth in the board
