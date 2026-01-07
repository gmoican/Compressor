# Compressor
This is a VST3/AU compressor plugin made with [JUCE](https://juce.com/). The sole purpose of this plugin is to showcase and test the performance of my `Compressor` class in [punk_dsp](https://github.com/gmoican/punk_dsp).

[![Compressor](https://github.com/gmoican/Compressor/actions/workflows/build.yml/badge.svg)](https://github.com/gmoican/Compressor/actions/workflows/build.yml)

![DemoImage](docs/demo.png)

## Features

The `Compressor` uses the following logic for applying dynamic processing:
1. Identify sidechain - _feed-back_ or _feed-forward_ topology determine how this is done.
2. Measure sidechain.
3. Compute gain reduction / addition.
4. Apply gain reduction / addition.

All classes have methods for updating the following parameters:
- Ratio.
- Threshold (in decibels).
- Knee (in decibels).
- Attack and release times (in miliseconds).
- Make-up gain (in decibels).
- Mix (in percentage).
- Feed-back / feed-forward topology.

Furthermore, there is a `getGainReduction` method meant to be used in the GUI for displaying the current gain reduction.

## Usage examples

```cpp
// --- PluginProcessor.h ---
#include "punk_dsp/punk_dsp.h"

class PluginProcessor : public juce::AudioProcessor
{
public:
    /* Your processor public stuff
     * ...
     */
private:
    /* Your processor private stuff
     * ...
     */
    punk_dsp::Compressor compressor;
};

// --- PluginProcessor.cpp ---
void PluginProcessor::prepareToPlay(double sampleRate, int samplesPerBlock)
{
    juce::dsp::ProcessSpec spec;
    spec.maximumBlockSize = samplesPerBlock;
    spec.numChannels = getTotalNumOutputChannels();
    spec.sampleRate = sampleRate;

    compressor.prepare( spec );
    compressor.prepare( spec );
    compressor.prepare( spec );

    // Your code...
}

void PluginProcessor::updateParameters()
{
    // Examples
    compressor.updateThres(-3.f);
    compressor.updateRatio(6.f);
    compressor.updateMix(90.f);

    // Your code...
}

void PunkOTTProcessor::processBlock (juce::AudioBuffer<float>& buffer)
{
    compressor.process(buffer);
    compressor.process(buffer);
    compressor.process(buffer);
}
```

## Plugins that make use of this compressor
* [PunkOTT](https://github.com/gmoican/PunkOTT) and [PunkOTT-MB](https://github.com/gmoican/PunkOTT-MB), my take on OTT-style processors.

