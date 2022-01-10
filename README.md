# command8
Test Command 8 MIDI Input and Output Data

This code uses WebMidi to perform MIDI data experiments with the Digidesign Command 8. The code must run in a browser that is capable of using WebMidi. In my case I use Google Chrome on Mac OS Monterey.

## Test 1
Test 1 receives a MIDI message from the device, displays info about the message, and sends the same message back to it.
#### Button Observations
1. When I press a button, it sends a "note on" message on channel 1.
2. When I release a button, it sends a "note on" message on channel 1.
3. For buttons with LEDs, the reflected message causes the LED to turn on when pressed and turn off when released.
4. Each button appears to have a unique note number and velocity.

Here are presses down and up for Mute 1, 2, 3, 4
```
noteon 90 3 40 note:3 velocity:64
noteon 90 3 0 note:3 velocity:0
noteon 90 3 41 note:3 velocity:65
noteon 90 3 1 note:3 velocity:1
noteon 90 3 42 note:3 velocity:66
noteon 90 3 2 note:3 velocity:2
noteon 90 3 43 note:3 velocity:67
noteon 90 3 3 note:3 velocity:3
```
Here are presses down and up for Solo 1, 2, 3, 4
```
noteon 90 2 40 note:2 velocity:64
noteon 90 2 0 note:2 velocity:0
noteon 90 2 41 note:2 velocity:65
noteon 90 2 1 note:2 velocity:1
noteon 90 2 42 note:2 velocity:66
noteon 90 2 2 note:2 velocity:2
noteon 90 2 43 note:2 velocity:67
noteon 90 2 3 note:2 velocity:3
```
#### Button Conclusions
1. Button presses can be detected by looking for "note on" messages and the button can be identified with the note number and velocity.
2. Button LEDs can be turned on and off using the same "note on" messages.

#### Encoder Observations
1. Turning an encoder sends a "controller change" message on channel 1.
2. Each encoder uses a different controller number, 64-71.
3. Turning an encoder left sends a value 63.
4. Turning an encoder right sends a value 65.
5. The reflected messages do not appear to change any of the LED or LCD displays.

Here are encoders moved right then left, strips 1-8
```
controlchange b0 40 41 controller:64 value:65
controlchange b0 40 3f controller:64 value:63
controlchange b0 41 41 controller:65 value:65
controlchange b0 41 3f controller:65 value:63
controlchange b0 42 41 controller:66 value:65
controlchange b0 42 3f controller:66 value:63
controlchange b0 43 41 controller:67 value:65
controlchange b0 43 3f controller:67 value:63
controlchange b0 44 41 controller:68 value:65
controlchange b0 44 3f controller:68 value:63
controlchange b0 45 41 controller:69 value:65
controlchange b0 45 3f controller:69 value:63
controlchange b0 46 41 controller:70 value:65
controlchange b0 46 3f controller:70 value:63
controlchange b0 47 41 controller:71 value:65
controlchange b0 47 3f controller:71 value:63
```
#### Encoder Conclusions
1. Encoder changes can be detected by looking for controller messages for controllers 64-71.
2. The controller value is just one of two values and indicates the direction it is being turned. 63 is left. 65 is right.

#### Fader Observations
1. When I touch a fader, it sends a "note on" message on channel 1.
2. When I release a fader, it sends a "note on" message on channel 1.
3. When I move a fader, a "controller change" message is sent.
4. The "controller change" controller number changes a lot as a fader is moved.
5. The "controller change" value changes smoothly from 0-127 as the fader is moved from the bottom to the top.

Here is fader 1 moved from bottom to top.
```
noteon 90 5 40 note:5 velocity:64
controlchange b0 38 0 controller:56 value:0
controlchange b0 10 1 controller:16 value:1
controlchange b0 28 1 controller:40 value:1
controlchange b0 0 2 controller:0 value:2
controlchange b0 18 2 controller:24 value:2
controlchange b0 30 2 controller:48 value:2
...
controlchange b0 8 7d controller:8 value:125
controlchange b0 30 7d controller:48 value:125
controlchange b0 10 7e controller:16 value:126
controlchange b0 30 7e controller:48 value:126
controlchange b0 38 7f controller:56 value:127
noteon 90 5 0 note:5 velocity:0
```
Here is fader 1 moved from bottom.
```
noteon 90 5 41 note:5 velocity:65
controlchange b0 39 0 controller:57 value:0
controlchange b0 11 1 controller:17 value:1
controlchange b0 29 1 controller:41 value:1
controlchange b0 1 2 controller:1 value:2
controlchange b0 19 2 controller:25 value:2
controlchange b0 31 2 controller:49 value:2
controlchange b0 9 3 controller:9 value:3
controlchange b0 21 3 controller:33 value:3
controlchange b0 39 3 controller:57 value:3
```
#### Fader Conclusions
1. Fader touch messages appear to be the same as button presses.
2. The controller value represents the fader position for 7 bits of precision for all faders.
3. It's not obvious how the faders 1-8 are indicated.

