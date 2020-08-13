# FAQ - Frequently Asked Questions

[How do I compile a PortAudio program?](#how-do-i-compile-portaudio-on-my-computer)

[How do I play a WAV or AIFF file?](#how-do-i-play-a-wav-or-aiff-file)

[How do I play or record a file using PortAudio?](#how-do-i-play-or-record-a-file-using-portaudio)

[How do I write a digital filter, or a reverb, or an FFT?](#how-do-i-write-a-digital-filter-or-a-reverb-or-an-fft)

[How do I make portAudio play two tones at the same time without distortion?](#how-do-i-make-portaudio-play-two-tones-at-the-same-time-without-distortion)

[How do I get audio at a sample rate that is not supported by PortAudio?](#how-do-i-get-audio-at-a-sample-rate-that-is-not-supported-by-portaudio)

## How do I compile PortAudio on my computer?

Please read the PortAudio tutorial. It has a section for most supported computer platforms.

## How do I play a WAV or AIFF file?

PortAudio has no support for reading or writing formatted audio files. We recommend using libsndfile for audio file I/O.

## How do I play or record a file using PortAudio?

We do not recommend doing file I/O in the PortAudio callback because the callback is run at a very high priority.
We recommend reading the data in a normal priority thread then writing it to a FIFO which is read by the PortAudio callback.
V19 has special functions called Pa_WriteStream() and Pa_ReadStream() for doing stream I/O though a FIFO.

## How do I write a digital filter, or a reverb, or an FFT?

That's a great question, but it is beyond the scope of PortAudio.
We just do audio I/O. How you generate the audio data is up to you.
The music-dsp mail list is dedicated to these topics and is a great source of information.
Also check out these audio programming links.

## How do I make portAudio play two tones at the same time without distortion?

Create just one PortAudio stream. Then in the audio callback function, simply mix the two tones together by adding their samples.
You will need to scale their amplitudes down so that they do not clip.
In this example I generate two waveforms then mix them together with different amplitudes.
I am using the paFloat32 data format.

    sineValue = generateNextSine();
    sawValue = generateNextSaw();
    *outputPtr++ = (0.3f * sineValue) + (0.4f * sawValue);
    
## How do I get audio at a sample rate that is not supported by PortAudio?

PortAudio will use whatever sample rates are available on the host.
PortAudio does not provide any additional sample rate conversion capability.
If you need a sample rate that is not available then you might want to use the SRC (Sample Rate Conversion) library by Eric de Castro Lopo.
