# Cymatic LFO

*CymaticLFO_x86.dll* and *CymaticSpectraLFO_x64.dll* are the 32 bit and 64 bit VST plug-ins respectively.

A stereo audio modulation plug-in that was designed to be paired with the CymaticSpectraStrobe VST plug-in.

Cymatic SpectraStrobe has the following parameters:

| Parameter | Purpose |
|-----------|---------|
| Enabled | Sets whether or not the LFO is currently active. |
| LFO Target | Selects whether or not the LFO will modulate *volume* or *panning*. |
| Waveform | Selects the LFO signal's waveform: *sine, square, triangle, saw, pulse*. |
| Frequency | Sets the oscillator's frequency. |
| Pulse Width | When a pulse wave is selected, adjust the pulse width (duty cycle). |
| Waveform Smooth | Adds an adjustable smoothing filter to the waveform. A value of 0 disables smoothing. |
| Amplitude | Adjusts the waveforms amplitude from *0% to 100%*. |
| Amp. Offset | Adjust the waveform's amplitude offset by *-100% to +100%* from center. |
| Direction | Changes the direction of the waveform when saw wave is selected. |
| Gain | Applies a final gain stage to the signal from *0x to 10x*. |

#### Modulating LED Brightness
When the LFO target is set to **volume**, it will modulate a mono waveform with the current audio track.
When used with SpectraStrobe tones signals, this has the effect of modulating LED brightness evenly across both eyes
on the respective color channel.

#### Modulating LED Panning
When the LFO Target is set to **panning** it will modulate brightness between the left and right eyes. By using 2
instances of the plug-in, one after the other, it is possible to modulate both LED brightness and left-to-right eye
panning of a SpectraStrobe color channel.

#### Using Multiple Instances

More than one Cymatic LFO plug-in instance can be added, one after the other, to the same SpectraStrobe audio track.
This will allow for more complicated modulation patterns to both brigthness and left-to-right eye panning. For example,
you could have a fast blinking strobe effect at 15 hz, and then have that overall blinking pattern fading in and out
over a slower 0.5 hz frequency.

