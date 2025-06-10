# Interactive Access and Visualization of Geospatial Data from the AWS Open Data Program

## Introduction

Open data is reshaping how we understand and respond to global challenges. From climate change to disaster recovery, the ability to access and analyze large-scale geospatial datasets is critical for scientific research, policy-making, and real-world applications. Leading the charge are several open data initiatives designed to lower access barriers and accelerate innovation: the [AWS Open Data Program](https://aws.amazon.com/opendata), the [Amazon Sustainability Data Initiative (ASDI)](https://exchange.aboutamazon.com/data-initiative), and the [Maxar Open Data Program](https://registry.opendata.aws/maxar-open-data).

The **AWS Open Data Program** hosts a diverse range of high-value datasets—from satellite imagery and climate models to genomics and machine learning benchmarks—on Amazon Web Services. These datasets are freely accessible and stored in Amazon S3, enabling cloud-native analysis without requiring massive local downloads. Researchers and developers can process data directly in the cloud using scalable tools like Amazon Athena, SageMaker, and open-source Python libraries. The registry of AWS Open Data is available at [https://registry.opendata.aws](https://registry.opendata.aws).

Building on this foundation, the **Amazon Sustainability Data Initiative (ASDI)** focuses specifically on environmental and sustainability-related datasets. ASDI supports global research efforts by providing open access to critical data such as weather forecasts, satellite observations, air quality indices, and hydrological models. These resources help drive solutions in areas such as climate resilience, renewable energy, conservation, and disaster risk management. ASDI also collaborates with organizations like NASA, NOAA, and the UN to make authoritative datasets readily available through AWS. The registry of ASDI is available at [https://registry.opendata.aws/collab/asdi](https://registry.opendata.aws/collab/asdi).

The **Maxar Open Data Program**, meanwhile, provides high-resolution satellite imagery in the aftermath of natural disasters and humanitarian crises. Unlike continuous monitoring programs, Maxar's initiative is event-driven—activated during emergencies such as hurricanes, wildfires, earthquakes, and conflicts. By releasing timely, publicly available imagery, Maxar empowers responders, analysts, and volunteers with actionable insights for damage assessment, response coordination, and recovery planning. More information about Maxar Open Data is available at [https://registry.opendata.aws/maxar-open-data](https://registry.opendata.aws/maxar-open-data/).

Together, these programs demonstrate the power of cloud-enabled open data to democratize access to geospatial information, promote global collaboration, and drive real-world impact. In this post, we demonstrate how to explore and visualize these datasets using interactive web applications and Jupyter notebooks.

## Interactive Web Applications for Geospatial Data Exploration

To make these datasets more accessible to a wider audience, we’ve developed two interactive web applications that simplify data discovery and visualization. Built with Python and modern web technologies, these tools allow users to explore AWS and Maxar open data directly in the browser.

### 1. Amazon ASDI Data Explorer

The **Amazon ASDI Data Explorer** provides a user-friendly interface for browsing and visualizing datasets available through ASDI and the broader AWS Open Data Program. Key features include:

- Catalog browsing with spatial and temporal filtering
- STAC-based search for satellite and environmental data
- Interactive map-based visualization

This Amazon ASDI Data Explorer is powered by [Leafmap](https://leafmap.org), an open-source Python package for interactive visualization of geospatial data, and deployed using the [Solara](https://solara.dev) web framework on [Hugging Face Spaces](https://huggingface.co/spaces).

The web app and the source code are available at:

- Web app: <https://huggingface.co/spaces/giswqs/Amazon-ASDI>
- GitHub: <https://github.com/opengeos/Amazon-ASDI>

![Amazon ASDI Data Explorer](https://github.com/user-attachments/assets/ad4f484f-e0ef-4c78-9027-694e9b3d6a93)
**Figure 1.** The Amazon ASDI Data Explorer allows users to search and visualize Amazon ASDI datasets interactively.

**How to use:**

1. Visit the [Amazon ASDI Data Explorer](https://huggingface.co/spaces/giswqs/Amazon-ASDI).
2. Click on the “ASDI” tab to launch the interactive map.
3. Zoom and pan to your region of interest. Optionally, you can use the drawing tool to draw a polygon on the map to select a region of interest.
4. In the STAC Search sidebar, choose “AWS Open Data” as the catalog source.
5. Select a dataset (e.g., _10m Annual Land Use Land Cover_) and customize the date range as needed.
6. Click on the “Search” button to search for the datasets. The footprints of the available datasets will be displayed on the map.
7. Select an item from the search results and click on the “Display” button to overlay the data on the map.
8. Use the Layers panel to adjust layer visibility and opacity.
9. Repeat steps 2-8 to search and visualize other datasets or change the region of interest.

This workflow allows users to access and explore complex geospatial datasets in just a few clicks—ideal for educators, researchers, and analysts alike.

---

### 2. Maxar Open Data Explorer

The **Maxar Open Data Explorer** focuses on disaster response and humanitarian mapping by providing interactive access to event-based satellite imagery released through the Maxar Open Data Program. Key features include:

- Browse available disaster events
- View satellite image footprints on the map
- Compare pre- and post-disaster imagery side by side

This Maxar Open Data Explorer is powered by [Leafmap](https://leafmap.org) and deployed using the [Solara](https://solara.dev) web framework on [Hugging Face Spaces](https://huggingface.co/spaces).

The web app and the source code are available at:

- Web app: <https://huggingface.co/spaces/giswqs/solara-maxar>
- GitHub: <https://github.com/opengeos/solara-maxar>

![Maxar Open Data Explorer](https://github.com/user-attachments/assets/94938ae5-bd76-4d26-964f-91c71c48c93f)
**Figure 2.** The Maxar Open Data Explorer allows users to visualize and compare Maxar satellite imagery for disaster events.

**How to use:**

1. Visit the [Maxar Open Data Explorer](https://huggingface.co/spaces/giswqs/solara-maxar).
2. Choose an event (e.g., _Libya_) from menu tab.
3. The application displays the imagery footprints on the map.
4. Pick a start date to filter the imagery taken after the start date.
5. Hover over any footprint to view its metadata displayed in the information panel in the lower left corner.
6. Click on any footprint to view the satellite imagery.
7. Use the Layers panel to control visibility and opacity.
8. Toggle the “Split map” option to compare before-and-after images.

This tool is especially useful for emergency responders, humanitarian agencies, and open-source mapping communities involved in real-time crisis mapping and assessment.

## Accessing and Visualizing Maxar Open Data in Jupyter Notebook

[Leafmap](https://leafmap.org) is a Python package that makes it easy to visualize and analyze geospatial data interactively. It provides seamless integration with AWS S3, STAC catalogs, and various geospatial data formats. With Leafmap, you can create interactive maps, perform geospatial analysis, and visualize results—all within a few lines of Python code.

### Installation

Before we begin exploring the datasets, let's install Leafmap. If you're running this in a Jupyter notebook, you can run the following cell:

```{code-cell} ipython3
# %pip install leafmap
```

This will install Leafmap and its dependencies, including support for various geospatial data formats, cloud-optimized imagery, and interactive mapping capabilities.

### Importing Libraries

Now let's dive into a practical example of accessing and visualizing Maxar Open Data. We'll start by importing the necessary libraries:

```{code-cell} ipython3
import leafmap
import geopandas as gpd
```

Here, we're importing `leafmap` for geospatial visualization and `geopandas` for working with geospatial data structures. GeoPandas extends the popular pandas library to handle geographic data efficiently.

### Discovering Available Disaster Events

The first step is to explore what disaster events are available in the Maxar Open Data catalog. Each collection in the catalog represents a single disaster event with associated satellite imagery:

```{code-cell} ipython3
leafmap.maxar_collections()
```

This function retrieves all available collections from the Maxar Open Data STAC (SpatioTemporal Asset Catalog) catalog. The output will show you a list of disaster events, each with a unique collection ID that we can use to access the specific imagery data.

### Selecting a Disaster Event

For this example, we'll focus on the devastating earthquake that struck Turkey and Syria in February 2023. The collection ID for this event is `Kahramanmaras-turkey-earthquake-23`. We can retrieve the geographic footprints of all available satellite images for this event from the [Maxar Open Data GitHub repository](https://github.com/opengeos/maxar-open-data), which provides both GeoJSON and TSV formats:

```{code-cell} ipython3
collection = "Kahramanmaras-turkey-earthquake-23"
url = leafmap.maxar_collection_url(collection, dtype="geojson")
url
```

This function generates a URL pointing to the GeoJSON file containing the spatial footprints and metadata for all satellite images related to the Turkey earthquake event. The GeoJSON format is particularly useful because it includes both the geographic boundaries of each image and associated metadata.

Now let's load this data and explore what's available:

```{code-cell} ipython3
gdf = gpd.read_file(url)
print(f"Total number of images: {len(gdf)}")
gdf.head()
```

Here we're using GeoPandas to read the remote GeoJSON file directly from the URL. The `gdf.head()` command shows us the first few rows of the dataset, revealing important metadata columns such as:

- **geometry**: The spatial footprint of each satellite image
- **catalog_id**: A unique identifier for grouping related images
- **quadkey**: Microsoft's quadkey system for tile identification
- **timestamp**: When the image was captured
- **visual**: Direct download link to the satellite image
- **event_name**: The disaster event name

### Visualizing Image Footprints

Now let's create an interactive map to visualize where all these satellite images are located geographically:

```{code-cell} ipython3
m = leafmap.Map()
m.add_gdf(gdf, layer_name="Footprints", zoom_to_layer=True)
m
```

![image](https://github.com/user-attachments/assets/13184223-b5cc-4868-836b-39dbb05cdd70)

This creates an interactive map centered on the earthquake region, with blue polygons showing the spatial coverage of each satellite image. You can zoom in, pan around, and click on individual footprints to see their metadata. This visualization helps us understand the geographic extent of the available imagery and identify areas with dense coverage.

### Temporal Analysis: Before and After the Earthquake

The earthquake struck on February 6, 2023, making temporal analysis crucial for damage assessment. Let's separate the imagery into pre-event and post-event datasets using the earthquake date as our temporal boundary.

First, let's get all images captured before the earthquake:

```{code-cell} ipython3
pre_gdf = leafmap.maxar_search(collection, end_date="2023-02-06")
print(f"Total number of pre-event images: {len(pre_gdf)}")
pre_gdf.head()
```

The `leafmap.maxar_search()` function allows us to filter the imagery collection by date. By setting `end_date="2023-02-06"`, we retrieve only images captured before the earthquake occurred. These pre-event images serve as our baseline for comparison.

Now let's get all images captured after the earthquake:

```{code-cell} ipython3
post_gdf = leafmap.maxar_search(collection, start_date="2023-02-06")
print(f"Total number of post-event images: {len(post_gdf)}")
post_gdf.head()
```

By setting `start_date="2023-02-06"`, we retrieve all images captured from the earthquake date onwards. These post-event images will show the immediate aftermath and damage caused by the earthquake.

### Comparing Pre-Event and Post-Event Coverage

Let's create a comparative visualization showing both pre-event and post-event image footprints on the same map:

```{code-cell} ipython3
m = leafmap.Map()
pre_style = {"color": "red", "fillColor": "red", "opacity": 1, "fillOpacity": 0.5}
m.add_gdf(pre_gdf, layer_name="Pre-event", style=pre_style, info_mode="on_click", zoom_to_layer=True)
m.add_gdf(post_gdf, layer_name="Post-event", info_mode="on_click", zoom_to_layer=True)
m
```

![image](https://github.com/user-attachments/assets/7c41b716-1b13-4754-857d-9df084e8bc26)

In this visualization, red polygons represent pre-earthquake imagery while blue polygons show post-earthquake coverage. The `info_mode="on_click"` parameter enables interactive information popups when you click on any footprint. You can toggle layers on/off using the layer control panel, and the different colors help distinguish the temporal coverage patterns.

### Selecting a Region of Interest

To focus our analysis on a specific area, we can define a region of interest (ROI). You can either draw a polygon on the map using the drawing tools, or we'll use predefined coordinates for a particularly affected area:

```{code-cell} ipython3
bbox = m.user_roi_bounds()
if bbox is None:
    bbox = [36.8715, 37.5497, 36.9814, 37.6019]
```

The `m.user_roi_bounds()` function attempts to get the bounding box coordinates from any region you've drawn on the map. If no region is drawn, we fall back to predefined coordinates that cover a significantly impacted area near Kahramanmaraş. The bbox format follows [min_longitude, min_latitude, max_longitude, max_latitude] convention.

### Searching Within the Region of Interest

Now let's search for satellite images that specifically cover our region of interest, filtering by both geographic bounds and temporal constraints.

First, let's find pre-earthquake images within our ROI:

```{code-cell} ipython3
pre_event = leafmap.maxar_search(collection, bbox=bbox, end_date="2023-02-06")
pre_event.head()
```

This search combines spatial filtering (using the bounding box) with temporal filtering (images before February 6, 2023). The result shows us only images that both intersect our geographic area of interest and were captured before the earthquake.

Similarly, let's find post-earthquake images within the same area:

```{code-cell} ipython3
post_event = leafmap.maxar_search(collection, bbox=bbox, start_date="2023-02-06")
post_event.head()
```

This gives us the corresponding post-earthquake imagery for the same geographic region, enabling direct before-and-after comparison.

### Preparing Images for Visualization

Maxar organizes satellite images into tiles, where each tile can contain multiple individual images identified by unique quadkeys. Let's extract the tile identifiers we need for visualization:

```{code-cell} ipython3
pre_tile = pre_event["catalog_id"].values[0]
pre_tile
```

This gets the catalog ID for the first pre-event tile in our search results. The catalog ID serves as a unique identifier for a collection of related satellite images covering the same geographic area.

```{code-cell} ipython3
post_tile = post_event["catalog_id"].values[0]
post_tile
```

Similarly, this extracts the catalog ID for the post-event tile. Having both pre- and post-event tile IDs allows us to create comparative visualizations.

### Creating Web-Optimized Tile Services

To display these high-resolution satellite images efficiently in a web browser, we need to convert them to MosaicJSON format, which enables dynamic tiling and streaming:

```{code-cell} ipython3
pre_stac = leafmap.maxar_tile_url(collection, pre_tile, dtype="json")
pre_stac
```

This generates a MosaicJSON URL for the pre-event tile. MosaicJSON is a specification that allows multiple raster files to be presented as a single web-optimized layer, enabling efficient visualization of large satellite imagery datasets.

```{code-cell} ipython3
post_stac = leafmap.maxar_tile_url(collection, post_tile, dtype="json")
post_stac
```

Similarly, this creates the MosaicJSON URL for the post-event imagery. These URLs point to tile services that can stream the imagery directly to our interactive map.

### Creating a Split-Screen Comparison

Now comes the powerful part—creating a side-by-side comparison to visualize the earthquake's impact:

We re-import leafmap to ensure we're using the latest configuration (particularly important for the folium backend which handles split maps better than ipyleaflet).

```{code-cell} ipython3
m = leafmap.Map()
m.split_map(
    left_layer=pre_stac,
    right_layer=post_stac,
    left_label="Pre-event",
    right_label="Post-event",
)
m.set_center(36.9265, 37.5762, 16)
m
```

![image](https://github.com/user-attachments/assets/1a43a737-e9ff-4c9d-b301-ff4c37787d44)

+++

This creates an interactive split-screen map where you can:

- **Left side**: View the area before the earthquake
- **Right side**: View the same area after the earthquake
- **Divider**: Drag the vertical divider left or right to compare different parts of the scene
- **Synchronization**: Both sides pan and zoom together, maintaining perfect geographic alignment

The `m.set_center()` function centers the map on the most impacted area at zoom level 16, providing detailed street-level view of the earthquake damage.

### Downloading Images for Offline Analysis

If you need to perform detailed analysis or store images locally, you can download the original high-resolution imagery:

```{code-cell} ipython3
pre_images = pre_event["visual"].tolist()
post_images = post_event["visual"].tolist()
```

This extracts the direct download URLs from our filtered datasets. The "visual" column contains links to the processed, analysis-ready satellite images in RGB format that are optimized for visual interpretation.

Now let's download the pre-event images:

```{code-cell} ipython3
leafmap.maxar_download(pre_images)
```

The `leafmap.maxar_download()` function handles the download process, saving images to your local directory with organized naming conventions. These high-resolution images can then be used for:

- Detailed damage assessment
- Machine learning training datasets
- Offline mapping applications
- Scientific research and analysis

```{code-cell} ipython3
# leafmap.maxar_download(post_images)
```

The post-event images are commented out to avoid long download times in this example, but you can uncomment this line to download the post-earthquake imagery as well. Each image is typically several megabytes in size, so consider your bandwidth and storage constraints when downloading large datasets.

## Conclusions

Access to high-quality geospatial data is no longer limited to technical experts with large computing resources. Thanks to open data initiatives like the AWS Open Data Program, ASDI, and the Maxar Open Data Program, coupled with intuitive tools like Leafmap and Solara, anyone can explore and visualize critical Earth data in minutes.

Whether you're a researcher investigating climate trends, a student exploring land cover dynamics, or a volunteer aiding in disaster mapping, these tools offer a powerful gateway to cloud-hosted open data—turning raw datasets into actionable insight.
