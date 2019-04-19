# SpectraStrobe
**Information and resources for creating SpectraStrobe audio tracks.**

SpectraStrobe and AudioStrobe are audio encoding/decoding technologies for use with
Auditory & Visual Stimulation (AVS) devices, also referred to as [Mind Machines](https://en.wikipedia.org/wiki/Mind_machine),
or Light & Sound Machines (L&S). These devices use specifically crafted auditory and visual
stimulus to explore different states of consciousness, typically in an attempt to invoke the
[frequency follow response (FFR)](https://en.wikipedia.org/wiki/Frequency_following_response) of the brain
through the use of techniques such as [binaural beats](https://en.wikipedia.org/wiki/Beat_(acoustics)#Binaural_beats),
monaural beats, and [isochronic tones](https://en.wikipedia.org/wiki/Isochronic_tones).

## Table of Contents

- [Format Specification](#format-specification)
  - [Encoding and Decoding Overview](#encoding-and-decoding-overview)
  - [Control Frequencies](#control-frequencies)
  - [SpectraStrobe Reference Tone](#spectrastrobe-reference-tone)
  - [Compatible Audio Formats](#compatible-audio-formats)
    - [Sample Rate](#sample-rate)
    - [Bit Depth](#bit-depth)
    - [File Formats](#file-formats)
      - [Wave Files](#wave-files)
      - [MP3 Files](#mp3-files)
      - [Flac Files](#flac-files)
- [Theory of Operation](#theory-of-operation)
  - [Audio Mixing Theory](#audio-mixing-theory)
    - [Adding SpectraStrobe to the Mix](#adding-spectrastrobe-to-the-mix)
    - [Headroom Considerations](#headroom-considerations)
    - [Equalization and Minimizing Frequency Bleed](#equalization-and-minimizing-frequency-bleed)
    - [Equalization and Filtering of Audible Tracks](#equalization-and-filtering-of-audible-tracks)
    - [Mixing Techniques](#mixing-techniques)
- [Resources](#resources)
  - [SpectraStrobe Tones for Samplers](#spectrastrobe-tones-for-samplers)
  - [SpectraStrobe Templates](#spectrastrobe-templates)
    - [Ableton Live](#ableton-live)
    - [Renoise](#renoise)
  - [SpectraStrobe VST Plugins](#spectrastrobe-vst-plugins)
    - [Cymatic SpectraStrobe](#cymatic-spectrastrobe)
    - [Cymatic LFO](#cymatic-lfo)

## Format Specification

Although this repository is meant to primarily address the SpectraStrobe format, both AudioStrobe and
SpectraStrobe work through similar principles, with SpectraStrobe being a bit more complex both in
implementation and capabilities.

In AVS devices the auditory component is usually experienced by the user through a pair of headphones
typically consisting of device-generated tones, recorded audio, or both. The visual component is usually
experienced as pulses of light emitted from LEDs built into special eye glasses that the user wears in
conjunction with the headphones. Some models offer a closed-eye experience while others allow for open-eye
viewing. The goal is generally to synchronize the auditory and visual pulses to create the desired stimulus
for the user.

AudioStrobe and SpectraStrobe are both focused on adding visual control signals  directly to recorded audio material
and do not focus on generating audio tones directly on an AVS device itself. This is achieved by encoding
and decoding visual control signals into the high frequency content of recorded audio in ranges that are
typically beyond the point of most humans' hearing, approaching [20 khz (kilohertz)](https://en.wikipedia.org/wiki/Hertz).

### Encoding and Decoding Overview

AudioStrobe and SpectraStrobe take a similar approach to encoding and decoding visual control signals from
within audio recordings, however AudioStrobe only encodes a single stereo control signal, where as SpectraStrobe
encodes three separate stereo control signals which typically correspond to individual [red, green, and blue
LEDs](https://en.wikipedia.org/wiki/Light-emitting_diode).

For encoding, high frequency [sine wave](https://en.wikipedia.org/wiki/Sine_wave) tones are added into
the audio content of the recording. Decoding involves using specialized filters during playback to isolate these
high frequency tones from the rest of the audio content, which are then used to control the brightness levels
of the AVS device's LEDs.

The brightness of the LEDs is based on the [amplitude](https://en.wikipedia.org/wiki/Amplitude) (volume) of
the corresponding high frequency control tone(s).

### Control Frequencies

The following table lists the various control tone frequencies:

| Format | Frequency | Purpose |
|--------|-----------|---------|
| AudioStrobe | 19200 hz | controls the brighness of the LEDs; does not specify LED color |
| SpectraStrobe | 18200 hz | reference tone used to indicate the SpectraStrobe format |
| SpectraStrobe | 18700 hz | controls the brightness of the red LEDs |
| SpectraStrobe | 19200 hz | controls the brightness of the green LEDs |
| SpectraStrobe | 19700 hz | controls the brightness of the blue LEDs |

***Note**: The SpectraStrobe reference tone at 18200 hz also requires a specific stereo panning pattern to be
applied in order for it to be correctly identified as SpectraStrobe audio.*

#### SpectraStrobe Reference Tone

Since both AudioStrobe and SpectraStrobe share a frequency space (19200 hz), in order for an AVS device
to be able to detect which type of material it is playing back, a reference tone must be added at 18200 hz
to indicate SpectraStrobe. This tone is also a sine wave but requires a special [stereo panning](https://en.wikipedia.org/wiki/Panning_(audio))
pattern to initiate detection.

The SpectraStrobe 18200 hz reference tone must be panned hard left/right (100% left then 100% right) in a
repeating pattern at a rate between 10-20 cycles per second (hz). Using audio mixing metaphors, and speaking
with the terminology of a [Low Frequency Oscillator (LFO)](https://en.wikipedia.org/wiki/Low-frequency_oscillation),
this would be interpreted as:

`waveform: square, frequency: 20 hz, offset: 0%, amplitude: 100%`

which would be applied to the stereo panning of the SpectraStrobe reference tone track within the mix.

### Compatible Audio Formats

#### Sample Rate

This indicates the number of [audio samples](https://en.wikipedia.org/wiki/Sampling_(music)) per second
used in digital audio recordings.

**Minimum: [44100 hz](https://en.wikipedia.org/wiki/44,100_Hz)**

Since the control tone frequencies are very high (nearly [ultrasonic](https://en.wikipedia.org/wiki/Ultrasound)) they
require enough bandwidth in the [sample rate](https://en.wikipedia.org/wiki/Sampling_(signal_processing)#Sampling_rate)
to be properly reproduced. The [Nyquist theorem](https://en.wikipedia.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem)
dictates that in order for a given frequency to be accurately reproduced, the bandwidth (sample rate) required is
twice that of the desired frequency. The highest frequencies in AudioStrobe and SpectraStrobe are nearly 20000 hz (or 20 khz)
so the minimum sample rate required would be 44100 hz (44.1 khz).

[44100 hz](https://en.wikipedia.org/wiki/44,100_Hz) has been the standard for digital consumer audio for decades
starting in the late 70s and continuing as the standard for Compact Discs into the 80s and beyond.

***Note:** Higher sampling rates (48000 hz, 96000 hz, etc.) are completely acceptable during the audio mixing process, however
most AVS devices will do just fine with 44100 hz and may only support playback at lower sample rates with 44100 hz
and 48000 hz being safe targets. Bouncing the final stereo mix to one of these two lower sample rates is recommended
for compatibility.*

#### Bit Depth

In digital audio recordings the [bit depth](https://en.wikipedia.org/wiki/Audio_bit_depth) indicates how many [bits](https://en.wikipedia.org/wiki/Bit)
should be used to encode the audio waveform's [amplitude](https://en.wikipedia.org/wiki/Amplitude) (volume).
The more bits available per sample, the more distinct/discreet levels of amplitude can be represented. In digital images
the equivalent would be: *the more bits used per pixel of an image, the more distinct colors available per pixel*.

**Target: [16 bits, unsigned integer](https://en.wikipedia.org/wiki/16-bit)**

Besides having a sampling rate of at least 44100 hz, the final digital audio recording should also have a minimum
bit depth of 16 bits per sample. During the audio mixing process it is recommended to use 32 bit
[floating point](https://en.wikipedia.org/wiki/Floating-point_arithmetic) precision, although 16 bit integer will also suffice.

***Note:** For the final stereo mix/bounce 16 bit is recommended for compatibility and smaller file sizes.*

#### File Formats

| Precision | File Format | Pro | Con |
|----------|--------------|---------|-----------|
| Lossless | .wav | accurate playback | large file size |
| Lossy | .mp3 | inaccurate playback | smaller file size |
| Lossless | .flac | accurate playback & medium file size | not presently supported |


There are two main types of digital audio file formats. Those are that are [lossless](https://en.wikipedia.org/wiki/Data_compression#Lossless)
and those that are [lossy](https://en.wikipedia.org/wiki/Data_compression#Lossy). Lossless formats will perfectly reproduce
the original digital audio content sample-by-sample during playback whereas lossy formats use digital [data compression](https://en.wikipedia.org/wiki/Data_compression)
to minimize the audio file size at the cost of introducing compression artifacts that cause inaccuracies
during playback. Lossy inaccuracies can vary from minor/unnoticeable to severe and audible, which is generally related
to the level of compression used: *the more compression, the smaller the file, and the higher the inaccuracies
similar to image compression artifacts in a file format like .jpg*.

***Note:** All final mixes should be stereo.*

##### Wave Files

**Recommended Settings: 44100 hz or 48000 hz, 16 bit**

[Wave files (.wav)](https://en.wikipedia.org/wiki/WAV) use uncompressed audio and offer perfect playback of the
original digital recordings but have the largest file size.

***Note:** Although it is possible to have 32 bit wave files, 16 bit is recommended for compatibility.*

##### MP3 Files

**Recommended Settings: 44100 hz or 48000 hz, 16 bit, [320 Kbps constant bit rate (CBR)](https://en.wikipedia.org/wiki/Constant_bitrate)**

[MP3 files (.mp3)](https://en.wikipedia.org/wiki/MP3) use varying methods of audio compression to achieve smaller
file sizes at the cost of introducing inaccuracies during audio playback.

***Note:** Although it is possible to use both variable bit rate (VRB) and higher compression settings (like 128 Kbps, etc.)
this is not recommended for use with AudioStrobe and SpectraStrobe as this will introduce too many sample
inaccuracies during playback and likely lead to visual control errors or failure to detect visual control in general.*

##### Flac Files

The [Flac file format (.flac)](https://en.wikipedia.org/wiki/FLAC) would be an ideal format for AVS devices
since it uses lossless compression to achieve smaller file sizes than .wav files, but allows for sample-perfect
playback of digital audio unlike .mp3 files. As of this time of the writing of this content no known AVS devices
support the .flac file format however (corrections welcome).

## Theory of Operation

During playback of the AudioStrobe or SpectraStrobe content special filters are used to isolate the control
tones in their respective frequency bands (18200 hz, 18700 hz, 19200 hz, 19700 hz). The amplitude of these
tones is used to control the respective brightness of the LEDs on the AVS device's glasses. In the case of
SpectraStrobe, the reference tone at 18200 hz does not directly control brightness, but indicates to the
playback device that SpectraStrobe content is present.

For the color control tones (18700 hz, 19200 hz, 19700 hz) the amplitude of their signals in relation to
the amplitude of the reference tone (18200 hz) determines the brightness of each LED channel. AudioStrobe
does not include a reference tone, so it uses a fixed amplitude model.

In general [-24 decibel (dB)](https://en.wikipedia.org/wiki/Decibel) indicates maximum brightness for the
color channel. For reference 0 dB is the maximum allowed signal level for a digital audio recording.

### Audio Mixing Theory

Digital [audio mixing](https://en.wikipedia.org/wiki/Audio_mixing) usually consists of a piece of software
or equipment known as a [Digital Audio Workstation (DAW)](https://en.wikipedia.org/wiki/Digital_audio_workstation).
Some audio software focuses more on simply editing single or multi track audio files as opposed to being a
full-fledged audio workstation.

Although digital audio software may have different user interfaces, the mixing
theory is typically the same:

*A series of mono or stereo audio "tracks" (isolated channels of audio playback)
that each have independent controls for volume (usually measured in dB), stereo panning (left/right speaker playback),
and options to process audio independently in each audio track. Mathematically speaking, each independent audio
track's waveform is added together (summed) to produce a final 2 channel, stereo mix, which is what listeners
ultimately hear during file playback.*

#### Adding SpectraStrobe to the Mix
To add AudioStrobe or SpectraStrobe information to an audio mixing session, one should create separate stereo tracks for each
tone required. With the exception of the SpectraStrobe reference tone, varying the volume of the track will
vary the brightness of the corresponding LED channel on the AVS device, and varying the panning of the track
will vary the left/right eye balance of the corresponding LED channel.

The following tables demonstrate the basic LED control theory for each tone track in the mix:

| Track Volume Level (dB) | LED Brightness |
|-------------------|--------------------|
| -INF dB | LED color channel is completely off |
| -24 dB | LED color channel is completely on; full brightness |

| Track Panning Balance (%) | LED Balance |
|-------------------|--------------------|
| 100% Left | LED color channel only displays on left eye; right eye off |
| 0% (centered pan) | LED color channel displays equally on left and right eye |
| 100% Right | LED color channel only displays on right eye; left eye off|

***Note:** These tables are only used to indicate the extremes of the range. As volume and panning are altered,
the corresponding LED values (brightness or left/right eye balance) will gradually change in respect to the
chosen level. Also, -24 dB is simply a standard value for the specification; with SpectraStrobe the overall brightness of each LED color channel is a
relative relationship between the reference tone's dB level and the color tone's dB level. For example, you can
set your reference tone volume below -24 dB to create more headroom in the mix, and you would drop the color tone
tracks volume to match. If the inherent volume of the tones you are mixing already is -24 dB, your maximum volume
level of the audio track on the mixer would actually be 0 dB / unity gain instead.*

#### Headroom Considerations

During audio mixing, all of the independent audio tracks will be summed into the final stereo mix. One must be
aware of the available [headroom](https://en.wikipedia.org/wiki/Headroom_(audio_signal_processing)) in the overall
mix. If the final stereo signal of digital audio reaches or exceeds 0 dB on either left or right channel it produces
digital clipping, which is a form of digital distortion, and is generally considered to be undesirable.

Even those familiar with audio mixing and headroom will want to give special care when mixing an AudioStrobe or
SpectraStrobe recording because even though the tones will not be heard (as they are basically ultrasonic) they still
consume part of the available headroom in the overall stereo mix. You can always adjust the tones down below
-24 dB to provide more headroom for the audible parts of your recording (allowing your mix to get louder), but
as you reduce the volume level of the tones, it is possible that the audible content in the mix gets loud enough
that it will interfere with the detection of the control tones during playback or even completely remove the
ability for the special decoder filters to sense them at all.

One way to achieve proper signal integrity is to EQ or filter all audible tracks so that frequencies starting
in the range of 13 khz and ending at 17 khz begin to taper off, with the steepest cutoff at 17khz. This leaves
the frequency space for 18 khz and above free for AudioStrobe and SpectraStrobe control signals and improves
track headroom since there won't be a stacking of signal levels from the actual sound/music part of the mix with
the inaudible tone control signals.

#### Equalization and Minimizing Frequency Bleed

Since AudioStrobe and SpectraStrobe insert their control signals into the audio recording itself, it is
possible for unwanted frequency energy to "bleed" into the control and reference channels causing unwanted
glitches and imprecise control of each color channel on the LEDs. This effect can be minimized using proper
track equalization (EQ) and filtering. In general, the steeper the equalization or filters, the better, as it
will minimize this frequency-based interference.

Ideally you would use the steepest (highest order/highest dB cut) [bandpass filter](https://en.wikipedia.org/wiki/Band-pass_filter)
available in your DAW on each track. You can also create your own bandpass filter by combining both a [lowpass filter](https://en.wikipedia.org/wiki/Low-pass_filter)
and a [highpass filter](https://en.wikipedia.org/wiki/High-pass_filter) together in series on each track,
and setting their cutoff frequency to the frequency of the respective tones. Keeping Q/resonance at 0 is
recommended as it may unintentionally amplify the energy around the filter cutoff point.

| Audio Track | Suggested Filter |
|-------------|------------------|
| Reference Tone | bandpass (or combined lowpass & highpass) w/ cutoff @ 18.2 khz |
| Red Tone |  bandpass (or combined lowpass & highpass) w/ cutoff @ 18.7 khz |
| Green Tone |  bandpass (or combined lowpass & highpass) w/ cutoff @ 19.2 khz |
| Blue Tone |  bandpass (or combined lowpass & highpass) w/ cutoff @ 19.7 khz |

***Note:** If creating an AudioStrobe track it corresponds to the green color tone @ 19.2 khz.*

#### Equalization and Filtering of Audible Tracks

As mentioned in [headroom considerations](#headroom-considerations), if you find that the audible portions
of your mix (ie the heard music/sound parts) appear to be interfering with the control tones' frequency space
you can place an EQ or [lowpass filter](https://en.wikipedia.org/wiki/Low-pass_filter) such that frequencies
above 17 khz are steeply removed. This will sacrifice some high frequency presence from the mix but the
benefit will be a cleaner signal for the AudioStrobe and SpectraStrobe control signals/tones. If using an
equalizer, you might want to slowly start cutting frequency content around 13 khz with growing steepness
until about 17 khz where the cut should be as steep as your equalizer offers.

#### Mixing Techniques

There are two main methods of achieving an AudioStrobe or SpectraStrobe mix:

1. Use a tone generator to generate sine wave tones at the appropriate frequency and volume that match the duration of
your entire mix and place these into an audio track within your DAW.
2. Use smaller forward-looped samples of each tone frequency, place them into a [sampler](https://en.wikipedia.org/wiki/Sampler_(musical_instrument))
(software or hardware) and trigger their natural note (usually C-3) using MIDI or equivalent to add the
tones to your mix

If you are more comfortable working with larger bounced audio tracks in a multi-track project, choose method 1.
If you would rather keep file size to a minimum and are more comfortable working with digital samplers, choose
method 2.

***Note:** With either technique, the SpectraStrobe reference tone must have [the appropriate panning dynamics](#spectrastrobe-reference-tone)
applied otherwise the AVS device will not recognize your recording as SpectraStrobe and will fall back to AudioStrobe instead.*

The DAW you use will determine the best ways to to manipulate the color control tracks' volume and panning
to control the LED signals. This can often be achieved through track automation by editing control envelopes
of the track's volume and panning. Your DAW also might allow you to apply [LFO](https://en.wikipedia.org/wiki/Low-frequency_oscillation)
control over track volume and panning. Some DAWs will let you use audio from audible channels to drive
the volume level or panning balance of your LED color tracks. It might be possible to combine some or all
of these techniques to creating complex, [modulated](https://en.wikipedia.org/wiki/Modulation) signals.

## Resources

The following is a list of resources that can be used in conjunction with the information presented in this
document to create AudioStrobe and SpectraStrobe content using digital audio software.

### SpectraStrobe Tones for Samplers

The [tones directory](resources/tones) of this repository contains four 1 second looped samples that can be
loaded into a digital sampler and played back using MIDI note C-3. These tones are used by the provided templates
in this repository.

***Note:** Ensure that your sampler has forward-looping of the samples enabled otherwise you will only get 1 second
of audio playback from the tones and then they will stop. Also these tones are produced at max volume, so you will
want to want to add a -24 dB adjustment to their output, either on the sampler or the audio track the sampler is mixed into.
Ideally each of these tones would be output to their own, separate stereo tracks (four individual stereo tracks, one per tone).
The SpectraStrobe reference tone does not have [proper panning applied](#spectrastrobe-reference-tone) so you
will need to figure out how to replicate this using the software or sampler of your choice; the provided
templates do have this panning applied and should work "out-of-box".*

### SpectraStrobe Templates

The [templates directory](resources/templates) of this repository contains example templates for creating
SpectraStrobe recordings with various DAWs. The templates are designed for use with the Kasina from [Mind Place](https://mindplace.com/).
You should be able to open the template projects in their corresponding DAW, study the configuration, and begin
to add your own content following the example they provide. These templates should work as-is, you will just need
to either use the Kasina's USB audio device driver as the output in your DAW or use a standard soundcard output into
the Kasina's AUX input.

#### Ableton Live
[Project Template](resources/templates/ableton)

[Ableton Live](https://www.ableton.com/) is a popular modern commercial DAW which specializes in live performance and playback.
This template uses the built-in sampler instrument (one per track) to implement the reference and color tones
and uses the AutoPan effect on the reference tone track to achieve the specified panning requirements to be
detected as a SpectraStrobe recording.

#### Renoise
[Project Template](resources/templates/renoise)

[Renoise](https://www.renoise.com/) is a lesser known DAW that is based on older computer [tracker programs](https://en.wikipedia.org/wiki/Music_tracker).
This is a contemporary implementation of a tracker as a DAW which has its roots in the computer [demo scene](https://en.wikipedia.org/wiki/Demoscene).
Although it has a steeper learning curve for most, once you understand its workflow and shortcuts it is an extremely
fast way to create music, similar to using a word processor to compose.

Renoise has slightly more interesting LFO and modulation options out-of-box compared to Ableton Live and is
considerably more affordable. It works on Windows, MacOs, and Linux. Renoise offers a free restricted version
with the most notable missing features being: ASIO support on Windows, and rendering to .wav disabled. The paid
license is quite affordable and paid license upgrades come very infrequently.

### SpectraStrobe VST Plugins

The following VSTs are bespoke VSTs developed primarily for personal use. Right now they are compiled for Windows,
as the goal is to keep extraneous development effort light, with the desire to focus on content creation
instead. They are being offered, however, in case others can make use of them. An Audio Units version may be
created in the future.

[VST Directory](resources/vsts)

#### Cymatic SpectraStrobe

[CymaticSpectraStrobe](resources/vsts/CymaticSpectraStrobe/README.md) is a VST effect used to generate and control
AdobeStrobe and SpectraStrobe signals that can also modulate the signals with audio input.

#### Cymatic LFO

[CymaticLFO](resources/vsts/CymaticLFO/README.md) is a VST effect that is designed to pair with CymaticSpectraStrobe.
It can also be used as a general audio/panning modulating LFO. It offers a variety of waveforms and generates frequencies
from 0 → 100 hz. Use this plug-in to modulate a control tone's brightness or panning. Put multiple instances of
the plug-in in series to create complex modulations.