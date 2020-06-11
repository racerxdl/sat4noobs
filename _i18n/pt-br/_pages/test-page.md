---
title: GOES Satellite Hunt
date: 2017-02-19T00:00:00-03:00
author: Lucas Teske
layout: page
guid: https://www.teske.net.br/lucas/goes-satellite-hunt/
---

# GOES Satellite Hunt

This book is adaptation of my blog post [GOES Satellite Hunt](http://www.teske.net.br/lucas/2016/10/goes-satellite-hunt-part-1-antenna-system/) that was published in the end of 2016 while I was reverse engineering the satellite signal. I'm doing this book to have a more organized document about GOES-13 Signals and also to keep up to date with my GOES-16 Reverse Engineering that followed the GOES-13 Reverse Engineering. This book will also be hosted at GitHub in [Create Commons Share Alike](https://creativecommons.org/licenses/by-sa/2.5/br/) license and any fixes are welcome from anyone. In the future I plan to add information about other satellites that use similar down link protocols like MSG-3 \(Meteosat 10\).

I also need to thank all people in \#hearsat @ starchat \(IRC\) for the help I got understanding SDR and Satellite Signal stuff, since when I started that I had no knowledge at all about it. Special thanks for trango \([@usa-satcom](https://twitter.com/usa_satcom)\) and mybit \([@devnulling](https://twitter.com/devnulling)\) for all the help with previous experiences in the area. Also I need to thank all my family for supporting it putting several dishes and antennas all over the roof of our house.

The study in this book lead to the creation of [Open Satellite Project](https://github.com/opensatelliteproject).

[Next Part](motivation)

# Summary

* [Motivation](motivation)
* [The Hardware Setup](the-hardware-setup)
  * [Assemble Process](the-hardware-setup/assemble-process)
  * [Dish Feed](the-hardware-setup/dish-feed)
  * [LNA and Filter](the-hardware-setup/lna-and-filter)
  * [Pointing the Antenna](the-hardware-setup/pointing-the-antenna)
* [The Demodulator](the-demodulator)
  * [Binary Phase Shift Keying Modulation](the-demodulator/demodulator-in-gnu-radio)
  * [Demodulating BPSK Signal](the-demodulator/demodulating-bpsk-signal)
  * [GNU Radio Flow](the-demodulator/gnu-radio-flow)
  * [Decimating and filtering to desired sample rate](the-demodulator/decimating-and-filtering-to-desired-sample-rate)
  * [Automatic Gain Control and Root Raised Cosine Filter](the-demodulator/automatic-gain-control-and-root-raised-cosine-filter)
  * [Synchronization and Clock Recovery](the-demodulator/synchronization-and-clock-recovery)
  * [Symbol Output from GNU Radio](the-demodulator/symbol-output-from-gnu-radio)
* [Frame Decoder](frame-decoder)
  * [Convolution Encoding, Frame Synchronization and Viterbi](frame-decoder/convolution-encoding-frame-synchronization-and-viterbi)
  * [Encoding the sync word](frame-decoder/encoding-the-sync-word)
  * [Frame Synchronization](frame-decoder/frame-synchronization)
  * [Decoding Frame Data](frame-decoder/decoding-frame-data)
* [Packet Demuxer](packet-demuxer)
  * [De-randomization of the data](packet-demuxer/de-randomization-of-the-data)
  * [Reed Solomon Error Correction](packet-demuxer/reed-solomon-error-correction)
  * [Virtual Channel Demuxer](packet-demuxer/virtual-channel-demuxer)
  * [Packet Demuxer](packet-demuxer/packet-demuxer)
  * [Saving the Raw Packet](packet-demuxer/saving-the-raw-packet)
* [File Assembler](file-assembler)
  * [File Header Processing](file-assembler/file-header-processing)
  * [LritRice Compression](file-assembler/lritrice-compression)
  * [File Name from Header](file-assembler/file-name-from-header)
  * [Viewing the files content](file-assembler/viewing-the-files-content)
* [File Types](file-types)
  * [LRIT Header Description](file-types/lrit-header-description)
    * [0 - Primary Header](file-types/lrit-header-description/primary-header)
    * [1 - Image Structure Header](file-types/lrit-header-description/1-image-structure-header)
    * [2 - Image Navigation Record](file-types/lrit-header-description/2-image-navigation-record)
    * [3 - Image Data Function Record](file-types/lrit-header-description/3-image-data-function-record)
    * [4 - Annotation Record](file-types/lrit-header-description/4-annotation-record)
    * [5 - Timestamp Record](file-types/lrit-header-description/5-timestamp-record)
    * [6 - Ancillary Text](file-types/lrit-header-description/6-ancillary-text)
    * [7 - Key Header](file-types/lrit-header-description/7-key-header)
    * [128 - Segment Identification Header](file-types/lrit-header-description/128-segment-identification-header)
    * [128 - Segment Identification Header](file-types/lrit-header-description/128-segment-identification-header)
    * [129 - NOAA Specific Header](file-types/lrit-header-description/129-noaa-specific-header)
    * [130 - Header Structured Record](file-types/lrit-header-description/130-header-structured-record)
    * [131 - Rice Compression Record](file-types/lrit-header-description/131-rice-compression-record)
    * [132 - DCS Filename Record](file-types/lrit-header-description/132-dcs-filename-record)
* [Ending](ending)
