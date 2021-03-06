The following describes the function of homing cycles and related Gcode. There seems to be no standard way to do homing, and machine variations complicate matters. TinyG's homing behaviors are adapted from [Peter Smid's CNC Programming Handbook, version 2](http://books.google.com/books?id=JNnQ8r5merMC&lpg=PA444&ots=PYOFKP-WtL&dq=Smid%20version3&pg=PA447#v=onepage&q=Smid%20version3&f=false) and [EMC2](http://www.linuxcnc.org/docview/html/config_ini_homing.html). TinyG supports a subset of EMC homing and does not cover all the cases EMC does _(Note: if you are running a machine configuration that is not supported by TinyG's subset please let us know on the forum)_

Return to Home can be carried out by using the G28 and G28.1 commands:
* G28 - Return to Zero: Return to machine zero at the traverse rate through an intermediate point
* G28.1 - Reference Axes: Reset machine coordinates to the homing switches

Some limitations / constraints in TinyG homing as currently implemented:

* Limit switches and homing switches are shared. The axis minimum limit switches become homing switches during a homing cycle
* Limit switches are currently programmed for normally open (NO) mechanical switches. Support for NC and opto switches is on the roadmap
* The homing sequence is fixed, and always runs XYZABC (but skipping all axes that are not specified in the G28.1 command)
* Special consideration is provided for dual-gantry situations. These are detected by the motor mapping configuration &nbsp;(not supported yet)<br>
* Supports a single home position. I.e. it does not support multiple-homes such as used by dual pallet machines and other complex machining centers

## Homing Configuration Settings

The following per-axis settings are used by homing. Substitute any of XYZABC for the 'x', below. The use of the settings is described in G28.1, below. See [http://www.synthetos.com/wiki/index.php?title=TinyG:Configuring#Homing_Settings Configuring Homing Settings]&nbsp;for how to set these.

* **$xTM** Travel Maximum - travel limit for search phase
* **$xSV** Homing Search Velocity - velocity for initially finding the homing switch. Set negative for travel in negative direction, positive for travel in positive direction
* **$xLV** Homing Latch Velocity - velocity for homing second pass (latching phase) (same positive and negative rules apply)
* **$xLB** Homing Latch Backoff -amount to back off switch prior to latch operation
* **$xZB** Homing Zero Backoff - machine coordinate system zero position defined as backoff (offset) from homing switch
* **$xSM** Switch Mode - sets mode for limit and homing switches (0=off, 1=homing only, 2=homing and limits - in any case they are always Normally Open)

## G28 - Return to Home
G28 will move the machine to the home coordinates through an intermediate point, with the home position detemined by the latest G28.1 cycle. Movement will occur at the traverse rate (G0 rate). Format is:
<pre>G28 X0 Y0 Z0 A0 B0 C0</pre>
G28 will move to coordinates for any specified axis: axes that are not specified are ignored (not moved). The axis value is the intermediate point for that axis.

For example, G28 X10 Y0 will move to zero in the XY plane without affecting the Z or ABC axes. The movement will pass through point (10,0). Set all values to 0 for a homing move without an intermediate way point. A G28 command must have at least one axis word and is only valid if the system has been previously homed using a G30.''<br>''

## G28.1 - Reference Axes (Homing Cycle)
G28.1 is used to home to physical home switches. G28.1 will home to a switch then set the machine zero for that axis (absolute zero) at an offset from that switch location. Format is:
<pre>G28.1 X0 Y0 Z0 A0 B0 C0
</pre>
If an axis is not present in the G28.1 command then that axis is ignored and it's zero value is not changed. &nbsp;

For example. G28.1 X0 Y0 will home the X and Y axes only. The values provided for X and Y don't matter, but something must be present.

### Homing Operation
* The homing sequence progresses through each axis provided in the G28.1 block in turn - i.e. it does not home on multiple axes simultaneously.
** Axes are always executed in order of ZXYABC. The order they occur in the G28.1 block has no effect.<br>
* Homing begins by checking the pre-conditions for homing:
** $xSV homing-search-velocity must be non-zero for the axis to be processed. Set negative to travel to the minimum switch, positive to find the maximum switch.<br>
* Homing begins by testing the homing switch for that axis. If a switch is tripped the axis will back off a distance from the switch as defines in the Latch Backoff value ($xLB). Backoff will always be in the opposite direction of the search.
* Once the switch is clear a homing move of length travel max ($xTM) is executed in the negative direction of the search velocity ($xSV). The move executes until the homing switch is hit. For example: A homing move in X with X-axis velocity of -1200mm/min and 400mm travel limit would move G1 F1200 X-400 in relative coordinates. To move to the opposite end of the machine a positive velocity would be configured (and presumably the limit-max switch would be hit).
* Once the switch is hit a backoff is performed as described above.
* Once backoff occurs the latch cycle is initiated if the homing-latch-velocity is non-zero. The latch-velocity must have the same sign as the homing-search-velocity. The axis moves onto the switch at the homing-latch-rate and a backoff to $xHZ is performed at the latch rate. The homing cycle advances to the next axis to be processed.
* Once all axes are processed the affected axes are moved to the absolute home location (machine zero).&nbsp;
* At this point the homing state will indicate that the machine has been homed.&nbsp;<br>

### Switches
Limit and homing switches are shared. For the duration of the homing cycle the limit switches act as homing switches. Once homing is complete they revert to being limit switches. (Note: If a limit switch is hit outside of a homing operation it will reset the machine).

At the current time only normally open mechanical switches are supported. Support is planned for NC and opto switches, to be controlled by the $xSW parameter.<br> 
