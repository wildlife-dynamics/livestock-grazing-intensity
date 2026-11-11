# livestock-grazing-intensity

Shows how hard each grazing block is grazed and how long it rests between grazings. The workflow reads livestock movement events and cattle count events from EarthRanger, and grazing block features for the block areas. It splits each zone's movements into grazing periods and calculates stocking density, grazing intensity and recovery time for every period.

## What it produces

**Dashboard**

| Widget | Shows |
|---|---|
| Stocking Intensity | Average cattle-days per hectare across all grazing periods, with a monthly sparkline. |
| Stocking Density | Average cattle per hectare, with a monthly sparkline. |
| Grazing to Rest Ratio | Average days grazed per day rested, with a monthly sparkline. |
| Grazing Intensity | Monthly average cattle-days per hectare, one line per grazing zone. |
| Effective Grazing Intensity | Monthly average cattle per hectare over the full graze-and-rest cycle, one line per grazing zone. |
| Livestock Count | Map of grazing zones coloured by cattle count (5 Natural Breaks classes), over the outlines of all fetched features. |

**Files** (in the results folder)

- A CSV of the grazing metrics table, one row per grazing period (see below).
- The HTML for each chart, sparkline and the map.

## How the metrics are worked out

1. **Grazing periods.** Each livestock movement event starts a period in the blocks it moved *to*. The period ends when the next movement event in the same zone starts. Dates are taken at day level. Repeated moves to the same blocks on the same day count as one move.
2. **Area.** The sum of the areas, in hectares, of the period's blocks. Block names are matched to feature names ignoring case and spaces. If any block has no matching feature, the area is left blank and the block is logged as a warning.
3. **Cattle.** The zone's most recent count on or before the date in. Counts taken during the period add a new row from the day of the count. Several counts for one zone on the same day are added together.
4. **Recovery.** The days since any of the same blocks was last left.

| Column | Formula |
|---|---|
| Grazing duration (days) | date out − date in |
| Recovery days | date in − previous grazing end date |
| Stocking density (cattle/ha) | cattle ÷ area |
| Stock intensity (cattle-days/ha) | cattle × duration ÷ area |
| Grazing intensity (cattle-days/ha) | cattle × duration ÷ area (the same value as stock intensity) |
| Grazing to Rest ratio | duration ÷ recovery days (blank when recovery is 0 or unknown) |
| Effective grazing intensity | cattle × duration ÷ (area × (duration + recovery days)) |

Some values are blank by design:

- The most recent period in each zone has no date out yet, so it has no duration or intensity.
- A block grazed for the first time within the time range has no recovery days.

## Requirements

An **EarthRanger** data source, set up as a connection, with:

- **Livestock movement events**: an event type whose details include a `communityzone` field and fields for the blocks moved from and to. Fields named `from`, `*_from`, `to`, `to_block` or `*_to` are all merged, and each can hold one block name or a list of names.
- **Cattle count events**: an event type whose details include `communityzone` and `count` fields. The run stops if either field is missing.
- **Grazing block features**: one or more feature types whose feature names match the block names used in the movement events. For the map, they must also include a polygon for each grazing zone, named exactly like its `communityzone` value.

---

## 1. Load the Workflow

In the workflow runner, add a new template using this repository's URL. Once added, select
**livestock-grazing-intensity** from the available templates list to load it.

---

## 2. Configure the Workflow

You'll be prompted to fill in the following parameters before running.

### Workflow Details

| Field | Description | Default |
|---|---|---|
| Workflow Name *(required)* | A name for this run, shown on the dashboard. | — |

### Time Range

| Field | Description | Default |
|---|---|---|
| Since *(required)* | The start time. | — |
| Until *(required)* | The end time. | — |
| Timezone | The timezone for the dates. | — |

Only events inside the range are used. Widen it to get recovery days for blocks first grazed early in the range, and a cattle count for the first period.

### Data Source

| Field | Description | Default |
|---|---|---|
| Data source *(required)* | The EarthRanger connection to read events and features from. | — |

### Livestock Movement Events

| Field | Description | Default |
|---|---|---|
| Event Types *(required)* | The event type(s) that record livestock moving between blocks. | — |

### Cattle Count Events

| Field | Description | Default |
|---|---|---|
| Event Types *(required)* | The event type(s) that record cattle counts per zone. | — |

### Grazing Block Features

| Field | Description | Default |
|---|---|---|
| Feature Type Names *(required)* | The EarthRanger feature type(s) holding the grazing blocks and the grazing zones, e.g. "Grazing Blocks" and "Grazing Zones". | — |

### Base Maps

| Field | Description | Default |
|---|---|---|
| Base maps | Tile layers under the map, bottom layer first. | World Topo Map, then World Imagery at 50% opacity |

---

## Developing

```bash
# recompile spec.yaml (first time: --install; after that: --update, which bumps VERSION.yaml)
./dev/recompile.sh --update

# run the test cases in test-cases.yaml
pixi run --manifest-path pixi.toml -e default bash dev/pytest-cli.sh livestock_grazing_intensity --all
```

The grazing tasks (`merge_block_movements`, `build_grazing_metrics`, `add_area_columns`, `dataframe_to_geodataframe`) come from `ecoscope-workflows-ext-distance-sample-counts`.

Documentation: [docs/index.html](docs/index.html).
