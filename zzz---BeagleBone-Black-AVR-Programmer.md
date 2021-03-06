Consider these build notes:

### Circuit

    var redPin = "P8_12";
    var greenPin = "P8_11";
    var inputPin = "P8_14";

Wire the Red LED from P8-12 to GND (pins P8 1 and 2 are GND).

Wire the Green LED from P8-11 to GND.

Wire the button between GND and P8-14, with a pullup resistor to 3v3 (pins P8 3 and 4 are 3v3.)


### Avrdude
Here are a few places to find Avrdude for different platforms.
* https://github.com/arduino/Arduino/blob/master/build/macosx/dist/tools-universal.zip
* https://github.com/arduino/Arduino/tree/master/build/linux/dist/tools
* https://github.com/arduino/Arduino/tree/master/build/windows
* You can also find Avrdude in the Arduino application download. Avrdude is buried in the latest Arduino package, version 1.5.7. Unzip the app and right click to show package contents (these are OSX instructions, other platforms may be different). You can find Avrdude in Contents/Resources/Java/hardware/tools/avr/bin/avrdude<br>

Run these command in a terminal _from your Mac_ (Note that this assumed Dropbox is running and synced):

    scp ~/Dropbox/Synthetos/avrdude-5.11.1-BBB.tgz root@beaglebone.local:
    scp ~/Dropbox/Synthetos/bbb-programmer.tgz root@beaglebone.local:
    
    ssh root@beaglebone.local

_Note: bbb-programmer.tgz has a altered copy of `/usr/lib/node_modules/bonescript/index.js` in it. If bonescript is updated, that may cause problems._

Now you should be connected to the beaglebone, the rest of the commands in that terminal will go to the BBB:

    cd /
    tar xzvf ~/avrdude-5.11.1-BBB.tgz
    tar xzvf ~/bbb-programmer.tgz
    opkg update
    opkg install kernel-module-ftdi-sio
    opkg install libftdi-dev

Now you should be able to go to [Cloud9](http://beaglebone.local:3000/), open `programmer.js` and run it.

Once you've verified proper operation, to deploy the BB as a programmer, move the `programmer.js` script into the `autorun` folder. You can do that with Cloud9 or in the terminal with this command:

    mv /var/lib/cloud9/{programmer.js,autorun/}

###Changing the hostname

Run this command on the BBB comand line (sss'd in) to change the hostname to `bbb-prog`:

    perl -pi -e 's/beaglebone/bbb-prog/' /etc/host{name,s}

You can change `bbb-prog` to another name is you wish, as long as it's a valid hostname. (No spaces, mostly.)

**Warning:** This will change the hostname to `bbb-prog` after reset or reboot of the BBB. To log in from this point on, the new ssh command is:

   ssh root@bbb-prog.local

All `scp` command will change `beaglebone.local` to `bbb-prog.local` as well.

## Making AVRDude

_Please use the precompiled binary in Dropbox. There are the instructions needed to build it._

**To be clear, if you use the binary from Dropbox, you do not need to follow these instructions.**

Notes: https://github.com/todbot/ArduinoOnBeagleBone

Prerequisites:

    opkg install libftdi-dev
    #opkg install libusb-1.0-dev

Download and install:

    wget http://download.savannah.gnu.org/releases/avrdude/avrdude-5.11.1.tar.gz
    tar xavf avrdude-5.11.1.tar.gz
    cd avrdude-5.11.1
    ./configure

To get bison working (maybe):

    mkdir -p /build/v2012.12/build/tmp-angstrom_v2012_12-eglibc/sysroots/x86_64-linux/usr/bin/
    ln -s /usr/bin/m4 /build/v2012.12/build/tmp-angstrom_v2012_12-eglibc/sysroots/x86_64-linux/usr/bin/m4

Then you can make:

    make


## Manually calling avrdude

If you wish to just call `avrdude` directly, you can use the following command line:

```bash
export TINYG=path_to_tinyg.hex
export XBOOT=path_to_xboot.hex
avrdude -q -c avrisp2 -p atxmega192a3 -P usb -u -U flash:w:${TINYG} -U boot:w:${XBOOT} -U fuse0:w:0xFF:m -U fuse1:w:0x00:m -U fuse2:w:0xBE:m -U fuse4:w:0xFE:m -U fuse5:w:0xEB:m
```

**WARNING**: This assumes the fuses are the same as shown. Double check them!

This also assumes you're using an AVR ISP MkII. Adjust the `-c avrisp2` as appropriate.
