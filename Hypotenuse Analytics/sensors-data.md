# Cleaning and preprocessing multi-sensor and satellite data for machine learning

## Overview
Effective preprocessing transforms raw optical, SAR, LiDAR, thermal, UAV and in-situ streams into consistent, low-uncertainty inputs for machine learning. Preprocessing must correct sensor physics and geometry, harmonize scale/units across sources, remove or mask non-surface signals (clouds, speckle, noise), and prepare tiled/annotated datasets with provenance so model training and validation reflect true task uncertainty [1].

## Typical sensor suite (brief)
- Optical multispectral / hyperspectral: multispectral (3-10 bands) and hyperspectral (hundreds of narrow bands) instruments differ in spectral and radiometric detail and thus require different radiometric/atmospheric handling and dimensionality reduction strategies [2].  
- SAR (C, L, X bands etc.): active microwave imaging works day/night and through clouds but has speckle and requires radar-geometry processing and terrain correction steps unique to SAR [3].  
- LiDAR: laser ranging produces 3-D point clouds used for DSM/DTM generation; point-level denoising and classification precede rasterization or feature extraction [4].  
- Thermal TIR: lower spatial resolution and different temporal characteristics than optical sensors; thermal products often suffer from spatial discontinuities (clouds, limited observations) [29].  
- UAV/drone imagery: very high spatial resolution RGB/multispectral/hyperspectral and on-board IMU/GNSS; requires local geometric correction and image-level denoising [5].  
- In-situ / time-series sensors: ground truth and calibration/validation data; used to calibrate and quantify model uncertainty [6].

(References for above list: [2], [3], [4], [29], [5], [6].)

## Recommended ordered preprocessing steps (sensor-specific then cross-sensor)

1. Raw conversion and radiometric calibration
   - Convert digital numbers (DN) to radiance/reflectance as a first step for optical sensors, using sensor calibration metadata before further correction [7]. (Cite: [7].)

2. Atmospheric and surface reflectance correction (optical)
   - Use physics-based processors for bottom-of-atmosphere reflectance when available (examples: Sen2Cor for Sentinel-2, Py6S/MAJA as alternatives/tools). Configure atmospheric LUTs / aerosol options per scene where possible [8], [9], [10]. (Cite: [8], [9], [10].)

3. SAR: radar-geometry processing, radiometric calibration, speckle handling, orthorectification
   - Preserve radar geometry until radar-specific steps (calibration, radiometric terrain flattening, multilook) are applied; perform terrain correction/orthorectification after radar-domain processing because resampling changes SAR statistics [11], [12].  
   - Apply speckle filtering (classical: Lee, Frost, Kuan; or ML-based denoisers). Window size and filter choice materially affect texture and ENL; adaptive or learned choices can outperform fixed windows in many regions [13], [14]. (Cite: [11], [12], [13], [14].)

4. Point-cloud (LiDAR) denoising and rasterization
   - Remove noise/outliers (statistical filters) and classify ground/non-ground before interpolating to DSM/DTM or deriving features (height metrics, intensity) [15], [16]. (Cite: [15], [16].)

5. Cloud and cloud-shadow detection & masking
   - Detect clouds probabilistically (e.g., s2cloudless thresholds) and build cloud-shadow masks using dark NIR pixels plus geometric projection; mask before harmonization steps that remove QA bands in some pipelines [19]. (Cite: [19].)

6. Geometric correction, reprojection, co-registration and resampling
   - Co-register sources to a common CRS and target resolution after their sensor-specific corrections; when fusing sensors (e.g., Landsat+Sentinel), apply bandpass and BRDF harmonization plus BRDF/temporal adjustments used by integrated products (HLS) to reduce inter-sensor bias [17], [18]. (Cite: [17], [18].)

7. Temporal alignment and gap-filling
   - Build regular time stacks by selecting/aggregating scenes (median, compositing) only after cloud masking and reflectance correction; integrate in-situ data for bias checks and model calibration [6], [17]. (Cite: [6], [17].)

8. Scale harmonization, resampling and patching
   - Choose target pixel size that balances spatial detail and noise; create tiles/patches for ML with overlap as needed. For large scenes, use chunked cloud-friendly stores (Zarr) and tile generation workflows to limit RAM requirements [20], [21], [22]. (Cite: [20], [21], [22].)

9. Feature extraction and dimensionality reduction
   - For hyperspectral data apply spectral feature extraction (1-D convolutions, index computation, endmember methods) or supervised reduction; for LiDAR derive height metrics; for SAR compute polarimetric decompositions where relevant [2], [16].

10. Label and metadata QA, augmentation and class balancing
    - Ensure label provenance, remove erroneous labels, and document acquisition conditions. Use augmentation/oversampling carefully; deep models substantially benefit from large labeled datasets but augmentation hyperparameters and their impact are not fully standardized in the reviewed evidence [25]. (Cite: [25].)

## Algorithms, methods and hyperparameter guidance
- Prefer physics-based atmospheric correction where available for optical workflows; configure LUTs/atmospheric parameters rather than relying on uncorrected DN inputs [8], [9], [10]. (Cite: [8], [9], [10].)  
- For SAR, classical speckle filters (Lee, Frost) remain effective; window sizes trade off speckle suppression vs. feature preservation and may be adaptively selected or replaced by DL denoisers (e.g., SAR2SAR) when training data and compute permit [11], [12], [13], [14]. (Cite: [11], [12], [13], [14].)  
- LiDAR outlier removal commonly uses statistical filters (e.g., PDAL defaults demonstrated in practice); tune mean_k and multiplier to scene density [15]. (Cite: [15].)  
- For large-scale ML, use libraries that enable lazy/chunked I/O and on-the-fly augmentation (rasterio/xarray/eo-learn + PyTorch/TorchGeo) to scale training pipelines [23], [24], [25]. (Cite: [23], [24], [25].)

## File formats, metadata and toolchain practices
- Use cloud-friendly tiled formats such as Cloud-Optimized GeoTIFF (COG) or chunked Zarr stores for large datasets; publish STAC metadata to record assets and instruments [22], [18], [28]. (Cite: [22], [18], [28].)  
- Recommended toolchain components (examples supported in reviewed evidence): SNAP/Graph Processing Tool for radar and atmospheric toolboxes, eo-learn and rasterio/rioxarray for Pythonic ingestion, PDAL for LiDAR, and PyTorch/TorchGeo for model training [24], [23], [15], [25]. (Cite: [24], [23], [15], [25].)

## Measuring and reporting data quality and downstream impact
- Report sensor-level accuracy (radiometric, geometric), mask coverage (fraction cloudy), and effects of processing choices (e.g., ENL for speckle reduction). Include uncertainty quantification and ablation experiments that show how preprocessing choices change model metrics; the EO literature identifies a lack of standardized uncertainty reporting and recommends explicit quantification [14], [26], [27]. (Cite: [14], [26], [27].)  
- Use in-situ data for calibration and independent validation and report R²/RMSE or confusion matrices stratified by scene conditions where possible [6], [17]. (Cite: [6], [17].)

## Evidence gaps
- No reviewed evidence prescribes standardized augmentation/oversampling hyperparameters for fused multi-sensor inputs; empirical tuning remains necessary.  
- Practical, validated recipes for large-volume inter-sensor radiometric normalization beyond HLS-style bandpass and BRDF adjustments are limited in the reviewed corpus.  
- Detailed guidance tying preprocessing choices quantitatively to downstream model hyperparameters (batch size, learning rate schedules) is not present in the gathered evidence.

## Practical starting recipe (concise)
1. Convert DN → radiance/reflectance (sensor metadata). Apply Sen2Cor/MAJA or equivalent for optical scenes where supported [8], [9], [10].  
2. Mask clouds (s2cloudless for Sentinel-2) and shadows [19].  
3. SAR: do radar-domain calibration → speckle filtering → terrain correction/orthorectification [11], [12], [13].  
4. LiDAR: outlier removal and classification, then rasterize height metrics [15].  
5. Co-register all layers to common CRS and resolution, apply inter-sensor bandpass/BRDF harmonization when fusing [17], [18].  
6. Tile into patches using chunked storage (Zarr/COG) and create provenance metadata (STAC/ISO fields) [22], [18], [28].  
7. Validate with in-situ data; run ablation experiments to quantify the effect of each preprocessing step on final model metrics [6], [27]. (Cite: all listed where applicable.)

## References
[1] https://appliedsciences.nasa.gov/sites/default/files/2023-02/Fundamentals_of_RS.pdf  
[2] https://mdpi.com/1424-8220/19/22/4837  
[3] https://sentinel-asia.org/e-learning/TrainingMaterials/GLOF_training/D01_L01_Introduction_to_Remote_Sensing.pdf  
[4] https://coast.noaa.gov/data/digitalcoast/pdf/lidar-101.pdf  
[5] https://mdpi.com/2072-4292/14/17/4298  
[6] https://ioccg.org/wp-content/uploads/2024/12/1.5-lecture2_rf.pdf  
[7] https://geo-informatie.nl/courses/grs10306/Clevers/RS%20CH4%20Preprocessing/RS%20Ch%204%20RS%20Preprocessing.pdf  
[8] https://seadas.gsfc.nasa.gov/help-9.2.0/Sen2CorHelp.html  
[9] https://py6s.readthedocs.io/en/latest/params.html  
[10] https://step.esa.int/main/wp-content/help/versions/13.0.0/snap-toolboxes/eu.esa.opt.opttbx.sta.adapters.help/installation/MAJAInstallation.html  
[11] https://forum.step.esa.int/t/radiometric-geometric-correction-workflow/2540  
[12] https://dges.carleton.ca/courses/IntroSAR/Winter2019/SECTION%207A%20-%20Carleton%20SAR%20Training%20-%20SAR_processing_Sentinal1%20-%20FINAL.pdf  
[13] https://jees.kr/journal/view.php?doi=10.26866%2Fjees.2024.2.r.220  
[14] https://egusphere.copernicus.org/preprints/2025/egusphere-2025-3726/egusphere-2025-3726.pdf  
[15] https://lizardtech.com/post/step-by-step-pdal-python-workflow-for-lidar-data-processing  
[16] https://mdpi.com/2072-4292/15/16/4112  
[17] https://mdpi.com/2072-4292/16/15/2695  
[18] https://developmentseed.org/scaling_science/docs/Open_formats_tools.html  
[19] https://developers.google.com/earth-engine/tutorials/community/sentinel-2-s2cloudless  
[20] https://forums.fast.ai/t/split-large-satellite-image-into-tiles-patches/32039  
[21] https://medium.com/@tahjudil.witra/from-satellite-data-to-machine-learning-simplifying-data-pipelines-with-a-cloud-based-approach-2c810cf30b2c  
[22] https://earthdata.nasa.gov/about/esdis/esco/standards-practices/cloud-optimized-geotiff  
[23] https://github.com/sentinel-hub/eo-learn-examples  
[24] https://theoj.org/joss-papers/joss.03337/10.21105.joss.03337.pdf  
[25] https://pytorch.org/blog/geospatial-deep-learning-with-torchgeo  
[26] https://nature.com/articles/s41598-024-65954-w  
[27] https://ris.utwente.nl/ws/portalfiles/portal/488727903/2312.05327v1.pdf  
[28] https://github.com/radiantearth/stac-spec/blob/master/commons/common-metadata.md  
[29] https://sciencedirect.com/science/article/pii/S2352938523000034# Cleaning and preprocessing multi-sensor and satellite data for machine learning

## Overview
Effective preprocessing transforms raw optical, SAR, LiDAR, thermal, UAV and in-situ streams into consistent, low-uncertainty inputs for machine learning. Preprocessing must correct sensor physics and geometry, harmonize scale/units across sources, remove or mask non-surface signals (clouds, speckle, noise), and prepare tiled/annotated datasets with provenance so model training and validation reflect true task uncertainty [1].

## Typical sensor suite (brief)
- Optical multispectral / hyperspectral: multispectral (3-10 bands) and hyperspectral (hundreds of narrow bands) instruments differ in spectral and radiometric detail and thus require different radiometric/atmospheric handling and dimensionality reduction strategies [2].  
- SAR (C, L, X bands etc.): active microwave imaging works day/night and through clouds but has speckle and requires radar-geometry processing and terrain correction steps unique to SAR [3].  
- LiDAR: laser ranging produces 3-D point clouds used for DSM/DTM generation; point-level denoising and classification precede rasterization or feature extraction [4].  
- Thermal TIR: lower spatial resolution and different temporal characteristics than optical sensors; thermal products often suffer from spatial discontinuities (clouds, limited observations) [29].  
- UAV/drone imagery: very high spatial resolution RGB/multispectral/hyperspectral and on-board IMU/GNSS; requires local geometric correction and image-level denoising [5].  
- In-situ / time-series sensors: ground truth and calibration/validation data; used to calibrate and quantify model uncertainty [6].

(References for above list: [2], [3], [4], [29], [5], [6].)

## Recommended ordered preprocessing steps (sensor-specific then cross-sensor)

1. Raw conversion and radiometric calibration
   - Convert digital numbers (DN) to radiance/reflectance as a first step for optical sensors, using sensor calibration metadata before further correction [7]. (Cite: [7].)

2. Atmospheric and surface reflectance correction (optical)
   - Use physics-based processors for bottom-of-atmosphere reflectance when available (examples: Sen2Cor for Sentinel-2, Py6S/MAJA as alternatives/tools). Configure atmospheric LUTs / aerosol options per scene where possible [8], [9], [10]. (Cite: [8], [9], [10].)

3. SAR: radar-geometry processing, radiometric calibration, speckle handling, orthorectification
   - Preserve radar geometry until radar-specific steps (calibration, radiometric terrain flattening, multilook) are applied; perform terrain correction/orthorectification after radar-domain processing because resampling changes SAR statistics [11], [12].  
   - Apply speckle filtering (classical: Lee, Frost, Kuan; or ML-based denoisers). Window size and filter choice materially affect texture and ENL; adaptive or learned choices can outperform fixed windows in many regions [13], [14]. (Cite: [11], [12], [13], [14].)

4. Point-cloud (LiDAR) denoising and rasterization
   - Remove noise/outliers (statistical filters) and classify ground/non-ground before interpolating to DSM/DTM or deriving features (height metrics, intensity) [15], [16]. (Cite: [15], [16].)

5. Cloud and cloud-shadow detection & masking
   - Detect clouds probabilistically (e.g., s2cloudless thresholds) and build cloud-shadow masks using dark NIR pixels plus geometric projection; mask before harmonization steps that remove QA bands in some pipelines [19]. (Cite: [19].)

6. Geometric correction, reprojection, co-registration and resampling
   - Co-register sources to a common CRS and target resolution after their sensor-specific corrections; when fusing sensors (e.g., Landsat+Sentinel), apply bandpass and BRDF harmonization plus BRDF/temporal adjustments used by integrated products (HLS) to reduce inter-sensor bias [17], [18]. (Cite: [17], [18].)

7. Temporal alignment and gap-filling
   - Build regular time stacks by selecting/aggregating scenes (median, compositing) only after cloud masking and reflectance correction; integrate in-situ data for bias checks and model calibration [6], [17]. (Cite: [6], [17].)

8. Scale harmonization, resampling and patching
   - Choose target pixel size that balances spatial detail and noise; create tiles/patches for ML with overlap as needed. For large scenes, use chunked cloud-friendly stores (Zarr) and tile generation workflows to limit RAM requirements [20], [21], [22]. (Cite: [20], [21], [22].)

9. Feature extraction and dimensionality reduction
   - For hyperspectral data apply spectral feature extraction (1-D convolutions, index computation, endmember methods) or supervised reduction; for LiDAR derive height metrics; for SAR compute polarimetric decompositions where relevant [2], [16].

10. Label and metadata QA, augmentation and class balancing
    - Ensure label provenance, remove erroneous labels, and document acquisition conditions. Use augmentation/oversampling carefully; deep models substantially benefit from large labeled datasets but augmentation hyperparameters and their impact are not fully standardized in the reviewed evidence [25]. (Cite: [25].)

## Algorithms, methods and hyperparameter guidance
- Prefer physics-based atmospheric correction where available for optical workflows; configure LUTs/atmospheric parameters rather than relying on uncorrected DN inputs [8], [9], [10]. (Cite: [8], [9], [10].)  
- For SAR, classical speckle filters (Lee, Frost) remain effective; window sizes trade off speckle suppression vs. feature preservation and may be adaptively selected or replaced by DL denoisers (e.g., SAR2SAR) when training data and compute permit [11], [12], [13], [14]. (Cite: [11], [12], [13], [14].)  
- LiDAR outlier removal commonly uses statistical filters (e.g., PDAL defaults demonstrated in practice); tune mean_k and multiplier to scene density [15]. (Cite: [15].)  
- For large-scale ML, use libraries that enable lazy/chunked I/O and on-the-fly augmentation (rasterio/xarray/eo-learn + PyTorch/TorchGeo) to scale training pipelines [23], [24], [25]. (Cite: [23], [24], [25].)

## File formats, metadata and toolchain practices
- Use cloud-friendly tiled formats such as Cloud-Optimized GeoTIFF (COG) or chunked Zarr stores for large datasets; publish STAC metadata to record assets and instruments [22], [18], [28]. (Cite: [22], [18], [28].)  
- Recommended toolchain components (examples supported in reviewed evidence): SNAP/Graph Processing Tool for radar and atmospheric toolboxes, eo-learn and rasterio/rioxarray for Pythonic ingestion, PDAL for LiDAR, and PyTorch/TorchGeo for model training [24], [23], [15], [25]. (Cite: [24], [23], [15], [25].)

## Measuring and reporting data quality and downstream impact
- Report sensor-level accuracy (radiometric, geometric), mask coverage (fraction cloudy), and effects of processing choices (e.g., ENL for speckle reduction). Include uncertainty quantification and ablation experiments that show how preprocessing choices change model metrics; the EO literature identifies a lack of standardized uncertainty reporting and recommends explicit quantification [14], [26], [27]. (Cite: [14], [26], [27].)  
- Use in-situ data for calibration and independent validation and report R²/RMSE or confusion matrices stratified by scene conditions where possible [6], [17]. (Cite: [6], [17].)

## Evidence gaps
- No reviewed evidence prescribes standardized augmentation/oversampling hyperparameters for fused multi-sensor inputs; empirical tuning remains necessary.  
- Practical, validated recipes for large-volume inter-sensor radiometric normalization beyond HLS-style bandpass and BRDF adjustments are limited in the reviewed corpus.  
- Detailed guidance tying preprocessing choices quantitatively to downstream model hyperparameters (batch size, learning rate schedules) is not present in the gathered evidence.

## Practical starting recipe (concise)
1. Convert DN → radiance/reflectance (sensor metadata). Apply Sen2Cor/MAJA or equivalent for optical scenes where supported [8], [9], [10].  
2. Mask clouds (s2cloudless for Sentinel-2) and shadows [19].  
3. SAR: do radar-domain calibration → speckle filtering → terrain correction/orthorectification [11], [12], [13].  
4. LiDAR: outlier removal and classification, then rasterize height metrics [15].  
5. Co-register all layers to common CRS and resolution, apply inter-sensor bandpass/BRDF harmonization when fusing [17], [18].  
6. Tile into patches using chunked storage (Zarr/COG) and create provenance metadata (STAC/ISO fields) [22], [18], [28].  
7. Validate with in-situ data; run ablation experiments to quantify the effect of each preprocessing step on final model metrics [6], [27]. (Cite: all listed where applicable.)

## References
[1] https://appliedsciences.nasa.gov/sites/default/files/2023-02/Fundamentals_of_RS.pdf  
[2] https://mdpi.com/1424-8220/19/22/4837  
[3] https://sentinel-asia.org/e-learning/TrainingMaterials/GLOF_training/D01_L01_Introduction_to_Remote_Sensing.pdf  
[4] https://coast.noaa.gov/data/digitalcoast/pdf/lidar-101.pdf  
[5] https://mdpi.com/2072-4292/14/17/4298  
[6] https://ioccg.org/wp-content/uploads/2024/12/1.5-lecture2_rf.pdf  
[7] https://geo-informatie.nl/courses/grs10306/Clevers/RS%20CH4%20Preprocessing/RS%20Ch%204%20RS%20Preprocessing.pdf  
[8] https://seadas.gsfc.nasa.gov/help-9.2.0/Sen2CorHelp.html  
[9] https://py6s.readthedocs.io/en/latest/params.html  
[10] https://step.esa.int/main/wp-content/help/versions/13.0.0/snap-toolboxes/eu.esa.opt.opttbx.sta.adapters.help/installation/MAJAInstallation.html  
[11] https://forum.step.esa.int/t/radiometric-geometric-correction-workflow/2540  
[12] https://dges.carleton.ca/courses/IntroSAR/Winter2019/SECTION%207A%20-%20Carleton%20SAR%20Training%20-%20SAR_processing_Sentinal1%20-%20FINAL.pdf  
[13] https://jees.kr/journal/view.php?doi=10.26866%2Fjees.2024.2.r.220  
[14] https://egusphere.copernicus.org/preprints/2025/egusphere-2025-3726/egusphere-2025-3726.pdf  
[15] https://lizardtech.com/post/step-by-step-pdal-python-workflow-for-lidar-data-processing  
[16] https://mdpi.com/2072-4292/15/16/4112  
[17] https://mdpi.com/2072-4292/16/15/2695  
[18] https://developmentseed.org/scaling_science/docs/Open_formats_tools.html  
[19] https://developers.google.com/earth-engine/tutorials/community/sentinel-2-s2cloudless  
[20] https://forums.fast.ai/t/split-large-satellite-image-into-tiles-patches/32039  
[21] https://medium.com/@tahjudil.witra/from-satellite-data-to-machine-learning-simplifying-data-pipelines-with-a-cloud-based-approach-2c810cf30b2c  
[22] https://earthdata.nasa.gov/about/esdis/esco/standards-practices/cloud-optimized-geotiff  
[23] https://github.com/sentinel-hub/eo-learn-examples  
[24] https://theoj.org/joss-papers/joss.03337/10.21105.joss.03337.pdf  
[25] https://pytorch.org/blog/geospatial-deep-learning-with-torchgeo  
[26] https://nature.com/articles/s41598-024-65954-w  
[27] https://ris.utwente.nl/ws/portalfiles/portal/488727903/2312.05327v1.pdf  
[28] https://github.com/radiantearth/stac-spec/blob/master/commons/common-metadata.md  
[29] https://sciencedirect.com/science/article/pii/S2352938523000034
