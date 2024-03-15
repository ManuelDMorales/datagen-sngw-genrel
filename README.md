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
     |___ Toolbox.py
     |___ WaveformGenRel_Explorer.ipynb
|___ LICENCE
|___ README.md
```

Run the Jupyter notebooks in the following order:

`Noise_Explorer.ipynb` for downloading interferometric noise data from LIGO and Virgo detectors (segments of 4,096s), exploring time series and power spectral densities, and locally saving noise segments. Open data was obtained from the website of the GWOSC.

`WaveformGenRel_Explorer.ipynb` for analyzing and preparing general relativistic waveforms. Their morphologies in the time domain and the time-frequency domain are explored. Scalograms (applying Morlet wavelet transform) are plotted. Resampling and rescaling are also applied, to have dimensionless strain time series with a sampling frequency of 4,094Hz. In addition, estimation HFF slopes are performed and, finally, other features are removed/filtered to save the waveforms such that they only have the HFF.

In addition, the file `Toolbox.py` is included, which contains specific functions for the notebooks.

# Important instructions

1. All scripts were run locally, then you will need to edit path locations in cells for read files. The general relativistic waveforms used in this work are available in the public folder [Waveforms_mod](https://drive.google.com/drive/folders/1GuOWzGEHlAedqWZcCAShcAanpDnC1bIy?usp=sharing). Download this folder and locate it in your local machine as shown in the tree detailed in implementation structure.
