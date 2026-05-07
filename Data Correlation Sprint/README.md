# NIRA-SNIC Matching and Correlation Analysis

## Overview

This task asks the student to study how the NIRA and SNIC datasets relate to each other across both time and space. The goal is to build a high-level understanding of how road friction, grip conditions, weather signals, image-based observations, and geographic position may align, differ, or reinforce one another.

The work should focus on matching the two data sources, preserving their original GPS information, and producing a correlation analysis that considers temporal alignment and spatial proximity. The final outcome should also include a simple web platform for exploring the matched and unmatched data visually.

## Available Data

### NIRA Data

1. Weather data: `vehicle_weather_parquet_files/`
   - Daily parquet files containing weather-related observations tied to road geometry.
   - Main variables include:
     - `timestamp`
     - `segmentid`
     - `geometryversion`
     - `wiperspeedaverage`
     - `temperatureaverage`
     - `geometrywkt`

2. Friction data: `friction_combined.csv`
   - Road friction observations with time and geometry references.
   - Main variables include:
     - `timestamp`
     - `avgmidpointfriction`
     - `frictionlossindicator`
     - `geometrywkt`
     - `highestmidpoint`
     - `lowestmidpoint`
     - `lengthofsegment`
     - `tile_id`

### SNIC Data

1. Image links and image metadata: `SNIC Grip Data/coe-snic-default-rtdb-export.json`
   - Contains image-related records and supporting metadata.
   - Main variables may include:
     - `captureId`
     - `downloadUrl`
     - `image`
     - `timestamp`
     - `gps.latitude`
     - `gps.longitude`
     - `gps.accuracy`
     - `storagePath`
     - `publishId`
     - `sourceLogId`

2. Grip features: `SNIC Grip Data/drives/`
   - Drive-level GeoJSON-style files containing grip-related road features and embedded point observations.
   - Main variables may include:
     - `geometry`
     - `friction`
     - `state`
     - `tdewdiff`
     - `tsurf`
     - `water`
     - `dataPoints[].time`
     - `dataPoints[].coordinates`
     - `dataPoints[].friction`
     - `dataPoints[].state`
     - `dataPoints[].ta`
     - `dataPoints[].tdew`
     - `dataPoints[].rh`
     - `dataPoints[].ice`
     - `dataPoints[].water`
     - `dataPoints[].accuracy`
     - `dataPoints[].weight`

## Task Statement

The project should include creating a high-level workflow that matches NIRA and SNIC data while respecting both temporal and spatial context. The matching process should preserve the GPS characteristics of each source rather than replacing one geometry with the other. A major part of the task is to standardize the NIRA friction measurements and SNIC grip measurements as much as reasonably possible so they can be compared and analyzed within a common framework.


After the matching stage, the project should include performing a correlation analysis that examines relationships across time and location. This analysis should highlight where the two sources agree, where they diverge, and whether certain weather or road-state conditions are associated with stronger or weaker alignment.


## Expected Output

The final result should include:

1. A matched NIRA-SNIC analytical dataset that keeps the original spatial references from both data sources.
2. A temporal and spatial correlation analysis between NIRA friction/weather information and SNIC grip/image-derived information.
3. A brief explanation of how grip and friction measurements were standardized for comparison.
4. A web platform that visualizes the data as map layers and supports features such as:
   - filtering by time, location, or condition
   - hotspot identification
   - inspection of matched vs. unmatched observations
   - ``display of image-linked records``
   - interactive exploration of spatial and temporal patterns

