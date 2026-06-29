# Fire Forecast in Greece 

Machine-learning project for estimating the size of a forest fire at the moment it breaks out, using **only information available before or at the start** of the fire (meteorological and spatial data).

## Objective

The problem is approached in two ways:

- **Regression** — predicting the burned area (on a logarithmic scale).
- **Classification** — predicting whether a fire will be small or large.

The methodology is based on the work of **Cortez & Morais (2007)**, which addresses an equivalent problem (prediction of burned area from meteorological and spatial data), applied here to entirely different and much larger data.

## Data

- **Source:** open data of the [Hellenic Fire Service](https://www.fireservice.gr/el/synola-dedomenon).
- **Period:** 2012–2025 (2012–2024 training set, 2025 test set).
- **Volume:** ~132,515 initial records, ~117,550 valid records after cleaning.
- **Meteorological:** retrieved per municipality and day from [Open-Meteo](https://open-meteo.com/) (temperature, wind, wind direction, humidity, precipitation).
- **Geocoding:** coordinates via the centroids of administrative boundaries ([GADM](https://gadm.org/) — `gadm41_GRC_*` files).

## Preprocessing & Feature Engineering

1. **Unification** of annual files with different structures into a single schema.
2. **Municipality matching** (Kallikratis/Kleisthenis) via fuzzy matching, with manual completion.
3. **Geocoding** via GADM.
4. **Cleaning** (removal of fires with zero burned area).

Three approximate drought indices were built: `Rain_7d`, `Rain_30d`, and `Days_without_rain`.

## Methodological choices

- Chronological train/test split (2012–2024 / 2025).
- Logarithmic transformation of the target (`log1p`) due to extreme asymmetry.
- One-hot encoding for the categorical variables.
- `StandardScaler` fit **only on the train set** (avoiding data leakage).
- Careful exclusion of variables known only after the fire (duration, extinguishing forces, etc.), so the model is useful in real conditions.


## Key conclusions

As prior research also recognizes, the problem contains extreme points that are hard to predict from meteorological data alone. The low R² and the difficulty of predicting the very large fires are not a weakness of the approach, but an inherent characteristic of the problem, factors such as arson, dry grass, or vegetation type are not available in the data.

## Authors
- Alexandros Korovesis — [@a-korovessis](https://github.com/a-korovessis)
- Evangelia Ntokou — [@evantokou](https://github.com/evantokou)
