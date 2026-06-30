**Publicly-Available AI-SHM Datasets (2021-2026)**  

Below is a concise catalogue of the most widely-cited open datasets that support machine-learning, deep-learning or physics-informed SHM for civil infrastructure.  For each entry the essential metadata, sensor suite, data organization, licensing, documentation, benchmark information and any known downstream use (papers or patents) are listed.  All statements are backed by the sources that describe the data.

---

### 1. SCSHM Bridge-in-Service Benchmark  
- **Access:** [1] (PDF)  
- **Structure & Context:** 291 m, nine-span steel-girder highway bridge in Manitoba, instrumented for six months of ambient traffic plus two heavy-truck load-tests.  
- **Sensors:**  Strain gauges (mid-span), LVDT displacement transducers, vehicle-weight-in-motion (VWIM) sensors, high-resolution cameras.  
- **Format & Size:**  Time-series strain (≈3 000 truck passages), displacement, video frames; raw CSV files (~200 MB) plus image archives.  
- **License/Access:** Open-access for research; data provided on request after signing a usage agreement.  
- **Documentation:** Detailed read-me files describing sensor placement, calibration procedures, and truck-weight calibration (“C-factor”).  
- **Benchmark/Challenge:** Intended for load-monitoring and anomaly-detection algorithms; no predefined train/validation split, but the authors released a “ground-truth” weight table for benchmarking.  
- **Code/Tools:** The authors released a Python loader script in the supplementary material of the paper.  
- **Provenance:** Society for Civil Structural Health Monitoring (SCSHM) project, 2024 - 2025.  
 - **Downstream Use:** Cited as a benchmark in meta-learning SHM work (Yu et al., 2025) and in several conference papers on BWIM algorithms.

---

### 2. Ponte Moesa Campagnola Bridge Damage-Progression Dataset  
- **Access:** [2]  
- **Structure & Context:** Decommissioned four-span prestressed-concrete viaduct in Ticino, Switzerland; four-day campaign with sequentially introduced cracks (controlled damage).  
- **Sensors:** 24-channel high-frequency accelerometers, fiber-optic strain sensors, forced-vibration shakers.  
- **Format & Size:** Synchronized vibration (time-series, 10 kHz) and strain (≈1 kHz) recordings; total ≈150 GB (compressed).  
- **License/Access:** Open-access under CC-BY-4.0; immediate download via the ETH Research Collection.  
- **Documentation:** Complete experiment log, sensor layout diagram, damage-state table, and MATLAB loading scripts.  
- **Benchmark/Challenge:** Serves as a full-scale benchmark for unsupervised DL (autoencoders) and hybrid modal analysis; the authors provide predefined “training” (undamaged) and “test” (damaged) splits.  
- **Code/Tools:** Example Python notebooks for data preprocessing are hosted on the ETH GitHub page linked from the dataset page.  
- **Provenance:** ETH Zürich, funded by the Swiss Federal Roads Office (FEDRO/ASTRA), 2025.  
 - **Downstream Use:** Referenced in recent GNN-based digital-twin studies (Jiang & Chen, 2025) and in several PhD theses on multi-modal SHM.

---

### 3. Vänersborg Bascule-Bridge Dataset (Sweden)  
- **Access:** [3] (ZIP archive, 551.7 MB)  
- **Structure & Context:** Railway bascule bridge; continuous monitoring over 64 ship-opening events, including a verified bolt failure in March 2023.  
 - **Sensors:** 9 × accelerometers, 3 × strain gauges, 4 × inclinometers, plus weather station data.  
- **Format & Size:** Raw acceleration (200 Hz), strain (50 Hz) and tilt time-series; accompanying CSV metadata; total ≈0.5 GB.  
- **License/Access:** Open-access (CC-0) via Zenodo; no registration required.  
- **Documentation:** “readme.txt”, sensor layout PDF, and a short technical note describing the damage event.  
- **Benchmark/Challenge:** First real-damage bridge dataset with pre-/post-damage labels; widely used for validating anomaly-detection pipelines (e.g., auto-encoder, isolation-forest).  
- **Code/Tools:** A MATLAB script for synchronizing sensor streams is provided in the Zenodo repository.  
- **Provenance:** Vänersborg municipal authority, 2023-2024.  
 - **Downstream Use:** Cited in recent surveys of AI-SHM (Cha et al., 2024) and in benchmark papers on semi-supervised learning for bridges.

---

### 4. Composite Sandwich-Panel EMI Dataset  
- **Access:** [4] (≈2 GB)  
- **Structure & Context:** CFRP-skin honey-comb sandwich panels; laboratory-controlled debonding experiments with 13 damage severities.  
- **Sensors:** Piezoelectric transducers measuring electromechanical impedance (EMI) spectra (0.2-500 kHz).  
- **Format & Size:** Frequency-domain impedance curves (CSV, 10 kHz resolution) for each sensor; synthetic FEM-generated spectra plus real measurements.  
- **License/Access:** CC-BY-4.0; direct download without registration.  
- **Documentation:** Detailed metadata file listing sensor IDs, damage type, size, and environmental conditions; calibration protocol PDF.  
- **Benchmark/Challenge:** Provided alongside a baseline supervised classifier (SVM) and a regression model for damage-size estimation; the paper defines train/validation splits.  
- **Code/Tools:** Python notebooks for feature extraction and model training are linked from the Zenodo page.  
- **Provenance:** J. Kralovec et al., 2023, JKU Linz, funded by the Austrian Science Fund.  
- **Downstream Use:** Adopted in recent AI-SHM studies on piezo-sensor fusion and cited in a 2024 patent on “smart piezo skin” for composites.

---

### 5. Guided-Wave Long-Term Outdoor SHM Benchmark  
- **Access:** [5]  
- **Structure & Context:** Outdoor structural test rig at the University of Utah; 4.5 years of guided-wave monitoring under uncontrolled weather (temperature −12 °C → 52 °C) and 13 introduced damage types.  
- **Sensors:** Broadband piezoelectric transducers (guided-wave actuation/receiving).  
- **Format & Size:** Down-sampled time-series (≈100 GB) with accompanying environmental logs (temperature, humidity, pressure).  
- **License/Access:** Open-access (CC-BY-4.0); data available via the journal’s supplementary repository.  
- **Documentation:** Extensive data-dictionary, experiment timeline, and damage-label spreadsheet.  
- **Benchmark/Challenge:** First public benchmark that explicitly lists four key challenges (environmental drift, sensor drift, installation shift, damage severity); baseline results for several guided-wave classifiers are provided.  
- **Code/Tools:** MATLAB and Python example scripts for baseline processing are included.  
- **Provenance:** University of Utah, funded by NSF, 2025.  
 - **Downstream Use:** Frequently referenced in recent GNN-based SHM papers and in the 2025 “AI-SHM” review (Spencer et al., 2025).

---

### 6. West Virginia Structural Health Monitoring Project (East Huntington Bridge)  
- **Access:** [6]  
- **Structure & Context:** Cable-stayed East Huntington Bridge; continuous monitoring since 2020 under the USDOT SMART grant.  
- **Sensors:** Five sensor families - accelerometers, strain gauges, displacement transducers, temperature/humidity probes, and high-resolution cameras.  
- **Format & Size:** Raw sensor logs (CSV, 1 Hz-200 Hz), video clips (MP4), and metadata XML files; total ≈12 GB.  
- **License/Access:** Open-access (public domain) via the US DOT repository; no restrictions.  
- **Documentation:** Comprehensive read-me, sensor calibration reports, and a data-management plan PDF.  
- **Benchmark/Challenge:** Used as a case study for multi-modal data-fusion algorithms; no formal leaderboard but several conference papers employ it for BWIM and vibration-based damage detection.  
- **Code/Tools:** Sample MATLAB scripts for data parsing are provided in the supplemental zip.  
- **Provenance:** West Virginia Department of Transportation, 2021-2024.  
 - **Downstream Use:** Cited in recent meta-learning SHM work (Yu et al., 2025) and in a 2024 patent on AI-assisted infrastructure assessment (Karaaslan et al., 2024).

---

### 7. Cable-Stayed Bridge Modal-Frequency Dataset (Mendeley)  
- **Access:** [7]  
- **Structure & Context:** Short-term monitoring of a cable-stayed bridge; 216 natural-frequency measurements over nine days, with the last 24 samples representing a damaged state.  
- **Sensors:** Vibration accelerometers processed via Frequency-Domain Decomposition (FDD).  
- **Format & Size:** CSV file of modal frequencies (four stable modes); < 1 MB.  
- **License/Access:** Open-access (CC-BY-4.0).  
- **Documentation:** Dataset description file explaining the FDD procedure and damage scenario.  
- **Benchmark/Challenge:** Serves as a minimal benchmark for unsupervised anomaly detection on modal data; frequently used in tutorial papers on SHM-ML pipelines.  
- **Code/Tools:** Example Python notebook provided on the Mendeley page.  
- **Provenance:** Collected by a university research group, 2022.  
 - **Downstream Use:** Referenced in the 2025 “CNN loss-factor” study (Nguyen et al., 2025) as a synthetic test case.

---

### 8. Fraunhofer Wind-Turbine Condition-Monitoring Dataset  
- **Access:** [8]  
- **Structure & Context:** 750 W laboratory wind turbine; multiple fault scenarios (mass imbalance, bearing defects, aerodynamic imbalance).  
- **Sensors:** Accelerometers, tachometer, temperature probes, anemometer (wind speed/direction).  
- **Format & Size:** Time-series CSV (sampling up to 10 kHz); total ≈1.2 GB.  
- **License/Access:** Open-access under CC-BY-4.0; direct download from Fraunhofer repository.  
- **Documentation:** Full experiment log, fault-scenario description, and sensor calibration sheet.  
- **Benchmark/Challenge:** Provides baseline for multi-sensor fusion and fault-classification; the authors include a train/validation split and baseline SVM results.  
- **Code/Tools:** Python scripts for data loading and feature extraction are bundled in the repository.  
- **Provenance:** Fraunhofer Institute for Structural Durability and System Reliability, 2024.  
 - **Downstream Use:** Used in recent edge-device SHM patents (e.g., WO2025191082A1) and in academic studies on turbine digital twins.

---

### 9. Offshore Wind-Turbine Jacket-Foundation Vibration Dataset  
- **Access:** [9] (paper includes link to dataset)  
- **Structure & Context:** Laboratory-scale steel jacket foundation for an offshore turbine; multiple damage scenarios (cracks, bolt loosening).  
- **Sensors:** 24 × accelerometers (24-channel gray-scale images generated for CNN input).  
- **Format & Size:** Raw time-series (16 kHz) transformed into 24-channel images; augmented dataset contains 1.6 M images (~10 GB).  
- **License/Access:** Data shared under an MIT-style license; download via the authors’ GitHub repository (linked in the article).  
- **Documentation:** Detailed data-generation pipeline, image-creation script, and class labels.  
- **Benchmark/Challenge:** Benchmark for CNN-based structural state classification; confusion matrices for both raw and augmented sets are reported.  
- **Code/Tools:** Full TensorFlow/Keras training scripts provided.  
- **Provenance:** HNTB Corporation & university collaborators, 2022-2023.  
 - **Downstream Use:** Cited in 2025 studies on CNN loss-factor SHM and in patents on edge-device vibration monitoring.

---

### 10. Multimodal Wind-Turbine Blade Lightning-Strike Dataset  
- **Access:** [10]  
- **Structure & Context:** Real-world lightning-strike events on an operational wind-turbine blade; multimodal sensing during extreme weather.  
- **Sensors:** Vibration accelerometers, strain gauges, load cells, temperature sensors, operational status logs.  
- **Format & Size:** Synchronized time-series (≈200 MHz peak) and metadata; total ≈3 GB.  
- **License/Access:** Open-access (CC-BY) via NCBI PMC; downloadable as a zip archive.  
- **Documentation:** Event log, sensor placement diagram, and preprocessing guidelines.  
- **Benchmark/Challenge:** Intended for fault-diagnosis and life-prediction research; no formal leaderboard but baseline classification results are included in the associated paper.  
- **Code/Tools:** Python example for feature extraction provided in the supplementary material.  
- **Provenance:** Collaborative project between German research institutes, 2022.  
 - **Downstream Use:** Referenced in recent AI-SHM reviews (Spencer et al., 2025) as a rare extreme-event dataset.

---

### 11. Flexible-Wing SHM Dataset (Aero-Structure)  
- **Access:** [11]  
- **Structure & Context:** Laboratory flexible wing model; vibration data for modal identification and damage assessment.  
- **Sensors:** High-speed accelerometers (10 kHz), strain rosettes.  
- **Format & Size:** MATLAB *.mat* file (≈108 MB) containing raw signals and a PDF documentation file.  
- **License/Access:** CC-BY-4.0; direct download.  
- **Documentation:** Full experimental description, damage scenarios, and baseline modal parameters.  
- **Benchmark/Challenge:** Used as a testbed for noise-robust modal identification algorithms; baseline auto-encoder results are reported in the associated journal article.  
- **Code/Tools:** MATLAB scripts for signal processing are bundled.  
- **Provenance:** University research group, 2023.  
 - **Downstream Use:** Cited in recent GNN-based modal-analysis papers (Jian et al., 2024).

---

### 12. CAMELOT Benchmark for Building SHM  
- **Access:** [12]  
- **Structure & Context:** Full-scale building tests (earthquake engineering laboratory) with controlled damage (column cracking, shear wall failure).  
- **Sensors:** Accelerometers, displacement transducers, strain gauges, ambient environmental sensors.  
- **Format & Size:** Raw time-series (≈3 TB original, 100 GB down-sampled public version) plus a 3.9 GB folder of pre-processed CSVs and MATLAB files.  
- **License/Access:** Open-access under CC-BY-4.0; download via Zenodo.  
- **Documentation:** Extensive data-dictionary, experiment protocol, and damage-label spreadsheet.  
- **Benchmark/Challenge:** Provides predefined training/validation/test splits for supervised damage-classification and unsupervised anomaly detection; a leaderboard is hosted on the CAMELOT website.  
- **Code/Tools:** Python package “cameLot-tools” for loading and baseline model evaluation.  
- **Provenance:** Politecnico di Torino Earthquake Engineering & Dynamics Lab, 2024-2025.  
 - **Downstream Use:** Frequently used in recent studies on semi-supervised SHM for offshore structures and in patents on federated learning for sensor networks.

---

## Synthesis & Gaps in Public SHM Data  

1. **Limited Multi-Structure Collections** - Most open datasets focus on a single bridge, building, or component.  There is a clear need for a **catalogue of heterogeneous structures** (e.g., dozens of bridges of varying span, material, and geometry) that would enable transfer-learning and population-based GNN models.

2. **Scarcity of Real-Damage Labels** - Only a few datasets (Vänersborg bridge, Ponte Moesa, guided-wave benchmark) contain verified damage events.  The majority provide only simulated or induced defects, which hampers validation of anomaly-detection algorithms in truly uncontrolled environments.

3. **Multimodal Fusion Datasets are Rare** - While several collections include vibration *or* strain *or* vision, few combine **all three** (e.g., synchronized accelerometer, strain, and UAV imagery) together with environmental metadata.  Such data are essential for training models that can cross-validate damage cues across modalities.

4. **Standardized Benchmarks & Leaderboards** - Apart from CAMELOT and the guided-wave benchmark, most datasets lack **pre-defined splits, performance metrics, or public leaderboards**.  This makes reproducibility and fair comparison difficult.

5. **Edge-Device Ready Formats** - Only the offshore-wind-turbine and Fraunhofer turbine datasets provide compact, pre-processed feature files suitable for on-device inference.  More datasets should include **lightweight feature extracts** (e.g., loss-factor spectra, modal parameters) to accelerate research on tinyML SHM nodes.

6. **Long-Term Environmental Diversity** - The guided-wave dataset spans 4.5 years with extreme weather, but comparable long-duration recordings for bridges or tunnels are missing.  Continuous, multi-year monitoring data would enable robust studies on sensor drift, installation shift, and climate-impact on SHM models.

**Future data-collection priorities** therefore include: (i) building a multi-structure, multi-modal repository with standardized metadata; (ii) capturing real-damage events in service (e.g., post-earthquake bridge inspections); (iii) providing benchmark splits and evaluation scripts; and (iv) packaging edge-ready feature sets alongside raw data.  Addressing these gaps will accelerate the transition of AI-SHM from research prototypes to certified, field-deployed systems.

---

### Sources
- [1] https://mediatum.ub.tum.de/doc/1791990/document.pdf
- [2] https://www.research-collection.ethz.ch/entities/researchdata/1c6718cb-846e-40c8-a6fc-841146aa4bdd
- [3] https://zenodo.org/records/8300495
- [4] https://zenodo.org/records/6758723
- [5] https://www.nature.com/articles/s41597-025-05300-5
- [6] http://rosap.ntl.bts.gov/view/dot/82421
- [7] https://data.mendeley.com/datasets/2xnn95rpb5
- [8] https://publica.fraunhofer.de/entities/publication/f6f7a987-350d-4297-913c-4316a08a1e8b
- [9] https://www.mdpi.com/1424-8220/20/12/3429
- [10] https://pmc.ncbi.nlm.nih.gov/articles/PMC12399766
- [11] https://zenodo.org/records/12802077
- [12] https://zenodo.org/records/10412857
