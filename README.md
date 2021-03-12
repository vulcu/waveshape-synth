# WaveDist - Waveshaper Audio Distortion #
A stereo audio distortion plugin created in JSFX for the Reaper DAW.

## Table of Contents ##
* [General Info](#general-info)
* [Features](#features)
* [Installation](#installation)
* [Algorithms](#algorithms)
* [References](#references)

## General Info
This project makes things sound bad, but in a good way. It relies on a handful of waveshaping algorithms to produce differing kinds of overdrive and distortion. The harmonic ratios and the balance between even and odd harmonics varies by algorithm, with some sounding better on certain musical sources than others. There's no hard-and-fast rules here, so just use your ears.

The `Input Rectification` control can lend itself to some neat octave-doubling effects too!

## Features ##
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
