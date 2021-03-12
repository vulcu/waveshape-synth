# Waveshape-Synth - Waveshaping Polyphonic Synthesizer #
An 8-voice polyphonic audio synthesizer with per-voice oscillator waveshaping created as a collection of Pure Data subpatches. Compatible with Pd-Vanilla but largely developed and tested in Purr Data. It was inspired by [wavedist](https://github.com/vulcu/wavedist) and uses the same waveshaping algorithms.

## Table of Contents ##
* [General Info](#general-info)
* [Features](#features)
* [Installation](#installation)
* [Algorithms](#algorithms)
* [References](#references)

## General Info
This project is a synthesizer not quite like any other. It relies on a handful of waveshaping algorithms to produce differing kinds of overdrive and distortion from the oscillators of each synthesizer voice, and then applies an ADS-envelope low-pass filter to each voice on an individual basis. The harmonic ratios and the balance between even and odd harmonics varies by algorithm, with some sounding better than others for certain oscillator and envelope combinations. There's no hard-and-fast rules here, so just use your ears.

The goal of this project is to provide a quick and simple way for a user to dial in rich, complex synth sounds without needing to know much about synthesizers. Unlike the [wavedist](https://github.com/vulcu/wavedist) plugin, all the waveshapers are active at once here, allowing for some incredibly complex harmonics.

## Features ##
Use `example.pd` to see typical usage within a Pure Data patch
Use `Waveshape-Automatism.pd` to interface with Automatism sub-patches and control signals
There are controls for `Input` and `Output` levels, along with a few other sliders:

![Main User Interface](./images/wavedist-ui-main.png)

There is also a drop-down selector box for selecting which waveshaper is active:

![Algorithm Drop-down Selector](./images/wavedist-ui-dropdown.png)

Each option in the drop-down has an associated slider to control it:
1) `leaky-integrator` -> `Integrator Tc`
2) `soft-knee` -> `Soft-Clip Knee`
3) `cubic` -> `Cubic Harmonic Balance`
4) `warp` -> `Warp K Level`

The `Input Rectification` is always active and can be used in addition to the active waveshaper. It controls the amount by which the input signal is recified and can be set to `0.0` to disable it.

#### To Do ####
* Add oversampling + filtering to reduce aliasing artifacts

#### Status: This project is active but development is slow ####

## Installation ##
To use this project, install it locally into your systems's `/Reaper/Effects` folder. On Windows 10, JSFX effects are usually located in `%appdata%\Reaper\Effects`. You may want to install it into another folder within that folder, such as %appdata%\Reaper\Effects\vulcu`.

## Algorithms ##
This project uses the following algorithms for waveshaping and signal limiting:
```
 1) soft clip alg:        y[n] = (1.5*x[n]) - (0.5*x[n]^3);
 2) leaky integrator alg: y[n] = ((1 - A) * x[n]) + (A * y[n - 1]);
 3) soft knee clip alg:   y[n] = x[n] / (K * abs(x[n]) + 1);
 4) cubic soft clip alg:  y[n] = (1.5 * threshold * HardClip(x[n])) -
                                ((0.5 * HardClip(x[n])^3) / threshold);
 5) warp alg: y[n] = (((x[n] * 1.5) - (0.5 * x[n]^3)) * (((2 * K) / (1 - K))
                    + 1)) / ((abs((x[n] * 1.5) - (0.5 * x[n]^3)) 
                    * ((2 * K) / (1 - K))) + 1);
 6) rectify alg: y[n] = ((1 - R) * softclip(x[n])) + (|softclip(x[n])| * R);
```

## References: ##
1)  Aarts, R.M., Larsen, E., and Schobben, D., 2002, 'Improving Perceived Bass and Reconstruction of High Frequencies for Band Limited Signals' Proc. 1st IEEE Benelux Workshop MPCA-2002, pp. 59-71
 2) Arora et al., 2006, 'Low Complexity Virtual Bass Enhancement Algorithm for Portable Multimedia Device' AES 29th International Conferance, Seoul, Korea, 2006 September 2-4
 3) Gerstle, B., 2009, 'Tunable Virtual Bass Enhancement', [ONLINE] <http:rabbit.eng.miami.edu/students/ddickey/pics/Gerstle_Final_Project.pdf>
 4) Yates, R. and Lyons, R., 2008, 'DC Blocker Algorithms' IEEE Signal Processing Magazine, March 2008, pp. 132-134

## ##
(C) 2017-2021, Winry R. Litwa-Vulcu

