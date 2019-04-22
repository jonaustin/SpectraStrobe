# Cymatic Tone Generator

*CymaticToneGenerator_x86.dll* and *CymaticToneGenerator_x64.dll* are the 32 bit and 64 bit VST plug-ins respectively.

This plug-in is a [binaural tone generator](https://en.wikipedia.org/wiki/Beat_(acoustics)#Binaural_beats)
with variable waveforms, base frequency, beat frequency, and a handful of other waveform adjustments. It can
generate a primary fundamental tone between *0 hz to 1000 hz* and a secondary tone at an offset from the
fundamental of *0 hz to +100 hz*. It is possible to generate and mix smoothly between monaural and binaural tones.

The plug-in can also be thought of as a general purpose, low-to-mid frequency tone generator with a secondary,
detunable oscillator and adjustable stereo width.

Cymatic Tone Generator has the following parameters:

| Parameter | Purpose |
|-----------|---------|
| Waveform | Selects tone waveform: *sine, square, triangle, saw, pulse*. |
| Base Frequency | Sets base/fundamental tone frequency in hz. |
| Freq. Offset | Sets the beat frequency in hz. 0 disables secondary tone. |
| Binaural | Continuously adjusts the tone between binaural and monaural. A value of *0%* is full monaural |
| Pulse Width | When pulse wave is selected, adjust the pulse width *0% to 100%* (duty cycle). |
| Waveform Smooth | Adds an adjustable smoothing filter to the waveform. A value of 0 disables smoothing. |
| Amplitude | Adjusts the waveforms amplitude from *0% to 100%*. |
| Amp. Smooth | Adds an adjustable smoothing filter to the amplitude. A value of 0 disables smoothing. |
| Amp. Offset | Adjust the waveform's amplitude offset by *-100% to +100%* from center. |
| Phase Offset | Adjust the waveform's phase offset by *0% to +100%*. |
| Direction | Changes the direction of the waveform. |

### Theory of Operation

The plug-in will appear as a MIDI Instrument. Place an instance of the plug-in on an audio track in your
DAW. The tone generator responds to MIDI *note on* and *note off* messages. When a MIDI *note on* message is received,
the tone generator will begin generating a continuous tone until a MIDI *note off* message is received.

If you are truly trying to create [binaural tones](https://en.wikipedia.org/wiki/Beat_(acoustics)#Binaural_beats)
you should stick with a *sine* waveform. The other waveforms are there for experimentation or alternative audio effects use.
You can take the "edge" out of a waveform by applying high waveform smooth values, but generally speaking sine
waves will work best. All non-sine waveforms are band-limited.

***Note:** The plug-in does not discern between different notes. Any MIDI note on or note off message will
control the tone generator's output. It is best to pick a single MIDI note to act as the trigger and to
use the same MIDI note through the duration of your mix. For example, if you pick MIDI note C-3 as your tone
trigger, always use note C-3 to start and stop tone generation using note on and off messages.*

#### Using Single Tone Mode

To generate only a single tone at the base frequency, set the **Freq. Offset** parameter to *0*. Adjusting the
**Binaural** parameter in single tone mode will only pan the single tone left and right.

#### Using Dual Tone Mode

Setting the **Freq. Offset** parameter to any value greater than *0* will produce a second tone with a frequency
of: *base frequency (hz) + frequency offset (hz)*. For example, a base frequency of *200 hz* and a frequency offset
of *10 hz* would produce a secondary tone at *210 hz* (*200 hz + 10 hz*).

#### Binaural Tones

When adjusting the **Binaural** parameter in the range of *-100% to +100%*, each tone will panned hard left and right
so that one tone is playing entirely in one ear at the extremes of the value range. Shifting between positive and negative
values will invert the stereo side that each tone is playing on.

#### Monaural Tones

When the **Binaural** parameter is set to the middle value of *0%*, the tones produced will be monaural and will
have a more emphasized beat with a focus in the center of the stereo field (completely balanced).

***Note:** It is possible to use in-between values for the **Binaural** parameter to create tones that are a mix
between monaural and binaural. By automating the **Binaural** parameter's values it is possible to create smooth
mix between monaural and binaural tones.*

