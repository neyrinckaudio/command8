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


## Test 2
Test 2 displays the message info in binary to help look for patterns.

Here are the eight faders touched and moved one at a time.
```
noteon 90 5 40 note:0000101 velocity:1000000
controlchange b0 10 28 controller:0010000 value:0101000
controlchange b0 28 28 controller:0101000 value:0101000
controlchange b0 0 29 controller:0000000 value:0101001
controlchange b0 18 29 controller:0011000 value:0101001
noteon 90 5 0 note:0000101 velocity:0000000

noteon 90 5 41 note:0000101 velocity:1000001
controlchange b0 9 24 controller:0001001 value:0100100
controlchange b0 21 24 controller:0100001 value:0100100
controlchange b0 39 24 controller:0111001 value:0100100
controlchange b0 21 24 controller:0100001 value:0100100
noteon 90 5 1 note:0000101 velocity:0000001

noteon 90 5 42 note:0000101 velocity:1000010
controlchange b0 12 25 controller:0010010 value:0100101
controlchange b0 2a 25 controller:0101010 value:0100101
controlchange b0 2 26 controller:0000010 value:0100110
controlchange b0 22 26 controller:0100010 value:0100110
controlchange b0 3a 26 controller:0111010 value:0100110
controlchange b0 1a 27 controller:0011010 value:0100111
noteon 90 5 2 note:0000101 velocity:0000010

noteon 90 5 43 note:0000101 velocity:1000011
controlchange b0 13 27 controller:0010011 value:0100111
controlchange b0 2b 27 controller:0101011 value:0100111
controlchange b0 3 28 controller:0000011 value:0101000
controlchange b0 1b 28 controller:0011011 value:0101000
controlchange b0 33 28 controller:0110011 value:0101000
noteon 90 5 3 note:0000101 velocity:0000011

noteon 90 5 44 note:0000101 velocity:1000100
controlchange b0 c 28 controller:0001100 value:0101000
controlchange b0 24 28 controller:0100100 value:0101000
controlchange b0 3c 28 controller:0111100 value:0101000
controlchange b0 14 29 controller:0010100 value:0101001
controlchange b0 2c 29 controller:0101100 value:0101001
noteon 90 5 4 note:0000101 velocity:0000100

noteon 90 5 45 note:0000101 velocity:1000101
controlchange b0 35 27 controller:0110101 value:0100111
controlchange b0 d 28 controller:0001101 value:0101000
controlchange b0 25 28 controller:0100101 value:0101000
controlchange b0 3d 28 controller:0111101 value:0101000
controlchange b0 15 29 controller:0010101 value:0101001
controlchange b0 2d 29 controller:0101101 value:0101001
controlchange b0 5 2a controller:0000101 value:0101010
noteon 90 5 5 note:0000101 velocity:0000101

noteon 90 5 46 note:0000101 velocity:1000110
controlchange b0 2e 23 controller:0101110 value:0100011
controlchange b0 6 24 controller:0000110 value:0100100
controlchange b0 1e 24 controller:0011110 value:0100100
controlchange b0 36 24 controller:0110110 value:0100100
controlchange b0 e 25 controller:0001110 value:0100101
controlchange b0 26 25 controller:0100110 value:0100101
controlchange b0 3e 25 controller:0111110 value:0100101
noteon 90 5 6 note:0000101 velocity:0000110

noteon 90 5 47 note:0000101 velocity:1000111
controlchange b0 f 29 controller:0001111 value:0101001
controlchange b0 27 29 controller:0100111 value:0101001
controlchange b0 3f 29 controller:0111111 value:0101001
controlchange b0 17 2a controller:0010111 value:0101010
controlchange b0 2f 2a controller:0101111 value:0101010
controlchange b0 7 2b controller:0000111 value:0101011
noteon 90 5 7 note:0000101 velocity:0000111
```
#### Fader Observations - 1
1. Bits 0-2 of the controller number are unique for each fader with values of 0-7.

Here is the first fader moved up slowly over a small range.
```
noteon 90 5 40 note:0000101 velocity:1000000
controlchange b0 18 2b controller:0011000 value:0101011
controlchange b0 30 2b controller:0110000 value:0101011
controlchange b0 8 2c controller:0001000 value:0101100
controlchange b0 20 2c controller:0100000 value:0101100
controlchange b0 38 2c controller:0111000 value:0101100
controlchange b0 10 2d controller:0010000 value:0101101
controlchange b0 28 2d controller:0101000 value:0101101
controlchange b0 0 2e controller:0000000 value:0101110
controlchange b0 18 2e controller:0011000 value:0101110
controlchange b0 30 2e controller:0110000 value:0101110
controlchange b0 8 2f controller:0001000 value:0101111
controlchange b0 20 2f controller:0100000 value:0101111
controlchange b0 38 2f controller:0111000 value:0101111
controlchange b0 10 30 controller:0010000 value:0110000
controlchange b0 38 2f controller:0111000 value:0101111
controlchange b0 10 30 controller:0010000 value:0110000
controlchange b0 28 30 controller:0101000 value:0110000
controlchange b0 0 31 controller:0000000 value:0110001
controlchange b0 18 31 controller:0011000 value:0110001
controlchange b0 30 31 controller:0110000 value:0110001
controlchange b0 8 32 controller:0001000 value:0110010
controlchange b0 20 32 controller:0100000 value:0110010
controlchange b0 38 32 controller:0111000 value:0110010
controlchange b0 10 33 controller:0010000 value:0110011
controlchange b0 38 32 controller:0111000 value:0110010
controlchange b0 10 33 controller:0010000 value:0110011
controlchange b0 28 33 controller:0101000 value:0110011
controlchange b0 0 34 controller:0000000 value:0110100
controlchange b0 18 34 controller:0011000 value:0110100
controlchange b0 0 34 controller:0000000 value:0110100
controlchange b0 18 34 controller:0011000 value:0110100
controlchange b0 30 34 controller:0110000 value:0110100
controlchange b0 8 35 controller:0001000 value:0110101
noteon 90 5 0 note:0000101 velocity:0000000
```
#### Fader Observations - 2
1. Bits 3-6 of the controller number change from small to larger as the value stays constant. Then when the value increases, bits 3-6 change from small to larger again. Possibly, bits 3-6 represent more precision of the fader position.
2. Because I am manually moving the fader I can't test if reflecting the message will also move the fader.

