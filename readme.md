### Original Author
big shoutout to BodgedButWorks: https://github.com/bodgedbutworks/Blaize_V3/

### About the (bodged) OSC implementation
- OSC Processing library provided by Andreas Schlegel: https://sojamo.de/libraries/oscP5/
- Intended for use with Max for Live (example included in repo) but should work for other things
- Uses a weird massive array `color_bank` to store color combinations for single button presses... sorry about this
- Uses another odd pattern for button toggles which expects a MIDI node and velocity value (because of how I'm implementing this on the Ableton side)

### Changes in this Fork
- disabled login screen and password entry for show/production purposes
- control panel is hidden on startup
- system now detects current target display's resolution and starts in full screen (with controls offscreen to the right)

### TODO
- re-implement some sort of external settings.cfg file for adjusting parameters (had it working at one point, but ran into odd global/typing issues with `fullScreen()`
- fix the BPM handling on OSC input to allow both a float or str

## OSC Messaging Specifications

