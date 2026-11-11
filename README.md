# livestock-grazing-intensity

livestock-grazing-intensity

## What it does

| Step | What it does |
|---|---|
| **Set Er Connection** |  |
| **Set Time Range** | Choose the period of time to analyze. |
| **Get Events** | Get events. |
| **Get Events** | Get events. |
| **Extract Column As Type** |  |
| **Extract Column As Type** |  |
| **Get Features By Type** | Fetch every feature of the chosen EarthRanger feature types as a GeoDataFrame, with each feature's type and set. |
| **Estimate Utm Crs** | Estimate the best UTM CRS for a GeoDataFrame and return it as an EPSG string. |
| **Normalize Json Column** |  |
| **Normalize Json Column** |  |
| **Drop livestock column prefix** | Remove a prefix from column names in a DataFrame with duplicate handling. |
| **Drop cattle column prefix** | Remove a prefix from column names in a DataFrame with duplicate handling. |
| **Add Area Columns** | Add one area column per requested unit, named `{prefix}_{unit}` (e.g. area_m2, area_ha, area_km2, area_acre, area_mi2). |
| **Map Columns** | Maps and transforms the columns of a DataFrame based on the provided parameters. The order of the operations is as follows: drop columns, retain/reorder columns, and rename columns. |
| **Merge Block Movements** | Merge all columns that contain from/to block information into two columns, FROM and TO. |
| **Map Column Value** | Map values in ``col`` through ``mapping`` and write the result to a new column. |
| **Apply Color Map** | Adds a color column to the dataframe based on the categorical values in the specified column. |
| **Map Columns** | Maps and transforms the columns of a DataFrame based on the provided parameters. The order of the operations is as follows: drop columns, retain/reorder columns, and rename columns. |
| **Build Grazing Metrics** | One row per grazing period (zone, block set, date in -> date out), split by cattle count changes. |
| **Draw Grouped Line Time Series Chart** | Multi-line time series chart, optionally stacked as filled area. |
| **Draw Grouped Line Time Series Chart** | Multi-line time series chart, optionally stacked as filled area. |
| **Draw Stat Sparkline** | Single value widget with a sparkline: a headline number (value_column aggregated over the whole frame), an optional caption (e.g. "182 patrols with sightings") and a sparkline of the same aggregate per time bucket. Returned as plotly HTML for create_plot_widget_single_view. |
| **Draw Stat Sparkline** | Single value widget with a sparkline: a headline number (value_column aggregated over the whole frame), an optional caption (e.g. "182 patrols with sightings") and a sparkline of the same aggregate per time bucket. Returned as plotly HTML for create_plot_widget_single_view. |
| **Draw Stat Sparkline** | Single value widget with a sparkline: a headline number (value_column aggregated over the whole frame), an optional caption (e.g. "182 patrols with sightings") and a sparkline of the same aggregate per time bucket. Returned as plotly HTML for create_plot_widget_single_view. |
| **Merge Two Dataframes** | Merge two DataFrames using a database-style join. |
| **Persist Stock Intensity Text** | Persist text to a file or cloud storage object. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Dataframe To Geodataframe** | Convert a plain DataFrame into a GeoDataFrame. |
| **Create Stock Intensity Sparkline View** | Create a plot widget with a single view. |
| **Create Plot Widget Single View** | Create a plot widget with a single view. |
| **Create Plot Widget Single View** | Create a plot widget with a single view. |
| **Create Plot Widget Single View** | Create a plot widget with a single view. |
| **Create Plot Widget Single View** | Create a plot widget with a single view. |
| **Apply Choropleth Colormap** | Colours a numeric column for choropleth display using Natural Breaks (Jenks) classification. |
| **Compute View State From Gdf** |  |
| **Create Geojson Layer** | Creates a GeoJSON layer definition based on the provided configuration. |
| **Create Geojson Layer** | Creates a GeoJSON layer definition based on the provided configuration. |
| **Combine Deckgl Map Layers** | Combine static and grouped map layers into a render-ready list. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Set Base Maps** |  |
| **Draw Map** | Creates a map based on the provided layer definitions and configuration. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Create Map Widget Single View** | Create a map widget with a single view. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Set Workflow Details** | Add information that will help to differentiate this workflow from another. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Set Groupers** |  |
| **Persist Df** | Persist dataframe to a file or cloud storage object. |
| **Gather Dashboard** |  |

## Requirements

- Access to a **EarthRanger** data source, set up as a connection

---

## 1. Load the Workflow

In the workflow runner, add a new template using this repository's URL. Once added, select
**livestock-grazing-intensity** from the available templates list to load it.

---

## 2. Configure the Workflow

You'll be prompted to fill in the following parameters before running.

### Set Er Connection

| Field | Description | Default |
|---|---|---|
| Data source *(required)* | Select one of your configured data sources. | — |

### Set Time Range

| Field | Description | Default |
|---|---|---|
| Since *(required)* | The start time | — |
| Until *(required)* | The end time | — |
| Timezone |  | — |

### Get Events

| Field | Description | Default |
|---|---|---|
| Event Types *(required)* | Specify the event type(s) to analyze (optional). Leave this section empty to analyze all event types. | — |

### Get Events

| Field | Description | Default |
|---|---|---|
| Event Types *(required)* | Specify the event type(s) to analyze (optional). Leave this section empty to analyze all event types. | — |

### Get Features By Type

| Field | Description | Default |
|---|---|---|
| Feature Type Names *(required)* | Names of the feature types to fetch, e.g. Park or Census Block. | — |

### Set Workflow Details

| Field | Description | Default |
|---|---|---|
| Workflow Name *(required)* |  | — |

---

## Developing

```bash
# recompile spec.yaml (first time: --install; after that: --update, which bumps VERSION.yaml)
./dev/recompile.sh --update

# run the test cases in test-cases.yaml
pixi run --manifest-path pixi.toml -e default bash dev/pytest-cli.sh livestock_grazing_intensity --all
```

Documentation: [docs/index.html](docs/index.html).
