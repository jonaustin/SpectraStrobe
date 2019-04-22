# Cymatic SpectraStrobe

*CymaticSpectraStrobe.component* is a universal binary Audio Units plug-in for both 32 bit and 64 bit DAWs.

This plug-in is meant primarily as an AudioStrobe and SpectraStrobe tone generator, with output level and signal
modulation capabilities. It is an effect plug-in that can sit on any audio channel in a compatible DAW. When
enabled it will output a tone at the selected frequency (for AudioStrobe tone, SpectraStrobe reference tone,
SpectraStrobe red tone, SpectraStrobe green tone, and SpectraStrobe blue tone).

Cymatic SpectraStrobe has the following parameters:

| Parameter | Purpose |
|-----------|---------|
| Enabled | Whether or not the plug-in is currently generating the selected tone. |
| Tone Mode | Sets which tone will be output by the plug-in: *SS Ref, SS Red, SS Green / AS, SS Blue* |
| Gain | A gain adjustment of the tone output; controls the LED color brightness. |
| Gain Smooth | Adds an adjustable smoothing filter to the gain control. A value of 0 disables smoothing.|
| Input Mix | Determines whether or not track audio will be modulated with the tone signal. |
| Input Gain | When using modulated audio input, adjusts the input gain of the audio. |
| Input Smooth| Adds an adjustable smoothing filter to the audio input. A value of 0 disables smoothing.|
| Input RMS | Applies an RMS window (up to 1024 samples) to audio input. A value of 0 disables RMS. |

### Theory of Operation

#### AudioStrobe

If you only want to create AudioStrobe content then you will only need a single instance of the plug-in with
the **tone mode** set to **Green | AS** (AudioStrobe). The green SpectraStrobe tone by itself is the same as
AudioStrobe.

#### SpectraStrobe

##### Setup

For SpectraStrobe content you will need 4 instances of the plug-in. Each of these plug-in instances should be
on their own audio track in your DAW. One for the reference tone to indicate SpectraStrobe content, and the other
3 being the color channel controls for: *red, green, and blue*.

There are default presets for the reference and color tones if you'd like to use them.

***Note:** Do not put multiple instances of the SpectraStrobe plug-in on the same audio track, in series.
Instances will not allow the tones frequencies to all pass and sum correctly. Instances will only attempt
to modulate the tracks incoming audio or block it entirely.*

***Note:** The SpectraStrobe reference tone requires a specific, high-speed panning pattern to be detected. Do not
attempt to adjust the panning of the audio track that the reference tone plug-in instance is on. It must be an
unpanned, stereo audio track.*

##### Adjusting Color Brightness

To adjust the LED brightness output from off to full on, adjust the plug-instances gain parameter or adjust the
track volume of the audio track that the plug-in instance is on in the DAW.

***Note:** The LED brightness level is relative to the volume level of the SpectraStrobe reference tone. It is
possible to keep color volume levels the same, and then lower the reference tone volume, thereby raising the brightness
of all color channels simultaneously. However a reference tone who's volume is too low may eventually have
trouble being detected, especially when mixing with other master audio.*

##### Adjusting Color Panning

The output of a particular color channel can be "panned" back and forth between the left and right eye just as
audio can be panned back and forth between a left and right speaker, and a mix of positions in between. To pan
a color channel, simply pan the stereo audio track in your DAW that the SpectraStrobe plug-in instance of the
desired color is on, and its color output will be panned to match between left and right eyes.

##### Modulating Track Audio

**Input Mix**

Each plug-in instance only has 2 audio input modulation settings: None or Modulate. When set to **None**, the
track audio will be blocked and not pass through the plug-in instance. When the input mix is set to **Modulate**,
the track's audio will be used as a modulation source for the generated tone, thereby modulating the brightness
of the color channel on the LED.

**Input Gain**

A gain applied to the audio input before it is modulated. Gain can be cut to a fraction of track audio, or boosted
to many multiples of incoming track audio volume. If your modulated LED colors are not very bright on the LEDs, try
boosting the gain at this stage until colors appear at the desired brightness level.

**Input Smooth**

Fast audio changes, without smoothing, can sound create audio artifacts that sound like clicks and pops. Applying
a smoothing value greater than 0 will enable a basic smoothing filter with an adjustable smoothing window in
milliseconds. Even a few milliseconds will filter most introduced artifacts.

**Input RMS**

An RMS averaging function that constantly samples and determines the average power of the incoming audio
signal over a window of time, which is then modulated with the color control tone. Any value greater than 0
will enable a progressively larger RMS averaging window of samples up to 1024 samples. This filter can be used
with or as an alternative too the **Input Smooth** filter.

***Note:** Completely unfiltered input audio will be modulated directly with the color tone. This will create
the most responsive feedback, but it will often be the most "glitchy" and noisy as well. Adding one or both input
filters will have the effect of adding an attack and release to the activity of the incoming audio as it is
modulated with the color tone leading to smoother color movements visually.*

### Mixing & Equalizing

Generally, you should be able to just add the plug-in instances, select to tone to generate, and then adjust
the gain to control brightness. The plug-ins by default use 0.2 ms of tone smoothing with helps filter the output signal better
within its respective frequency band so that when they are summed, the signals remain in good integrity in relation
to each other. However a few other mixing and equalizing are worth considering:

##### Bandpass Filter

A tight bandpass filter at the respective frequency of the control tone on each track will help ensure less
energy "leaks" out of the band into the others when the signal is modulated.

##### Lowpass & Highpass Filters

When a bandpass filter is not available, or high-order lowpass and highpass filters are available you might
consider adding one of each, in series, with their cutoff points set to the frequency of the respective color
tone you are trying to filter.

##### Signal Levels

As other audio content is added to your mix, it will additively sum with the control and reference tone signals.
Generally two problems can occur:

1. The rest of your audio content is too loud and energetic when summed with the generated tones so that as
your main mix peaks, the SpectraStrobe signal is lost when decoding the audio and interference or loss of control
temporarily occurs.
2. Audio content in your mix has energy in the frequency ranges of the SpectraStrobe signals and interferes with
your control tone signals. This usually appears as unintended LED "noise" on one or more of the color channels.

To fix the first problem you will need to apply a balance in your mix between the loudest master peaks and the
relative volume of the reference and color tones. Generally speaking the color tones should be as low as they can
while still maintaining clean color information even during your audio mix's loudest peaks. You can move the mix
in either direction to meet in the middle as needed.

As a general rule as well as a solution to the second problem, you can always EQ all or most other audio content
in your mix to remove all energy in frequencies starting around 13 khz and a steep cutoff at 17 khz. AudioStrobe
and SpectraStrobe tones begin right above 18 khz. This can be achieved with multiband equalizers or steep lowpass filters.

##### Using Auxiliary Sends

Creating individual auxiliary sends/buses in your DAW for each plug-in instance is often a helpful design. Not
only does it keep the tracks out of the way of the audible portions of your mix, but if you use audio input modulation
on any of the color channels, it allows you to easily allow one or more audio tracks to modulate colors in a
controlled way by changing their auxiliary send amount on the respective sends. This way a whole multi-channel
mic'd set of drums could modulate the red channel by increasing the send amount to the red send on each of the
mic tracks for the drums, each with individual modulation gain based on send amount.
