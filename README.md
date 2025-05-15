# 🌱 Drone-Based Soil Analysis Using Multispectral, Thermal, and Terrain Data 
### *Precision Agriculture with UAV, RGB, NIR, Thermal, and Topographic Data*

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

graph TD
    A[Data Acquisition (UAV Imagery & DTM)] --> B[Preprocessing]
    B --> C[Compute Indices: SAVI, BI, RI, LST]
    B --> D[DTM Derivatives: Slope, Flow, Hillshade, Watershed]
    C --> E[Normalize Raster Layers]
    D --> E
    E --> F[Multiband Raster Stack]
    F --> G[Unsupervised Classification (ISODATA)]
    G --> H[Zonal Statistics]
    H --> I[Contextual Interpretation with Terrain Data]
    I --> J[Map Styling + Report Generation]
    
### *Step-A:*

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/a.jpg?raw=true">
</p>

### *Step-B:*

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/b1.jpg?raw=true">
</p>

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/b2.jpg?raw=true">
</p>

### *Step-C & E:*

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/c_and_e.jpg?raw=true">
</p>

### *Step-D:*

<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/d.jpg?raw=true">
</p>

### *Step-F:*
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/f.jpg?raw=true">
</p>

### *Step-G:*
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_BI.jpg?raw=true">
</p>
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_RI.jpg?raw=true">
</p>
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_SAVI.jpg?raw=true">
</p>
<p align="center">
<img src="https://github.com/warhazzard/Project-Soil-Analysis/blob/main/Output/zonalst_TMP.jpg?raw=true">
</p>

#### Final map:
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
Fertilization Planning: Target zones rich in organics (high SAVI)

Irrigation Efficiency: Match cool zones to optimal watering areas

Crop Selection: Adapt species to transitional vs. highly fertile zones

Runoff Mitigation: Avoid seeding near erosion-prone edges

Field Zoning: Align natural watersheds with agricultural management units

---


