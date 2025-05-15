# 🌱 Drone-Based Soil Analysis Using Multispectral, Thermal, and Terrain Data 
### *Precision Agriculture with UAV(RGB, NIR, Thermal, and Topographic Data)*

---

## 📌 Overview

This project demonstrates how to analyze **bare-soil fields** using **high-resolution UAV imagery** (RGB, NIR, Thermal) and **terrain context** (DTM-derived slope, watershed, and flow direction). The goal is to produce actionable insights for **precision agriculture**, particularly for fields without standing crops.

<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/front-title.jpg?raw=true">

---

## 🎯 Project Objective

- Assess **soil health and moisture** in a bare agricultural field.
- Combine **spectral indices** (SAVI, BI, RI, LST) with **terrain analysis**.
- Identify and map optimal soil zones using unsupervised classification.
- Support **data-driven decision-making** for water, fertilizer, and planting strategies.

---

## Study Area & Data Sources

### **Metadata:**

| **Field**                | **Details**                                                                                                                                         |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| **Title**                | Low-altitude multispectral and thermal-infrared imagery from agricultural fields, Black Creek basin, Allen County, IN, USA - spring 2017                 |
| **Authors**              | Tanja N. Williamson, Shawn M. Meyer                                                                                                                 |
| **Published Date**       | 2018                                                                                                                                                |
| **Publisher**            | U.S. Geological Survey (Louisville, KY)                                                                                                             |
| **Related Work**         | “Delineation and inspection of tile-drain networks…” by Williamson, Dobrowolski, Meyer, Frey, and Allred (2018)                                     |
| **DOI Link**             | [https://doi.org/10.5066/F7Q81C9V](https://doi.org/10.5066/F7Q81C9V)                                                                                |
| **Flight Dates**         | April 18, 2017 and May 10, 2017                                                                                                                     |
| **Platform**             | 3DR Solo quadcopter UAV                                                                                                                             |
| **Cameras**              | Multispectral: MicaSense RedEdge (Blue, Green, Red, Red Edge, NIR) <br> Thermal: FLIR Vue Pro R 640, 13 mm lens, uncooled vanadium oxide microbolometer |
| **Coordinate System**    | NAD83 UTM Zone 16N                                                                                                                                  |
| **Bounding Box (WGS84)** | West: -84.909189, East: -84.897853, North: 41.215467, South: 41.204922                                                                             |
| **Study Area**           | Edge-of-field site near **Bull Rapids Road**, **Allen County, Indiana**, **USA**                                                                             |

---

### *Data used:*

| **Source**      | **Sensor**      | **Bands / Layers Used**                | **Ground Resolution** |
|-----------------|-----------------|----------------------------------------|----------------------|
| UAV Drone       | Multispectral   | RGB, Red Edge, NIR                     | ~6 cm                |
| UAV Drone       | Thermal (RAW)        | Longwave Infrared (LWIR)               | ~11 cm    (upsampled to 6 cm)           |
| Point cloud from stereo pairs    | Multispectral              | Elevation, Slope, Hillshade, Watershed | ~1 m   (downsampled)              |

---
## Steps

### Workflow Summary

1. **Data Acquisition**
  - UAV Imagery (RGB, NIR, Red Edge, Thermal)
  - Digital Terrain Model (DTM)

2. **Preprocessing**
  - Image transformation & Clipping

3. **Index Calculation**
  - SAVI (Soil-Adjusted Vegetation Index)
  - BI (Brightness Index)
  - RI (Redness Index)
  - TMP (Land Surface Temperature from Thermal)

4. **Terrain Analysis**
- Derive Slope, Flow Direction, Flow Accumulation
- Watershed Boundaries

5. **Data Normalization**
  - Normalize Index Layers \[0–1]
  - Ensure interpretability consistency across layers

6. **Unsupervised Classification (ISODATA)**
  - Classify stacked raster to segment soil zones

7. **Zonal Statistics**
  - Compute stats per zone (mean, std, range) for each index
  - Associate with field boundary vector zones  

8. **Interpretation & Mapping**
  - Interpret classified zones using index patterns
  - Overlay with terrain derivatives for validation
  - Generate styled final map

---

### *Step-1:*
- Collected high-resolution UAV imagery across multiple spectral bands: RGB, NIR, Red Edge, thermal, and point cloud from USGS (source mentioned above)

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/a.svg?raw=true">
</p>

### *Step-2:*
- Performed image transformations (raw thermal to temperature (celsius)) and clipping to align and prepare the datasets for analysis. 
- Prepared DTM from point cloud by classifying the ground points than rasterizing the result obtained
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/b1.svg?raw=true">
</p>

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/b2.svg?raw=true">
</p>

### *Step-3 & 5:*
- Computed various indices to assess soil and vegetation characteristics:
  - SAVI (Soil-Adjusted Vegetation Index) 
    - Higher SAVI: Slight organic residue / darker topsoil
    - Lower SAVI: Exposed, dry, sandy zones
  - BI (Brightness Index) to determine soil brightness levels.
    - High BI: Lighter soils (chalky, sandy)
    - Low BI: Darker soils (loamy, clay-rich)
  - RI (Redness Index) to identify iron oxide concentrations.
    - Higher RI indicates redder soils, often high in iron oxides.
  - TMP (Land Surface Temperature) derived from thermal imagery to assess surface heat.
    - Lower TMP: cooler zones = more moisture
    - Higher TMP: hotter = dry or compacted soil
- Normalized all raster layers to a [0–1] scale to ensure consistency and comparability across different indices and terrain attributes.

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/c_and_e.svg?raw=true">
</p>

### *Step-4:*
- Derived terrain attributes from the DTM:
  - Calculated slope, flow direction, and flow accumulation to understand water movement.
  - Generated watershed boundaries to delineate drainage areas.

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/d.svg?raw=true">
</p>

### *Step-6:*
- Applied the ISODATA algorithm to the normalized raster data to segment the area into distinct soil zones based on spectral features.
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/f.svg?raw=true">
</p>

### *Step-7:*
- Computed statistical measures associated with specific field boundary vector zones for detailed analysis.

*Zonal stats: Brightness Index*
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_BI.jpg?raw=true">
</p>

*Zonal stats: Redness Index*
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_RI.jpg?raw=true">
</p>

*Zonal stats: SAVI*
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_SAVI.jpg?raw=true">
</p>

*Zonal stats: TMP*
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_TMP.jpg?raw=true">
</p>

### Final map:
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/final_map.jpg?raw=true">
</p>



## 📊 Key Insights

| Class        | SAVI | BI   | RI   | TMP  | Interpretation                      |
|--------------|------|------|------|------|-------------------------------------|
| Best Soil    | High | Low  | Low  | Cool | Moist, rich, high fertility zones   |
| Moderate     | Mid  | Mid  | Mid  | Warm | Transitional, mixed-use suitability |
| Needs Care   | Low  | High | High | Hot  | Dry, oxidized, low retention zones  |

---

### Terrain Context: Watershed, Slope & Flow Direction

| Factor         | Best for Soil Health                | Not Ideal for Soil Health           |
|----------------|------------------------------------|-------------------------------------|
| Watershed      | Basin (collects water, retains moisture) | Outflow (loses water, prone to dryness) |
| Slope          | Gentle (prevents runoff, retains nutrients) | Steep (increases erosion, poor retention) |
| Flow Direction | Inward/Convergent (supports accumulation) | Outward/Divergent (promotes drainage)    |

---

## 🌾 Applications in Precision Agriculture
Fertilization Planning: Helps identify zones rich and poor in organics

Irrigation Efficiency: To devise optimal watering plan

Crop Selection: Adapt species to transitional vs. highly fertile zones

Runoff Mitigation: Avoid seeding near erosion-prone edges

Field Zoning: Align natural watersheds with agricultural management units for enhanced productivity

---


