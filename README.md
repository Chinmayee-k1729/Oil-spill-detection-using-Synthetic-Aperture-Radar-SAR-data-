# OIL SPILL DETECTION ON SEA SURFACE USING SAR IMAGES
# Overview

The project handles oil spill detection in Synthetic Aperture Radar (SAR) images by a whole pipeline starting from physics understanding, traditional image processing to machine learning modeling. The approach addresses pre-processing the data, engineering the features, and the classification through a mix of both classical and contemporary approaches.

## Physics Behind SAR and Oil Spill Detection

Synthetic Aperture Radar (SAR) is an active Earth observation system that emits microwave signals and records the backscatter from the Earth's surface. SAR imagery over water is responsive to changes in surface roughness:

- **Clean Water:** Is bright due to strong backscatter from capillary waves.
- **Oil-Covered Water:** Is dark patches as oil suppresses these waves, with reduced backscatter.

Oil spill detection exploits this contrast, but problems arise due to lookalikes (e.g., low wind areas, biogenic slicks) that also look dark.

## Pipeline and Logic

### 1. Data Acquisition & Reading

- SAR data used.
- **Libraries:**
  - [`opencv`](https://opencv.org/) (cv2) for image processing and visualization.
  - [`rasterio`](https://rasterio.readthedocs.io/) for reading and processing geospatial raster data.

### 2. Preprocessing & Speckle Noise Reduction

SAR images contain speckle-noise. We experimented with a few of the denoising filters:

- **Mean Filter:** Basic averaging, helps to remove random noise.
- **Lee Filter:** Adaptive filter that maintains edges, reduces speckle.
- **Kuan Filter:** Similar to Lee, but utilizes a different statistical approach.
- **Gaussian Filter:** Blurs images through averaging using a Gaussian kernel, reduces high-frequency noise.
- **Bilateral Filter:** Smoothing without edge loss, by considering both spatial and intensity changes.

After applying these filters and plotting their histograms, the Kuan filter gives a smoother, narrower histogram with better speckle reduction and detail preservation than all the other filters so we decided to go with kaun filter for noise reduction.

### 3. Image Enhancement & Feature Extraction

- **Thresholding:** Applied various thresholding techniques (global, adaptive) to segment dark patches.
- **Edge Detection:** Employed operators like Canny and Sobel to set patch edges.
- **Morphological Operations:** Postprocessing operation with dilation, erosion, opening, and closing to remove detected regions noise and clean up.

### 4. Machine Learning Modeling

After feature extraction of oil spill candidates:

- Extracted features (shape, texture, size, intensity statistics).
- Built and tested machine learning models:
- Classical models.
- Trained and validated on annotated data.

### 5. Postprocessing

- Applied morphological methods to fill in the mask of detected oil spills.
- Removed tiny artifacts and holes in the detected region. 
