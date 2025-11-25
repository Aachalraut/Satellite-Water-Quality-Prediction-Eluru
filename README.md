## This repository contains the geospatial and machine learning pipeline developed for pond water quality prediction in Eluru, India. It integrates Sentinel-2 satellite imagery with ML models by extracting multispectral features using Google Earth Engine.

## 🌍 Integration of Satellite Remote Sensing and Machine Learning for Pond Water Quality Prediction in Eluru

This project focuses on integrating Satellite-based Remote Sensing (Sentinel-2) with Machine Learning to automate the extraction of pond-level spectral indicators and generate datasets for pond water quality prediction in the Eluru region (India).
The workflow uses Google Earth Engine (GEE) for geospatial automation and prepares structured band-level data suitable for downstream ML models such as Random Forest, Gradient Boosting, XGBoost, SVM, or Deep Learning models.

## 📌 Project Objective

To build an automated pipeline that:
Reads pond information (Latitude, Longitude, and Sample Collection Dates) from a CSV file.
Extracts Sentinel-2 imagery for each pond location on (or close to) the survey date.
Computes spectral band statistics + vegetation and water indices (e.g., NDVI).

## Exports:
GeoTIFF satellite images for each pond
CSV files containing band values for ML model training
Prepares a ready-to-use dataset for water quality parameter prediction (pH, DO, Turbidity, etc.).

## 🛰️ Why Sentinel-2?
Sentinel-2 imagery is ideal for water quality studies because it provides:
10–20 m spatial resolution
High revisit time (5 days)
Rich spectral bands (Visible, NIR, SWIR)
Compatibility with computing indices like NDVI, NDWI, MNDWI

## 🚀 Key Features of This Project
✔ Automated for Large Datasets
Processes pond locations in batches of 10 rows, suitable for thousands of ponds.
✔ Handles Non-standard Dates
Accepts date formats like:
October 31, 2024
31-10-2024
2024/10/31
GEE auto-interpretable dates
✔ Retrieves True-Color Satellite Images
Each pond exports a 200m buffered GeoTIFF.
✔ Extracts Band-Level Information

For each pond:
B2 (Blue)
B3 (Green)
B4 (Red)
B8 (NIR)
B11, B12 (SWIR)
NDVI

✔ Saves Band Values to CSV
A structured dataset is automatically generated for ML training.
✔ Error Handling Built-in

## Handles:
Missing coordinates
Invalid dates
Ponds with no available imagery
Null band values

## 🛰️ Use of Google Earth Engine (GEE)

Google Earth Engine (GEE) serves as the core geospatial processing platform for this project. It is used for automating satellite image retrieval, filtering, preprocessing, visualization, and extraction of spectral features corresponding to each pond location in Eluru.

The following steps summarize how GEE is utilized in the system:

⚙️ 1. Importing Pond Coordinates as an Asset

Pond locations stored in a CSV file (Latitude, Longitude, and Date of Data Collection) were uploaded to GEE as a Table Asset.
This allowed GEE to represent each pond as a point Feature inside a FeatureCollection.

Workflow:

Assets → NEW → Table Upload

Upload ponds.csv

Convert it into a FeatureCollection

Register asset path
example:
projects/<project-id>/assets/gedata

This enabled GEE to loop through each pond and process them individually.

⚙️ 2. Filtering Sentinel-2 Satellite Imagery

For each pond:
The script reads its latitude, longitude, and sampling date.
Sentinel-2 Surface Reflectance (COPERNICUS/S2_SR_HARMONIZED) images are filtered by:
Location (filterBounds)
Date range (sampling date ± 1 day)
Cloudiness (sorted by CLOUDY_PIXEL_PERCENTAGE)
This ensures that the least-cloudy, closest-possible satellite image for each pond is selected.

⚙️ 3. Extracting Spectral Bands & Indices

Once the most suitable image is identified, the script extracts:

Sentinel-2 Bands
Band	Meaning
B2	Blue
B3	Green
B4	Red
B8	Near Infrared (NIR)
B11	SWIR-1
B12	SWIR-2

⚙️ 4. Exporting Spectral Data as CSV
All extracted band values, NDVI, pond ID, date, and status flags are stored in a FeatureCollection and exported as a CSV using:
Export.table.toDrive()
CSV Structure:
Pond_ID	Date	B2	B3	B4	B8	B11	B12	NDVI	Status
This becomes the dataset used for Machine Learning.

⚙️ 5. Batch Processing (10 Rows at a Time)
To avoid GEE memory limits:
A batching mechanism was implemented
Processes 10 ponds per execution

⚙️ 6. Automated Logging & Error Tracking
GEE handles:
Missing dates
Invalid latitude/longitude
Dates with no available images
Statistics extraction failures
Each row is tagged with a Status:
image_exported
no_image
missing_input
This ensures data consistency for ML training.

## ▶️ Usage

1. Upload your pond coordinates CSV file containing:
   - Latitude
   - Longitude
   - Date of Data Collection

2. Run the Google Earth Engine (GEE) script to:
   - Filter Sentinel-2 images based on date and location
   - Extract spectral band values (B2, B3, B4, B8, B11, B12)
   - Compute NDVI and other indices
   - Export GeoTIFF images and CSV feature tables

3. Use the exported CSV dataset in the ML notebook to train:
   - Random Forest
   - Gradient Boosting
   - XGBoost
   - SVM
   - Deep Learning models

4. Generate predictions for water quality parameters such as:
   - Dissolved Oxygen (DO)
   - Ammonia
   - pH
   - Turbidity
   - Temperature

## 📜 License
This project is released under the **MIT License**.  
Please refer to the LICENSE file for full terms.

## 👥 Authors / Contributors
- Aachal Raut — AI Engineering Student
- Aayushi Joshi — AI Eb=ngineering Student
