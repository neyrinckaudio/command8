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

## Test 3
Test 3 displays the controller number as two parts: bits 3-6 and bits 0-2. Also, when reflecting the controller message back to the device, the controller number is increased by one.

Here I move fader 1 a bit.
```
noteon 90 5 40 note:0000101 velocity:1000000
controlchange b0 20 12 controller:0100 000 value:0010010
controlchange b0 38 12 controller:0111 000 value:0010010
controlchange b0 10 13 controller:0010 000 value:0010011
controlchange b0 30 13 controller:0110 000 value:0010011
controlchange b0 8 14 controller:0001 000 value:0010100
controlchange b0 20 14 controller:0100 000 value:0010100
controlchange b0 0 15 controller:0000 000 value:0010101
controlchange b0 20 15 controller:0100 000 value:0010101
controlchange b0 0 16 controller:0000 000 value:0010110
controlchange b0 18 16 controller:0011 000 value:0010110
controlchange b0 30 16 controller:0110 000 value:0010110
controlchange b0 8 17 controller:0001 000 value:0010111
controlchange b0 20 17 controller:0100 000 value:0010111
controlchange b0 38 17 controller:0111 000 value:0010111
controlchange b0 18 18 controller:0011 000 value:0011000
controlchange b0 30 18 controller:0110 000 value:0011000
controlchange b0 8 19 controller:0001 000 value:0011001
controlchange b0 28 19 controller:0101 000 value:0011001
controlchange b0 10 1a controller:0010 000 value:0011010
controlchange b0 28 1a controller:0101 000 value:0011010
controlchange b0 8 1b controller:0001 000 value:0011011
controlchange b0 30 1b controller:0110 000 value:0011011
controlchange b0 8 1c controller:0001 000 value:0011100
controlchange b0 28 1c controller:0101 000 value:0011100
controlchange b0 0 1d controller:0000 000 value:0011101
controlchange b0 20 1d controller:0100 000 value:0011101
controlchange b0 38 1d controller:0111 000 value:0011101
controlchange b0 10 1e controller:0010 000 value:0011110
controlchange b0 28 1e controller:0101 000 value:0011110
controlchange b0 0 1f controller:0000 000 value:0011111
controlchange b0 18 1f controller:0011 000 value:0011111
controlchange b0 30 1f controller:0110 000 value:0011111
controlchange b0 8 20 controller:0001 000 value:0100000
controlchange b0 20 20 controller:0100 000 value:0100000
controlchange b0 38 20 controller:0111 000 value:0100000
controlchange b0 10 21 controller:0010 000 value:0100001
controlchange b0 28 21 controller:0101 000 value:0100001
controlchange b0 0 22 controller:0000 000 value:0100010
controlchange b0 18 22 controller:0011 000 value:0100010
controlchange b0 30 22 controller:0110 000 value:0100010
controlchange b0 8 23 controller:0001 000 value:0100011
controlchange b0 20 23 controller:0100 000 value:0100011
noteon 90 5 0 note:0000101 velocity:0000000
```
#### Fader Observations
1. When I move a fader, the fader after it moves acccordingly. That is, moving fader 1 makes fader 2 move, and so on, up to moving fader 8 which makes fader 1 move. It is good evidence that bits 0-2 indicate the fader.
2. Bits 3-6 never got a value larger than 7. That is, bit 6 is always zero. If these bits add fader precision, then there are just three bits that add to the seven bits encoded in the value. That implies a max fader resolution of of 10-bits.

## Test 4
Test 4 sends every possible "note on" to the device on channel 1. It sends at a rate of one per 10 ms. Then I just observe what happens on the device.

#### Test 4 Observations
1. The first time I ran the test, I noticed that the program got bogged down at some point. The device was sending a message which was being reflected which caused the device to send a message and so forth in a loop. So, I disabled message reflecting.
2. With message reflecting disabled I ran the test again and observed device LEDs turning on and off. In addition to the button LEDs, several non-button LEDs were turning on and off: meter LEDS, select LEDs, and a few of the encoder ring LEDs for encoders 1 and 2.
3. While the test ran the device sent one "note on" message: note 0, velocity 127.

#### Test 4 Conclusions
1. All LEDs, except for the encoder ring LEDs, can be probably be controlled with "note on" messages.
2. Oddly, a few of the encoder ring LEDs turned on, so that is a mystery. Perhaps others turned on and off quickly so that I could not see them.

## Test 5
Test 5 is similar to Test 4, but now I can set ranges for the "note on" vectors, set the rate, and run refined tests.

note 0-0, vel 0-127, 10 ms
Observation - Several LEDs turn on during the test. At the end, the LCD changes by clearing the "(Offline)". About five seconds after the test completes, all teh LEDs turn off and the LCD displays "(Offline)" again.

note 0-0, vel 0-126, 10 ms
Observation - after test runs, These buttons are ON:Pan, EQ, Select 1-8, Enter, Flip, Plugin. All remain lit forever after the test. The LCD "(Offline)" indication never changes.

Now if I press the ON buttons, I notice they all send note = 0, which makes sense.

note 0-0, vel 127-127, 10 ms
Observation - Sending out a note=0, vel=127 message makes the device sends out a note=0, vel=127 message. And the "(Offline)" LCD text goes away. And all the LEDs turn off after several seconds.

note 0-0, vel 0-126, 100 ms
Now I run this range repeatedly and observe the LEDs. I can see that the buttons turn off early in the test and back on later in the test. That matches the observed velocities for reflected button down messages have bit 6 set in the velocity.

note 1-1, vel 0-126, 100 ms
These LEDS are ON: Send, Select Green LED 1-8, Undo, Master Faders, Mix. So now we see that this range covers Select Green LEDs 1-8. By refining the velocity range we can determine their corresponding velocity values.

## Test 6
Test 6 is similar to Test 5, but now the velocity range is reduced to 0-63 and a set of OFF and ON (bit 6) messages are created for the range. This lets me further narrow ranges to isolate the non-button LEDs and confirm that they turn ON and OFF the same as the button LEDs.

note 1-1, vel 0-63, 10 ms
As expected, these LEDS are ON: Send, Select Green LED 1-8, Undo, Master Faders, Mix.

note 1-1, vel 0-31, 10 ms
Same result: these LEDS are ON: Send, Select Green LED 1-8, Undo, Master Faders, Mix

note 1-1, vel 0-15, 10 ms
Same result: these LEDS are ON: Send, Select Green LED 1-8, Undo, Master Faders, Mix

note 1-1, vel 0-7, 10 ms
new result: these LEDS are ON: Select Green LED 1-8

So, sub velocity values 0-7 address the Select Green LEDs for note = 1.

note 2-2, vel 0-7, 10 ms
new result: Solo 1-8 On

note 3-3, vel 0-7, 10 ms
new result: Mute 1-8 On

note 4-4, vel 0-7, 10 ms
new result: Select Red LEDs are ON.
So, sub velocity values 0-7 address the Select Red LEDs for note = 4.

So now I will try to zero in on the meter LEDs.

note 4-127, vel 0-7, 10 ms
Eventually, the meter LEDs turn on. But it takes a while to get there. So, lets refine the note range.

note 64-127, vel 0-7, 10 ms
The meter LEDs turn on very quick now. But oddly they turn off and on as it progresses.
Meter 0 1-8 ON, Meter 0 1-8 OFF, Meter -3 1-8 ON, Meter 0 1-8 ON, Meter 0 1-8 OFF, Meter -3 1-8 OFF, Meter -6 1-8 ON, .... and so on. It is like the meter LEDs are bits in a counter counting up as the sequence progresses.

note 64-64, vel 0-7, 10 ms
Nothing happens.

note 65-65, vel 0-7, 10 ms
This turns on all eight Meter 0 LEDs.

note 66-66, vel 0-7, 10 ms
This turns on all eight Meter -3 LEDs and turns OFF all eight Meter 0 LEDs.

note 67-67, vel 0-7, 10 ms
This turns on all eight Meter 0 LEDs. The eight Meter -3 LEDs remain lit

note 68-68, vel 0-7, 10 ms
This turns OFF all eight Meter -3 and 0 LEDs and turn ON all eight Meter -6 LEDs.

note 68-68, vel 0-7, 10 ms
This turns OFF all eight Meter -3 LEDs and turn ON all eight Meter -6 LEDs.

note 64-64, vel 0-7, 10 ms
This turns OFF all LEDS.

note 65-65, vel 0-0, 10 ms
This turns ON all Strip 1, Meter 0 LED.

I think the Note is acting like a bitfield and velocity addresses the strip.

## Test 7
Test 7 confirms my idea that the meter LEDs for a strip are addressed by the velocity 0-7 and the note, offset by 64, is a bitfield for teh meters. In teh code you can set the bitfield and the velocity and light up the meters as desired.

## Test 8
Going back to Test 6, I could not find any way to use note on messages to light more than just a few encoder ring LEDs. So, for test 8 I will send all possible controller change values.

Observation - Controller numbers 0-63 repeatedly address the faders 1-8 and the value sets the fader position. Controller numbers 64-127 appear to have no effect on the surface.

## Test 9
Test 9 sends sequenced sysex vectors to the device to see if anything can be found tha alters the surface. Digidesign uses ID=0x13, so we will try that. Because each data byte can be 0-127, it can take huge amounts of time to sequence through all values if the sysex message has more than a few data bytes. So, the strategy will be to limit each byte to a small range. The code has a 'numBitsPerByte' value to set the range. numBitsPerByte=2 means that it will cycle through 0-3 range. 3 means 0-7 range and so on.

for example, numBytesStart=1, numBytesEnd=2, numBitsPerByte=2 sends these sysex messages:
```
04:15:36.432	To Port 1	SysEx		Digidesign $4 bytes	F0 13 00 F7
04:15:36.443	To Port 1	SysEx		Digidesign $4 bytes	F0 13 01 F7
04:15:36.452	To Port 1	SysEx		Digidesign $4 bytes	F0 13 02 F7
04:15:36.462	To Port 1	SysEx		Digidesign $4 bytes	F0 13 03 F7
04:15:36.472	To Port 1	SysEx		Digidesign $5 bytes	F0 13 00 00 F7
04:15:36.482	To Port 1	SysEx		Digidesign $5 bytes	F0 13 00 01 F7
04:15:36.493	To Port 1	SysEx		Digidesign $5 bytes	F0 13 00 02 F7
04:15:36.502	To Port 1	SysEx		Digidesign $5 bytes	F0 13 00 03 F7
04:15:36.513	To Port 1	SysEx		Digidesign $5 bytes	F0 13 01 00 F7
04:15:36.523	To Port 1	SysEx		Digidesign $5 bytes	F0 13 01 01 F7
04:15:36.532	To Port 1	SysEx		Digidesign $5 bytes	F0 13 01 02 F7
04:15:36.543	To Port 1	SysEx		Digidesign $5 bytes	F0 13 01 03 F7
04:15:36.552	To Port 1	SysEx		Digidesign $5 bytes	F0 13 02 00 F7
04:15:36.563	To Port 1	SysEx		Digidesign $5 bytes	F0 13 02 01 F7
04:15:36.573	To Port 1	SysEx		Digidesign $5 bytes	F0 13 02 02 F7
04:15:36.583	To Port 1	SysEx		Digidesign $5 bytes	F0 13 02 03 F7
04:15:36.592	To Port 1	SysEx		Digidesign $5 bytes	F0 13 03 00 F7
04:15:36.606	To Port 1	SysEx		Digidesign $5 bytes	F0 13 03 01 F7
04:15:36.615	To Port 1	SysEx		Digidesign $5 bytes	F0 13 03 02 F7
04:15:36.623	To Port 1	SysEx		Digidesign $5 bytes	F0 13 03 03 F7
```
Observations
numBytesStart=1, numBytesEnd=2, numBitsPerByte=2
No response from surface

numBytesStart=3, numBytesEnd=3, numBitsPerByte=2
No response from surface

numBytesStart=4, numBytesEnd=4, numBitsPerByte=2
Encoder rings 1-4 light up two LEDs each.

numBytesStart=5, numBytesEnd=5, numBitsPerByte=2
No response from surface

numBytesStart=4, numBytesEnd=4, numBitsPerByte=3
Encoder rings 1-8 light up three LEDs each. All the LED rings light up around the same time, near the start of the test. The test keeps running and nothing happens thereafter. Watching it again, it appears the LED rings light up at about the time the first byte changes from 0 to 1. All the changes happen while that first byte == 1 I think.

So, it appears a message like this is affecting encoder rings:
F0 13 01 D2 D3 D4 F7

## Test 10
Test 10 refines test 9 to set the first byte to 0x01 and sequnce just three bytes through ranges.

For example, for numBytesStart=3, numBytesEnd=3, numBitsPerByte=3, it goes through this sequence:

```
04:33:53.863	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 00 00 00 F7
04:33:53.872	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 00 00 01 F7
04:33:53.882	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 00 00 02 F7
04:33:53.892	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 00 00 03 F7
04:33:53.903	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 00 00 04 F7
....
04:33:58.953	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 07 07 03 F7
04:33:58.963	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 07 07 04 F7
04:33:58.972	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 07 07 05 F7
04:33:58.982	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 07 07 06 F7
04:33:58.992	To Port 1	SysEx		Digidesign $7 bytes	F0 13 01 07 07 07 F7
```

And indeed the encoder rings light up almost immediately upon starting this sequence. Typically, Sysex messages are formed as:

F0, Manufacturer ID, Device ID, Data, F7

So this suggests a device ID of 0x01.


