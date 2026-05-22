---
layout: post
title:  "How to Plot Spatial data in HA using Python"
date:   2026-05-21 16:41:33 +0100
categories: ha python geospatial air quality 
---
# Home Assistant: London Air Quality Geospatial Heatmap Guide

This guide explains how to extract embedded geographic data from the King's College **London Air Quality** integration, process it with Python using **AppDaemon**, and display a dynamic PM2.5/PM10 heatmap overlay on your dashboard.

---

## 🛠️ Step 1: Install & Configure AppDaemon

Because Home Assistant OS (HAOS) runs inside secure containers, you need AppDaemon to manage the external Python libraries required to generate interactive map files.

1. Navigate to **Settings > Add-ons > Add-on Store** in your Home Assistant UI.
2. Search for **AppDaemon**, click **Install**, and toggle **Show in sidebar** on.
3. Select the **Configuration** tab at the top of the AppDaemon add-on page.
4. Locate the `system_packages` and `python_packages` blocks, then add `folium` and `requests` exactly as shown below:
   ```yaml
   system_packages: []
   python_packages:
     - folium
     - requests
   ```
5. Click **Save**, return to the **Info** tab, and click **Start**.

---

## 🐍 Step 2: Create the Python Script

AppDaemon communicates natively with your Home Assistant database, so you do not need to manage or expose secure API tokens.

1. Use an add-on like **File Editor** or **Advanced SSH & Web Terminal** to open your Home Assistant storage directory.
2. Navigate to the `/config/appdaemon/apps/` folder path.
3. Create a new file named `london_aqi_map.py` and paste the following Python code inside it:

```python
import hassapi as hass
import folium
from folium.plugins import HeatMap

class LondonAQIMap(hass.Hass):

    def initialize(self):
        # Build the map file immediately on system initialization
        self.generate_map()
        # Automatically update the map layer every 30 minutes
        self.run_every(self.generate_map, "now", 30 * 60)

    def generate_map(self, kwargs=None):
        self.log("Starting London Air Quality heatmap generation...")
        
        # Populate this list with your active borough sensor entity IDs
        borough_sensors = [
            "sensor.merton",
            "sensor.islington",
            "sensor.westminster",
            "sensor.lambeth",
            "sensor.southwark"
        ]
        
        # Define target metric: 'PM2.5' or 'PM10'
        target_pollutant = "PM2.5"
        heatmap_points = []

        for entity in borough_sensors:
            try:
                # Native AppDaemon API call to extract the entire state state array
                state_data = self.get_state(entity, attribute="all")
                if not state_data:
                    continue
                
                attributes = state_data.get("attributes", {})
                sites = attributes.get("sites", [])
                
                if not isinstance(sites, list):
                    continue
                    
                # Iterate through the embedded locations provided by King's College
                for site in sites:
                    lat = site.get("latitude")
                    lon = site.get("longitude")
                    pollutants = site.get("pollutants", {})
                    target_data = pollutants.get(target_pollutant, {})
                    value = target_data.get("value")
                    
                    if lat and lon and value is not None:
                        try:
                            # Clean and structure points: [Latitude, Longitude, Intensity]
                            heatmap_points.append([float(lat), float(lon), float(value)])
                        except ValueError:
                            continue
                            
            except Exception as e:
                self.log(f"Error parsing entity {entity}: {e}", level="WARNING")

        if not heatmap_points:
            self.log("No valid coordinates or pollution metrics found. Skipping render.", level="WARNING")
            return

        # Initialize base map layer centered on Greater London
        london_map = folium.Map(
            location=[51.5074, -0.1278], 
            zoom_start=11, 
            tiles="CartoDB positron" # Clean minimalist backdrop for high-contrast overlays
        )
        
        # Build and configure heatmap plume sizes
        HeatMap(
            data=heatmap_points, 
            radius=35,  # Radius of each individual pollution point
            blur=20,    # Blur amount to merge neighboring plumes smoothly
            max_zoom=12
        ).add_to(london_map)
        
        # Save output straight to HAOS public web directory path
        try:
            london_map.save("/config/www/london_aqi_heatmap.html")
            self.log("Heatmap rendered successfully.")
        except Exception as e:
            self.log(f"Failed to write map output file: {e}", level="ERROR")
```

---

## ⚙️ Step 3: Register the App inside AppDaemon

AppDaemon tracks active execution routines via a configuration manifest file.

1. Inside the same `/config/appdaemon/apps/` folder, locate and open the file named `apps.yaml`.
2. Append the following structural entry directly to the bottom of the file:
   ```yaml
   london_aqi_heatmap_app:
     module: london_aqi_map
     class: LondonAQIMap
   ```
3. Save changes. AppDaemon automatically picks up changes to this file, processes dependencies, and executes the runtime environment immediately. Check the AppDaemon **Log** tab to ensure it initializes with no syntax warnings.

---

## 📊 Step 4: Add the Card Layer to your Dashboard

Home Assistant exposes everything inside your local `/config/www/` directory to its built-in internal web server path `/local/`.

1. Open your Home Assistant frontend dashboard.
2. Click the three dots menu button in the top-right corner and choose **Edit Dashboard**.
3. Click **Add Card** at the bottom, select **Webpage** (or use the raw code editor), and input these values:
   ```yaml
   type: iframe
   url: /local/london_aqi_heatmap.html
   aspect_ratio: 16:9
   ```
4. Click **Save** and exit dashboard editing. Your interactive, zoomable pollution map is now active.


