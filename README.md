# mmPcGait: Radar Point Cloud Gait Recognition Dataset Under Multi-view and Multi-wearing conditions
mmPcGait is an open-source gait recognition dataset collected with 77-GHz millimetre wave radar. It can be used to study the performance of radar point cloud based gait recognition methods under multi-view and multi-wearing conditions. Our paper "Fine-Grained Spatial-Temporal Gait Recognition Network Based on Millimeter-Wave Radar Point Cloud" is being submitted. Our dataset and codes will be made public in the near future. We hope that our work can serve as a baseline and inspire further research. Following, we introduce the composition and implementation details of this dataset.
## Dataset Description
![scene](IMG/scene.png)
*  **90 person:** 90 volunteers of different ages, gender, heights, weights are involved.<br>
*  **10 views:** 10 different wearing views, including the degree of 0,15,30,45,60,90,300,315,330,345.<br>
*  **3 wearing conditions:** 3 different wearing conditions, including normal walking, carrying a bag and wearing a coat.<br>
## Dataset Implementation
### Radar Configuration
* We have set up a data collection platform with a millimeter-wave radar (TI AWR2243), a camera and a PC. The camera is used for scene record. For completely irradiating human body, radar is placed 1.1m above the ground, which is close to the middle height of human torso. And we collect human walking data in an empty room. 

* The mmWave radar AWR2243 is equipped with MIMO antenna array which consists of 12 transmitters and 16 receivers. Besides, it works in time-division multiplexing multiple-input multiple-output (TDM-MIMO) mode, that means the 12 transmitters emit linear frequency modulated continuous wave (LFMCW)  successively. The parameters of the radars are set as follows:

Parameter | Value 
:----:  | :----:  
Carrier frequency | 77 GHz
Chirp Duration | 15.5 us
Chirp slope | 100 MHz/us 
Bandwidth | 1200 Mhz
Samples Per Chirp | 128
Sampling Frequency | 12.5 Msps
Chirps Per Frame | 128
Frame Duration | 25 ms

* Under these settings, the range resolution is 0.125m, maximum unambiguous range is 18.75m. The velocity resolution is 0.082m/s, maximum unambiguous velocity is 5.24m/s. The azimuth angular resolution is 1.4 degree and the elevation angular resolution is 18 degree.

### Data preprocessing
To generate human point cloud from mmWave radar data, signal processing pipeline in shown in below picture:
![signal processing](IMG/sp.png)
* We firstly perform range and doppler FFT followed by static clutter removal to get RD map. After that, CFAR detection is applied to detect the
high-energy locations in RD map, and the raw point cloud is obtained by applying angle FFT to CFAR detection results.
* We apply a discrete Kalman filter (KF) to get the real target centroid position at current frame. And select a candidate cube area centered at target centroid and discard all other points.
* Finally, density-based spatial clustering for application with noise (DBSCAN) algorithm is executed to further extract the clean target point cloud.

## Data Format
* We have collected radar data from 90 subjects under ten walking views (0,15,30,45,60,90,300,315,330,345) and three wearing conditions (normal walking, carrying bag, wearing coat). For each subject, we have collected 10 groups of data under each walking view, which contain 6 groups of normal walking data (NM#01-NM#06), 2 groups of carrying bag data (BG#01-BG#02) and 2 groups of wearing coat data (CT#01-CT#02). On the whole, we have collected 100 groups of gait data for each subject and our dataset totally contains 9000 groups of gait data.
![dataset](IMG/dataset.png)
* The data files have been named with “SubjectID-WearingCondition-GroupIndex-WalkingView”. For example, data file “001-nm-02-090” represents the second group of normal walking data under 90 degrees view for subject 1. Moreover, csv format is applied to store the point cloud data. In csv file, each row records the data of a single radar point. And our csv file totally contains 7 columns, i.e., Frame, Num, X, Y, Z, Doppler and RCS. The first two columns represent the frame index and the points number within that frame. The remaining columns represent the attributes of radar point: X, Y, Z represent the 3D coordinate (m), Doppler represent the doppler velocity (m/s), and RCS represent the radar cross section.

## Data Illustration
![Fig 8](IMG/pc.png)
## Code
The Code will be published in the near future.
## Citation