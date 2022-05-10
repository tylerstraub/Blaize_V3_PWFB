### Original Author
big shoutout to BodgedButWorks: https://github.com/bodgedbutworks/Blaize_V3/

### About the (bodged) OSC implementation
- OSC Processing library provided by Andreas Schlegel: https://sojamo.de/libraries/oscP5/
- Intended for use with Max for Live (example included in repo) but should work for other things
- Uses a weird massive array `color_bank` to store color combinations for single button presses... sorry about this
- Uses another odd pattern for button toggles which expects a MIDI node and velocity value (because of how I'm implementing this on the Ableton side)
- Original TCP networking still works

### Changes in this Fork
- disabled login screen and password entry for show/production purposes
- control panel is hidden on startup
- system now detects current target display's resolution and starts in full screen (with controls offscreen to the right)

### Todo
- re-implement some sort of external settings.cfg file for adjusting parameters (had it working at one point, but ran into odd global/typing issues with `fullScreen()`
- fix the BPM handling on OSC input to allow both a float or str

## OSC Messaging Specifications

### SLIDERS (float)
`/speed`: expects float (0-1)

`/size`: expects float (0-1)

`/brightness`: expects float (0-1)

`/strobing`: expects float (0-1)

`/shading`: expects float (0-1)

`/bpm`: expects float (0-1) **atrociously hacked and inacurate, maybe don't use this**

### COLOR CHANGES (pair of ints: note #, velocity)
*velocity doesn't matter here, multicolor toggle is enabled/disabled accordingly*
- MIDI 78-85 (F#6-C#7) **SINGLE COLOR BANKS**
- MIDI 86-127 (D7-G10) **COLOR COMBO BANKS**

### PRESET CHANGES (pair of ints: note #, velocity)
*velocity also ignored here*
- MIDI 0-31 (C0-G2)

### BUTTON TOGGLES (pair of ints: note #, velocity)
*velocity determines on/off state... ON > 64*
- MIDI 36-42 (C3-F#3)

## Blaize Data Structure (original TCP interface)
```
- messages are split on new line (/n)
- messages expect a C value (address registry) and a V value (data registry)

// CHANGE CURRENT PRESET
- C values of 0-31 trigger preset changes (V value must be 0)

ex: 
	C10V0 (switch to preset 10)

// CHANGE COLORS
- value 32 triggers color buttons
- selectable V values for collers are a range from 0-7
	0: RED
	1: YELOW
	2: GREEN
	3: CYAN
	4: BLUE
	5: MAGENTA
	6: WHITE
	7: RANDOM

ex: 
	C32V1 (change color to yellow)
	C32V1/nC32V6 (change color to green and white when multicolor enabled)

// CHANGE BUTTONS (on/off)
- values 36-42 seem to toggle on/off buttons with value 0-1 (some do nothing?)
	36: MULTICOLOR
	37: BLACKOUT
	38: BPM (toggle on/off)
	41: LINE MOVE
	
// CHANGE SLIDERS
- C values of 44-47 control the main sliders with a V range of 0-100
	44: SPEED
	45: SIZE
	46: BRIGHTNESS
	47: STROBING
	50: SHADING

// CHANGE BPM
- C value of 254, v value of target BPM

ex:
	C254V120 (set bpm to 120)
```
