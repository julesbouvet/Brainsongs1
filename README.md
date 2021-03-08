# Brainsong1

This code was written for the Brain Songs project during an internship at IMT Atlantique supervized by Nicolas Farrugia and Giulia Lioi. 


## Block analysis 

![alt text](https://github.com/alixlam/Brainsongs1/blob/master/block_data/experimental_protocol.png)

### Method for preprocessing and analysis :
Preprocessing:
The preprocessing was done using the MNE-python toolbox by Gramfort et al. [1]
First, the signals were bandpassed filtered with FIR (Finite Impulse Response) keeping the frequency between 1.0 and 40 Hz.
To reduce the contamination of the EEG signal by eye movement artefacts, IC’s components were estimated on the continuous data. 
Using a prefrontal electrode as an Electro-Oculography channel, we ran an autodetection algorithm to find the IC’s components that best match the ‘EOG’ channel. The ICA components that correlate strongly with the EOG signal were then removed and the EEG signal was recomposed with the remaining IC’s components.
Finally, the new signal was segmented into consecutive epochs of 2 seconds, epochs where the signal’s amplitude of one or more channels exceeded a predefined threshold were removed in order to keep 90% of data. 

Data analysis:
Individual Alpha Frequency (IAF) [2] was determined by finding the individual dominant EEG frequency in the baseline signal. Based on that frequency we defined the Alpha, Theta and beta bands respectively (7.5-12.5, 4.5-7.5, 12.5-25).
To conduct our analysis, we estimated the power spectral density using multitapers and computed the powers in the different frequency bands. The 2s epochs were labelled with their corresponding state (free, low, and fast) and we ran ANOVA. 

## Continuous data

### Method for preprocessing : 
The preprocessing was the same as previously

Data analysis:
We conducted a time-frequency analysis using Morlet Wavelet to compute the correlation between the subjective time feedback and the evolution of power of different frequency bands (alpha, beta and theta) in time.
Then we tried the Spoc algorithm [3] to see if we could predict the variable subjective time with the EEG signal. 

## Data Analysis

### Aim of this analysis :
The aim of this additional analysis was to characterize the density (number of notes played every second) and volume of the musical performance of CR on the three performances and to assess their impact on Subjective Time Resolution (STR) and EEG rhythms. This analysis was performed separately for each performance.   

### Method for estimating note density :
First, we separated the different musical instruments recording before counting notes. For each performance there were three instruments: clarinet (CR), drums and double bass for the first two performances and clarinet, double bass and trumpet for the third performances. As we are focusing on the performance of CR (specifically, the number of notes he played) we separated the clarinet from the two other instruments. We used the 4 stem source separation pretrained model from the library Spleeter (Stoter et al. 2018, Hennequin et al. 2020) developed and open-sourced by Deezer (https://github.com/deezer/spleeter). Spleeter allowed us to split music instruments leveraging pre-trained neural networks implemented in TensorFlow.  Next, note onset detection was performed on the clarinet track separated during step 1. For this, we used an onset_detection method available in the librosa package  (https://librosa.org/doc/main/generated/librosa.onset.onset_detect.html#librosa.onset.onset_detect) that works by detecting local peaks in the envelope strength, using hyperparameters set on a large music database (https://github.com/CPJKU/onset_db). We validated note density  extraction qualitatively by retrieving several samples for each note density bin and checked qualitatively if the note density was well estimated.   

### Analysis between note density and STR :
Then, we assessed the impact of note density during the musical performance on behavioral (STR). First, we estimated the distribution of note density values across performances. As there are many moments during which CR does not play, we get an important proportion of note density values of 0. However, non-zero density values followed an approximately gaussian distribution, that we binned in three categories according to XXXX. We tested the hypothesis whether CR plays more notes per second in low or high STR by performing a Wilcoxon test on non zero note densities. We also compare qualitatively the count of note densities equal to zero (i.e. moments during which CR does not play for one second) in low and high STR. We also tested the hypothesis that the volume of CR musical performance was related to STR. To this end we calculated the RMS energy of each performance using the librosa function librosa.rms() and compared RMS distributions for low and high STR using Welch's t-test.   

### Analysis between note density and EEG :
Finally, to assess if note density had a significant impact on EEG power in different frequency bands we used the four above mentioned categories: silence (note density equal to zero), low density, medium density and high density, and computed alpha, beta and theta EEG power in each category. We tested for associations between power and note density using a Kruskall Wallis test, and performed pairwise  t-tests for post-hoc comparisons.


## References
[1] A. Gramfort, M. Luessi, E. Larson, D. Engemann, D. Strohmeier, C. Brodbeck, R. Goj, M. Jas, T. Brooks, L. Parkkonen, M. Hämäläinen, MEG and EEG data analysis with MNE-Python, Frontiers in Neuroscience, Volume 7, 2013, ISSN 1662-453X, [DOI]


[2] Klimesch W. EEG alpha and theta oscillations reflect cognitive and memory performance: a review and analysis. Brain Res Brain Res Rev. 1999;29(2-3):169-195. doi:10.1016/s0165-0173(98)00056-3

[3] Dahne, S., et al (2014). SPoC: a novel framework for relating the amplitude of neuronal oscillations to behaviorally relevant parameters. NeuroImage, 86, 111-122.

