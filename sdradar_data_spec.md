# SDRadar Sample data

## Introduction

The data format is as follows:
- Each **sweep** consists to a series stepped frequency radar **sub-pulse** collections.
- Each stepped frequency **sub-pulse** consists of three files:
  1. a `*.dat` file with binary channel 0 adc sample data (nominally 2560 complex<int16_t> samples or 10.24 kB)
  1. a `*-loopback.ref` file with binary channel 1 reference loopback adc sample data used for calibration (nominally 2560 complex<int16_t> samples or 10.24 kB)
  1. a `*-config.txt` file with a record of the SDRadar configuration and external sensor (GPS/IMU/altimeter) data.
- Each of these three sub-pulse files is time-tagged with the same UTC timestamp and the filename includes the frequency of the sub-pulse.
- `.dat` and `.ref` files are both simply binary files with raw int16 data corresponding to I,Q channels.
- `-config.txt` are ascii string txt files and can parsed with the delimiter `:`

## Data size

The total data size in bytes for each sweep is computed as:

(2 x (wfrm_len * 4) + 3000) x num_freq_steps

- wfrm_len is the number of ADC samples collected as determined by maximum desired range of the SDRadar and the input waveform length. This parameter may be configured arbitrarily in real-time, but the default value is 2560 samples
- num_freq_steps is the number of discrete stepped frequencies at which data is collected per sweep. This is determined from the desired total bandwidth of the system divided by the instantaneous bandwidth of the SDRadar (50 MHz) times the bandwidth efficiency (percent overlap of sub-pulse spectra in frequency domain)
- 3000 corresponds to the nominal 3kB size of the config information file.
