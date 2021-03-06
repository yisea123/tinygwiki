_This page currently applies to code In the Edge and Dev branches - build 407xx and later (as of 1/27/14)_ <br><br>
TinyG distinguishes between hard and soft alarms.

A hard alarm is unrecoverable and sends the machine into a shutdown state. Hard alarms are caused by hitting a limit switch or by an internal error indicating the firmware may have lost its mind (e.g. assertion failures or conditional branches that should never happen). In this case the system is considered unrecoverable and the current job is presumed lost. All position information is lost and the machine must be recovered from reset.

A soft alarm preserves machine state and may be recoverable by the host.

## Hard Alarms
### Hard Alarms in Version 0.96 and Earlier
The current system behavior for an alarm is:
* A switch configured as a limit being hit causes the board to go into an ALARM state (status code = 2).
* (Another case generating a hard alarm is an internal assertion failure such as a memory corruption or stack overflow).
* Everything is shut down immediately and the spindle direction LED flashes quickly. (Note: an assertion failure might find the system so compromised that it cannot even do this and remaining steps)
* A hard-alarm exception report is generated indicating the cause
* The system will not process any new input, or any commands queued in the planner or serial input buffers.
* The system can only be recovered by hitting RESET, sending ^x (control X) to the serial port, or power cycling.

### Hard Alarms in Version 0.97 and Later
Hard alarms still work like above but the status code is moved from 2 to 11. The '2' status code is now reserved for a soft alarm.

## Soft Alarms
Soft alarms are introduced in version 0.97. Soft alarms can occur for the following reasons
* The system determines that a move will exceed the soft limits
* An illegal Gcode command is detected
* An arc is specified in a way that cannot be executed

If a soft alarm is triggered the following happens:
* The move that caused the soft limit error returns a "soft limit exceeded" error code, or some other error code.
* A soft-alarm exception report is generated indicating the cause, and repeating the above status code
* The system will not accept any new commands until a {"clear":true} ( alt: $clear=1 ) is received. Any new commands will fail with an "Alarm violation" error code (or somesuch).
* All commands in the serial buffer will be flushed by reading and discarding them (Note: not just by killing the serial buffer). This way the clear can always be received.
* Independently of the above, the moves already in the planner queue will be run to completion. If the host does not want this behavior they can issue a feedhold and buffer flush before or after the clear command.



### (old text - deprecated)
* System halts all movement with a feedhold and preserves position at the feedhold point.
* Stops processing any commands in the planner or serial queue
* Generates an exception report indicating the type of alarm. The type is returned as a status code (2) and some displayable message text.

The interesting question is what to do next. If the serial RX buffer is empty it's easy enough to send in a command to release the alarm state (calm), but if it's not that's a different story. Running the system using queue reports can keep the buffer empty.

(a) One possible solution to a non-empty RX buffer is to read and reject all motion commands with a known error response such as "system alarmed, command rejected" until a calm command is received. This puts the burden of replay back on the controller. Status reports and other queries should be possible prior to receiving a calm.

(b) Another possibility is to add a new single character command that can jump the queue (e.g. ^Y) or overload one of the existing ones, like a feedhold (!) received during an alarm. But even this won't work if the host is jammed up in flow control on its end. Nothing TinyG can do about that.

I suggest (a) even though it's a bit messy. Some of this could be mitigated using line numbers or a machine generated "program counter" so the host knows exactly what was rejected.

Really speaks to managing the flow control using QRs and not serial-level FC.
