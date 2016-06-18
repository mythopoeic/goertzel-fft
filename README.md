# Benchmark of Goertzel algorithm and scipy.fftpack.fft

## Overview  
To evaluate the strength of specific frequency component in signal, Goertzel algorithm will be a better solution than fast Fourier transform(FFT).  
But the computational time is related to the size of data. If we need to analyze a huge volume of data, how can we improve the performance?  
In this project, `short-time technique` is introduced. It allows us to exchange some frequency resolution for a faster computational speed.  


## Package version
python: 2.7.9  
numpy: 1.9.1  
scipy: 0.15.0  


## Implemented algorithms
1. dsp.goertzel: Normal Goertzel algorithm.
2. dsp.goertzel_m: Same as 1., but it can take multiple values as `ft`(target frequency). This implementation is used to inspect the decrement of overhead resulting by calling goertzel() multiple times when we need to evaluate several `ft`.
3. dsp.shorttime_goertzel: Short time version of Goertzel algorithm.
4. dsp.shorttime_goertzel_m: Implemented with the same reason of `goertzel_m`.
5. dsp.fftalg(method='fft'): Normal FFT algorithm.
6. dsp.fftalg(method='stft'): Short-time version of FFT.

In order to make the comparison as fair as possible, please note that the short-time techniques of `shorttime_goertzel`, `shorttime_goertzel_m` and `fftalg(method='stft')` are all implemented in python, not C.  


## Algorithm verification
To verify the correctness of implemented algorithms, you can execute `main.py`.
```python
$ python main.py
```

Download the files from the link below, and modify the path to choose the file you want to analyze.
[Dropbox | Test data](https://www.dropbox.com/sh/w02sfh10sqom8y5/AAC1E5IB7vnfHxn93PHdh9hLa?dl=0)

Ex:
```python
# main.py
def main(pltfig):
...
# filepath = os.path.join(data_dir, 'FOLDER', 'FILE_NAME.EXT')
filepath = os.path.join(data_dir, 'data', 'sig_60Hz.csv')
...
```

Notice that:
1. To get precise runtime of algorithm, please set cb_plt as `None`
2. To see spectrum generated by fft, please set cb_plt as `cbplot.plt_spectrum`


You can also generate a simple signal for analysis.
To do this, try to use the function `export_sig` in `main.py`.

EX:
```python
$ python gensig.py -f [file path] -T [duration] ...
```

|arguments|description|default|
|---|---|---|
|-f|Path of file/data.|None|
|-T|Duration of signal. signal length = T*fs.|100|
|--fs|Sampling rate.|1000|
|--ft|Target frequency.|60|

Notice that the directory of output file was set as `..\data\` by default.

## Benchmark

```python
$ python benchmark.py
```

Run single method/algorithm
```python
$ python benchmark.py -m [method/algorithm name] --ft [target frequency] ...
```

|arguments|description|default|
|---|---|---|
|-m|Name of method/algorithm.(see the table below)|None|
|-f|Path of file/data.|'..\data\rawecg.csv'|
|--fs|Sampling rate.|1000|
|--ft|Target frequency for analysis. (please use comma (\',\') as a delimiter.)|'50,60'|
|--width|Width of window for short time algorithm.|1000|
|--step|Repeated execution times per step.|5|


Supported methods/algorithms (listed in benchfunc.alglist):  

|input|func name|description|can user assign multiple values to `ft`?|
|---|---|---|---|
|go|goertzel|Normal Goertzel algorithm.|N|
|gom|goertzel_m|Normal Goertzel algorithm.|Y|
|stgo|goertzel_st|Short-time version of Goertzel algorithm.|N|
|stgom|goertzel_st_m|Short-time version of Goertzel algorithm.|Y|
|fft|fft|Fast Fourier transform.|Y|
|stft|stft|Short-time version of FFT. (this is not actually the STFT("short time Fourier transform") we known)|Y|
	
## Performance
Data type: int16  
![Fig 01. (data type: int16)](http://i.imgur.com/QK0SWJr.png)  
![Fig 02. Zoomed Fig 01.](http://i.imgur.com/HlMj80n.png)  
Data type: float64
![Fig 03. (data type: float64)](http://i.imgur.com/CLYZwj7.png)  
![Fig 04. Zoomed Fig 03](http://i.imgur.com/GK7JtFN.png)  

## Reference
[wikipedia - Goertzel](https://en.wikipedia.org/wiki/Goertzel_algorithm)  
[stackoverflow - Implementation of Goertzel algorithm in C](http://stackoverflow.com/questions/11579367)  
