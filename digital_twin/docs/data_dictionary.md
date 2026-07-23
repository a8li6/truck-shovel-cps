# Data Dictionary

All values in this project are synthetic engineering assumptions
created for software verification and comparative experiments.
They are not derived from any operating mine.

## Input Files

### `data/scenarios/base_scenario.json`

| Section | Field | Meaning |
|---|---|---|
| `simulation` | `duration_minutes` | Total simulated shift length in minutes. |
| `simulation` | `warmup_minutes` | Early period excluded from KPI reporting. |
| `simulation` | `seed` | Base random seed for reproducibility. |
| `learning` | `ewma_alpha` | EWMA learning rate for online estimates. |
| `learning` | `minimum_observations` | Observations required before trusting an estimate. |
| `learning` | `switch_penalty_minutes` | Penalty discouraging excessive reassignment. |
| `fleet` | `number_of_trucks` | Number of truck processes to instantiate. |
| `fleet` | `payload_mean/std/min/max` | Truncated-normal payload parameters in tonnes. |
| `shovels[]` | `id`, `loading_min/mode/max` | Per-shovel triangular loading-time parameters in minutes. |
| `dumps[]` | `id`, `dump_min/mode/max` | Per-dump triangular service-time parameters in minutes. |

### `data/scenarios/routes.csv`

| Column | Type | Meaning |
|---|---|---|
| `origin` | string | Starting location code (e.g. `D1`, `S1`). |
| `destination` | string | Ending location code. |
| `load_state` | string | `empty` or `loaded`, indicating direction of travel. |
| `distance_km` | float | Route distance in kilometres. |
| `mean_speed_kph` | float | Average travel speed for that route in km/h. |

## Generated Files

### `data/generated/service_time_samples.csv`

| Column | Type | Meaning |
|---|---|---|
| `sample_id` | int | Sample index (1..500) within a resource/activity. |
| `resource_type` | string | `shovel` or `dump`. |
| `resource_id` | string | Resource identifier (e.g. `S1`, `D2`). |
| `activity` | string | `loading` or `dumping`. |
| `duration_minutes` | float | Sampled activity duration from triangular distribution. |

### `data/generated/travel_time_samples.csv`

| Column | Type | Meaning |
|---|---|---|
| `sample_id` | int | Sample index within a route. |
| `origin` | string | Route origin. |
| `destination` | string | Route destination. |
| `load_state` | string | `empty` or `loaded`. |
| `travel_minutes` | float | Sampled travel duration (mean × triangular multiplier). |

### `data/generated/payload_samples.csv`

| Column | Type | Meaning |
|---|---|---|
| `sample_id` | int | Sample index. |
| `payload_tonnes` | float | Truncated-normal payload sample clipped to [82, 102] tonnes. |

### `data/generated/generation_summary.json`

| Field | Meaning |
|---|---|
| `seed` | Seed used for this generation run. |
| `samples_per_distribution` | Number of samples per distribution. |
| `service_rows` | Total rows in `service_time_samples.csv`. |
| `travel_rows` | Total rows in `travel_time_samples.csv`. |
| `payload_rows` | Total rows in `payload_samples.csv`. |

## Event Log (added from Day 3 onward)

Columns: `replication_id`, `seed`, `sim_time_min`, `event_type`,
`truck_id`, `origin`, `destination`, `shovel_id`, `dump_id`,
`queue_length`, `duration_min`, `payload_tonnes`, `policy`,
`decision_score`, `estimate_before`, `estimate_after`, `notes`.
