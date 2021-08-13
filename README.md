# Benchmark for Goertzel algorithm and scipy.fftpack.fft

[![Build Status](https://travis-ci.com/NaleRaphael/goertzel-fft.svg?branch=master)](https://travis-ci.com/NaleRaphael/goertzel-fft)
[![Binder](https://mybinder.org/badge_logo.svg)][launch_on_binder]

## Overview

To evaluate the power of specific frequency component in signal, `Goertzel algorithm` will be a better solution than `fast Fourier transform (FFT)`. Because `Goertzel algorithm` allows us to evaluate a single `DFT (Discrete Fourier Transform)` term at a time.

But the computational time is related to the size of data. If we need to analyze a huge volume of data, how can we improve the performance? In this project, `short-time technique` is introduced. It allows us to take less computational time with an acceptable error tolerance (depends on the argument `window` given by user).

## Environments
### Machine
* OS: Windows 7
* CPU: Intel Core i5 5200U @ 2.20GHz
* RAM: 4.00 GB Single-Channel DDR3 @ 798MHz

### Version of Python and packages
* Python: 2.7.9 (2019.10.05: tested on 3.6.5)
* Numpy: 1.9.1 (2019.10.05: tested on 1.14.3)
* Scipy: 0.15.0 (2019.10.05: tested on 1.1.0)

## Installation
* Install from git by `pip`

  (but you won't be able to run scripts of [verification](#Algorithm-verification) and [benchmark](#Run-benchmark))

  ```bash
  $ pip install git+https://github.com/mythopoeic/goertzel-fft.git
  ```

* Clone and install from source

  ```bash
  $ git clone https://github.com/mythopoeic/goertzel-fft.git
  $ cd goertzel-fft
  $ pip install .
  ```

* To uninstall this package:

  ```bash
  $ pip uninstall gofft
  ```

## Usage
* Evaluate the power of a single DFT term by `goertzel`

  ```python
  import gofft
  import numpy as np

  fs = 1000   # sampling frequency
  ft = 60     # target frequency to be evaluated (60 Hz)
  dur = 2     # duration of signal
  num = fs*dur  # sampling points
  t = np.linspace(0, dur, num)  # time series
  data = np.sin(2*np.pi*ft*t)   # signal to be evaluated (60 Hz)

  mag = gofft.alg.goertzel(data, fs, ft, fs)
  print(mag)  # 0.4969141358692001
  ```

  Or you can checkout this ipython notebook: [demo_simple_example.ipynb](./doc/ipynb/demo_simple_example.ipynb)
  (run on [binder][launch_on_binder])

## Implemented algorithms

1. `gofft.alg.goertzel`: Normal Goertzel algorithm.
2. `gofft.alg.goertzel_m`: Same as 1., but it can take multiple values as `ft` (target frequency). This implementation is used to inspect the decrement of overhead resulted by calling `goertzel()` multiple times when we need to evaluate several `ft`s.
3. `gofft.alg.goertzel_st`: Short time version of Goertzel algorithm.
4. `gofft.alg.goertzel_st_m`: Implemented with the same reason of `goertzel_m`.
5. `gofft.alg.fft_eval`: Evaluate specific DFT terms by `scipy.fftpack.fft`.
6. `gofft.alg.stfft_eval`: Short-time version of `fft_eval`.

**NOTE 01: In order to make the comparison as fair as possible, please note that the short-time techniques in `goertzel_st`, `goertzel_st_m` and `stfft_eval` are all implemented in python, not in C.**

**NOTE 02: In this project, `stfft_eval` (short-time version of `fft_eval`) is different to the widely-known [`STFT` (short-time Fourier transform)][STFT].**


## Algorithm verification
*(you don't need to install this package to run the following scripts)*

* To verify the correctness of implemented algorithms, you can run unit tests. 

  All test cases are written in `gofft/alg/tests/test_dsp.py`.

  ```bash
  $ python runtests.py
  ```

## Run benchmark
*(you don't need to install this package to run the following scripts)*

* Run all benchmark cases and plot result

  ```bash
  $ python runbench.py
  ```

* Run all benchmark cases but don't plot result

  ```bash
  $ python runbench.py --skip_plot
  ```

* Plot result only (please make sure that there are log files in folder `bench_log`)

  ```bash
  $ python runbench.py --skip_bench
  ```

## Performance

* Data type: float64 (fig_02 is a partial view of fig_01)

  ![Fig 01. Result of benchmark][dtype_float64_o]

  ![Fig 02. Result of benchmark (zoomed in)][dtype_float64_z]


## Reference
[wikipedia - Goertzel](https://en.wikipedia.org/wiki/Goertzel_algorithm)  
[stackoverflow - Implementation of Goertzel algorithm in C](http://stackoverflow.com/questions/11579367)  

[dtype_float64_o]: https://i.imgur.com/vV9pjDE.png
[dtype_float64_z]: https://i.imgur.com/Bw3ohXI.png

[STFT]: https://en.wikipedia.org/wiki/Short-time_Fourier_transform
[launch_on_binder]: https://mybinder.org/v2/gh/NaleRaphael/goertzel-fft/master?filepath=doc%2Fipynb%2Fdemo_simple_example.ipynb
