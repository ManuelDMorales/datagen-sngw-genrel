# Summary

Dataset generator of strain (time series) samples containing general relativistic gravitational waves from core-collapse supernovae added to noise from LIGO-Virgo interferometric detectors. Jupyter notebooks developed in Python and written by Manuel D. Morales (e-mail: <manueld.morales@academicos.udg.mx>).


# Science background

<b><ins>Interferometric noise</ins></b>

For this implementation, we used real noise from LIGO (L1, H1) and Virgo (V1) interferometric detectors from O3b run. This is a non-Gaussian and non-stationary noise stored in strain time series of 4,096s of duration, sampled at 16,384Hz and 4,096Hz. For our implementation, we choose the second sampling option. Data available on the [Gravitational Wave Open Science Center](https://gwosc.org/).

<b><ins>General relativistic waveforms</ins></b>

We draw on three openly distributed waveforms from three CCSN numerical relativity simulations: [Andresen 2019 m15nr h+](https://doi.org/10.1093/mnras/stz990) (from a 3D code), [Morozova 2018 M13_SFHo h+](https://doi.org/10.3847/1538-4357/aac5f1) (from a 2D code), and [Cerda-Duran 2013 fiducial h+](https://iopscience.iop.org/article/10.1088/2041-8205/779/2/L18) (from a 2D code).

As shown in Fig. 1, the time-frequency representation of these waveforms shows different features, and the most important (in terms of energy) is the High Frequency Feature (HFF). This feature has an increasing monotonic frequency profile in time, which, at first order, can be considered linear. Based on state-of-the-art CCSN models, the HFF feature carries information about the pulsation of the proto-neutron star (PNS) created in the CCSN. For this implementation, we filtered all features other than the HFF in the waveforms, and then we injected the resulting filtered waveforms into the noise segments. Like the implementation developed in our project [datagen-sngw-phen](https://github.com/ManuelDMorales/datagen-sngw-phen), we chose these waveforms because their HFF slope belongs to three classes defined there, respectively: Andresen (class 1, 1,620 =< HFF slope =< 4,990), Morozova (class 2, 1,450 =< HFF slope < 1,620), and Cerda-Duran (class 3, 950 =< HFF slope < 1,450).

# Implementation structure

```
datagen-sngw-genrel
|___ Codes
     |___ Noise_Explorer.ipynb
     |___ Process_GenRelWaveforms.ipynb
     |___ Toolbox.py
     |___ WaveformGenRel_Explorer.ipynb
|___ Datasets
|___ Waveforms_mod
     |___ Phen
     |___ GenRel
|___ Waveforms_orig
     |___ GenRel
|___ LICENCE
|___ README.md
```

Run the Jupyter notebooks in the following order:

`Noise_Explorer.ipynb` for downloading interferometric noise data from LIGO and Virgo detectors (segments of 4,096s), exploring time series and power spectral densities, and locally saving noise segments. Open data was obtained from the website of the GWOSC.

`WaveformGenRel_Explorer.ipynb` for analyzing and preparing general relativistic waveforms. Their morphologies in the time domain and the time-frequency domain are explored. Scalograms (applying Morlet wavelet transform) are plotted. Resampling and rescaling are also applied, to have dimensionless strain time series with a sampling frequency of 4,094Hz. In addition, estimation HFF slopes are performed and, finally, other features are removed/filtered to save the waveforms such that they only have the HFF.

`Process_GenRelWaveforms.ipynb` for applying injections, data conditioning (whitened, band-pass filtering), and generating a dataset of window strain samples. Each one of these windows has the same length Twin<4,096s, contains noise plus a general relativistic CCSN waveform, and is saved in a .txt file. In addition, a log.dat file is saved which contains information about injected signals in all windows.

In addition, the code `Toolbox.py` is included, which contains specific functions for the notebooks.

# Important instructions

1. All scripts were run locally, then you will need to edit path locations in cells for read files. The general relativistic waveforms used in this work are available in the public folder [Waveforms_orig](https://drive.google.com/drive/folders/1IIUoJQ5gyzyqX7q3UhA53ws6la35QXE9?usp=sharing). Download this folder and locate it in your local machine as shown in the tree detailed in implementation structure.

2. In the input of `WaveformGenRel_Explorer.ipynb` script (subsection 0.2), you can set the distance from the CCSN through the input parameter set_distance. Depending on this value (and the noise realization), the distribution of SNR values alongthe datasets can change.

3. In `Process_GenRelWaveforms.ipynb`, a specific waveform is selected to be injected in a noise strain data segment. Each injection is performed in a location inj_time +/- jitter (in seconds), in which jitter is a random value in the range [inj_time - jitter_lim, inj_time + jitter_lim]. Jitter_lim is also an input parameter that can be set in subsection 2.1 (ensure that jim_lim << dt_inj).

4. The time window duration of window strain samples is Twin = wf_max + alpha, where wf_max is the duration of the longest injected waveform in the previous project [datagen-sngw-phen](https://github.com/ManuelDMorales/datagen-sngw-phen), where alpha is an input parameter. Then, to appropriately run the `Process_GenRelWaveforms.ipynb` script, download the public subfolder [Phen](https://drive.google.com/drive/folders/1zBhZcY6Gesn3Tsdb4psPeH4uPLRHGH5s?usp=sharing) and locate it inside the folder Waveforms_mod as shown in the tree of implementation structure.
   
5. If you want to change the duration of window samples, you can change alpha input parameter in subsection 4.1 of `Process_PhenWaveforms.ipynb` script. Moreover, if you want to change the value of wf_max (for instance, only considering the injected general relativistic waveforms), you can put the new value in subsection 4.2 of the aforementioned script.

6. Before running `Process_PhenWaveforms.ipynb` to perform injections (and to generate window samples) covering a whole noise segment of 4,096s, it is highly recommendable to apply it only on a reduced segment of a few tens of seconds. To perform this check, set reduce_segment=1 and reduced_time_n equal to the duration (in seconds) of the reduced segment, in subsection 2.1 of the script.
   
7. For the check previously mentioned, it is also useful to set input parameters doplot_spectrogram=1 (subsection 3.4.1) and set_doplots=1 (subsection 4.1). This output plots of windows samples in time and time-frequency domains. Only set this when working with a reduced segment; outputting plots working with the complete segment of 4,096s considerably slows down the script execution. 
