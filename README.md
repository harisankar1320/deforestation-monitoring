# Deforestation-monitoring - Near-real-time deforestation monitoring using Sentinel-1/2 time series fusion


## Project Overview

Download and preprocess Sentinel-1 and Sentinel-2 imagery
Generate cloud-free NDVI/NBR time series
Compute Sentinel-1 VH/VV backscatter statistics
Detect deforestation signals using  Cumulative Sum(CUSUM) and Long Sho
Long Short-Term Memory (LSTM) based neural network and compare 
statistical and deep learning approaches for temporal change detection

# Study Area
The study are of this work is Rondonia, Brazil
Cordinates = min_lon: -62.9, min_lat: -11.7, max_lon: -62.1, max_lat: -11.0


# Dataset

## Sentinel-1 SAR

- Product Type: GRD
- Polarizations: VV and VH

### SNAP preprocessing workflow

- Apply Orbit File
- Thermal Noise Removal
- Calibration
- Speckle Filtering
- Terrain Correction
- Subsetting

## Sentinel-2 Optical

Bands used:

| Band | Description |
|---|---|
| B04 | Red |
| B08 | NIR |
| B11 | SWIR1 |
| B12 | SWIR2 |
| SCL | Scene Classification Layer |

Indices generated:

- NDVI
- NBR
- NDMI

---

## Methodology (preprocessing, time series, CUSUM, LSTM)

### Sentinel-2 Preprocessing
Cloud masking was applied using the Scene Classification Layer (SCL) 
from Level-2A products, retaining only vegetation, bare soil and water 
pixels. Spectral indices NDVI, NDMI and NBR were computed from the 
cloud-masked bands and saved as GeoTIFF files.

### Sentinel-1 Preprocessing
Raw GRD scenes were processed in ESA SNAP using a graph consisting of 
Apply Orbit File, Thermal Noise Removal, Radiometric Calibration, 
Speckle Filtering (Lee 5x5) and Range Doppler Terrain Correction. 
Output was projected to UTM Zone 20S (EPSG:32720) at 10m resolution.

### Time Series Fusion
Mean VH backscatter and mean NDVI were extracted per scene and 
organised into a unified time series DataFrame sorted by date. 
Sentinel-1 provides cloud-free observations every 12 days while 
Sentinel-2 fills spectral detail when cloud-free.

### Change Detection
CUSUM (Cumulative Sum) algorithm was applied to the VH backscatter 
time series to detect sustained deviations from the baseline forest 
signal. A change point was detected at 2023-09-30, confirmed by 
concurrent NDVI reduction in Sentinel-2 observations.

## Project Status

This project is actively under development.

### Completed
- Sentinel-2 data download and preprocessing (cloud masking, NDVI/NDMI/NBR)
- Sentinel-1 data download and SNAP preprocessing (calibration, speckle filter, terrain correction)
- Time series fusion (area-wide mean VH and NDVI)
- CUSUM change detection (area-wide)

### In Progress
- Per-pixel CUSUM change detection and deforestation mapping
- Carbon loss estimation using IPCC emission factors

### Planned
- LSTM-based deep learning change detection
- Comparison of CUSUM vs LSTM performance
- QGIS visualization and publication-quality maps