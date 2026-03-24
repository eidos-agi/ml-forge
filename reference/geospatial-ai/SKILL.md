---
name: geospatial-ai
description: Build satellite imagery and geospatial AI pipelines using NDVI, NBR indices, deep learning segmentation (UNet, SegNet), and Google Earth Engine. Apply to real estate valuation, agricultural monitoring, infrastructure assessment, insurance risk, and environmental conservation.
---
# Satellite & Geospatial AI
## When to Use This Skill
Use when working with location-based data that benefits from satellite or aerial imagery: real estate valuation, agricultural health, wildfire damage, infrastructure monitoring, insurance risk assessment, environmental compliance.

## Instructions
1. **Data Acquisition**
   - Google Earth Engine: `ee.ImageCollection('COPERNICUS/S2')` for Sentinel-2
   - INPE: Brazilian space agency for South American coverage
   - Planet Labs: high-resolution commercial imagery
   - Drone: RGB for visual, multispectral for vegetation health, thermal for heat mapping
2. **Spectral Indices**
   - NDVI = (NIR - Red) / (NIR + Red) — vegetation health (-1 to +1, >0.3 healthy)
   - NBR = (NIR - SWIR) / (NIR + SWIR) — burn severity
   - dNBR = pre-fire NBR - post-fire NBR — change detection
   - NDWI = (Green - NIR) / (Green + NIR) — water presence
3. **Deep Learning Segmentation**
   - UNet: encoder-decoder with skip connections, best for precise boundary delineation
   - SegNet: encoder-decoder using max-pooling indices, more memory efficient
   - Input: multi-band satellite patches (e.g., 256x256 pixels, 4-12 bands)
   - Output: pixel-level classification mask (burned/unburned, crop/no-crop, etc.)
4. **Change Detection Pipeline**
   - Acquire pre-event and post-event imagery
   - Compute spectral indices for both
   - Calculate difference (dNBR, dNDVI)
   - Classify change severity: Mild (<0.27), Moderate (0.27-0.66), Severe (>0.66)
5. **Infrastructure** — AWS S3 for image storage, SageMaker for training, Batch for overnight inference, PostGIS for spatial queries
6. **Automated Monthly Monitoring** — Cron job acquires latest imagery, computes indices, flags changes above threshold, generates report with before/after comparison maps
