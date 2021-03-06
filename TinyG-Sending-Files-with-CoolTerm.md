### Summary:
If you want a no frill easy to setup and send files quickly sort of solution then CoolTerm is a good choice.<br>If you followed the instructions in the connecting and tuning sections of the wiki then you already have it install too.
### Pros:
* Very easy to start sending files quickly.
* Cross platform.

### Cons:
* [Asynchronous Commands](https://github.com/synthetos/TinyG/wiki/JSON-Flow-Control-Specification#async-commands) are not available during a file send. 
* No GUI Preview.
* If you cancel a job in progress, unsent code in the send buffer could crash your machine. See below.

Summary:
##

1. Make sure you have CoolTerm installed and you are able to connect to TinyG.  See [Connecting to TinyG](https://github.com/synthetos/TinyG/wiki/Connecting-TinyG#establish-usb-connection) for complete instructions on how connected to TinyG with CoolTerm.<br>
2. If you are now connected to TinyG with CoolTerm, disconnect.  We need to change one value in the <code>Options</code> of Coolterm **BEFORE** we connect to TinyG.  Now that you have clicked the options button you should see something like this:<p>
![CoolTerm Connect](https://farm8.staticflickr.com/7471/15846337390_23d505b552_o_d.png)
<p>Notice that the <code>CTS</code> is checked.  **This is VERY IMPORTANT.  Please make sure CoolTerm has <code>CTS</code> checked!**<p>

3. Once you are done checking CTS go ahead and click <code>OK</code>.  Now click the <code>Connect</code> button.  As long as you have the right port selected and the right baud rate set, you should now be connected to TinyG.<br>
4.  Technically, we could send a file to TinyG now.  However, we need to make sure <code>CTS</code> is enabled on TinyG.  It IS enabled by default, but it is always good to make sure before sending files that will depend on <code>CTS</code>.  To check this you will need to click in the text input box in CoolTerm.  It is at the very bottom of CoolTerm.  Once you are able to type into the input box issue a CTS query by sending: <br><code>$ex</code> then hit enter.  You just issued getter command in TinyG for the CTS enabled property.  If <code>CTS</code> is enabled, you should see something like this:<br>
$ex 2 - Paste this in.
5.  Now you are ready to send a gcode file.  Just to re-cap... <br>
**You are sure you....<p>**

  * Checked to make sure CoolTerm was connected with <code>CTS</code> checked.
  * Checked to make sure TinyG has <code>CTS</code> enabled on the hardware.
<p>
Now to send a file (assuming your tool is at the desired X,Y,Z coordinates) click on the <code>Connection</code> menu and then select the "Send Text File".  From here you will be asked to select a text file. <p>**WARNING:** when you click the selected file and hit open you file is being send to TinyG instantly!<p>

### About Canceling
Be cautious about canceling a gcode file you have sent to TinyG. You may want to cancel, for example, to change a broken bit or to modify your gcode. When you press 'cancel' there might be a partial command still in the send buffer.  When you press return, either alone or with a new command, you will add the new command to what is already in the send buffer, and CoolTerm will send it all. This has some chance of crashing your machine.  (For example, say your TinyG is routing a hole at Y299X33, and you send Y299X32, but part-way through you hit cancel. If CoollTerm only sends "Y2" (missing the last 99X32) then your machine could crash as it tries to get to unexpectedly go from Y299 to Y2.)<p>

The "connection/flush serial port" ought to solve this problem, but does not. Until a solution is found, the best practice is to hit cancel, then issue a "return" with your hand poised by the emergency stop.<p>

### **NEED TO FINISH**
