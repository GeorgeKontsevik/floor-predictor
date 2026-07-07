# floor-predictor

---

![License](https://img.shields.io/github/license/GeorgeKontsevik/floor-predictor?style=flat&logo=opensourceinitiative&logoColor=white&color=blue)
[![OSA-improved](https://img.shields.io/badge/improved%20by-OSA-yellow)](https://github.com/aimclub/OSA)

---

## Table of Contents

- [Overview](#overview)
- [Core Features](#core-features)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Architecture](#architecture)
- [Contributing](#contributing)
- [License](#license)
- [Citation](#citation)

---

## Overview

floor-predictor is a Python project for building geospatial machine-learning workflows that predict building floor-related labels and whether a building is residential using OpenStreetMap-derived features. It is aimed at developers, data scientists, and researchers working with urban morphology or OSM-based prediction experiments, including those who want to adapt the pipeline to their own geospatial data. The repository provides a notebook-driven workflow backed by modular Python code for fetching boundaries, preparing building and road features, adding amenities, and training or applying a model. For runnable steps and a concrete end-to-end example, start with the Getting Started material.

---

## Core Features

- Build geospatial feature tables from OpenStreetMap buildings, roads, and amenities, giving developers a single pipeline for turning raw urban data into model-ready inputs.
- Enrich building footprints with land-use context and derived location features, which helps capture neighborhood morphology around each object.
- Normalize categorical building and land-use labels into a consistent encoded feature set, making the output suitable for downstream machine learning models.
- Train, save, load, and reuse classifiers for floor and living-space prediction, which supports both experimentation and repeatable inference.
- Run the pipeline on custom building, road, and amenity layers instead of OSM downloads, which makes the project adaptable to external geospatial datasets.

---

## Installation

**Prerequisites:** requires Python >=3.10

Install floor-predictor using one of the following methods:

**Build from source:**

1. Clone the floor-predictor repository:
```sh
git clone https://github.com/GeorgeKontsevik/floor-predictor
```

2. Navigate to the project directory:
```sh
cd floor-predictor
```

3. Install the project dependencies:

```sh
pip install -r requirements.txt
```

---

## Getting Started

### Prerequisites

- Python project with notebook-driven pipeline.
- Required Python packages are listed in `requirements.txt` and `pyproject.toml`.
- The example workflow uses Jupyter notebook `pipeline.ipynb`.

### Quick start

1. Install the project dependencies from the repository files.
2. Open `pipeline.ipynb`.
3. Load or prepare input data. The notebook shows a cached dataset at `data/processed_data_spb.pkl`.
4. Run the preprocessing cells to build `full_gdf`, then create `data` from it.
5. Train a model with `ModelHandler` and save it to the repository-backed path used in the notebook:

```python
handler = ModelHandler("floor_predictior/model/model_dt.pkl", df=data, target_col="is_living")
handler.set_model(external_model)
handler.train_model(X_train, y_train, save=True)
```

6. Load the saved model from the same path and run predictions on the rows where `is_living` is missing:

```python
handler = ModelHandler("floor_predictior/model/model_dt.pkl", df=predict_data, target_col="is_living")
handler.load_model_from_file()
predicted = handler.predict(predict_data, map_labels=False)
```

### Using your own data

The repository also documents a path for running `osm_living_predictor` on your own layers instead of downloading from OSM. The notebook section starts with `## Использование osm_living_predictor на своих данных` and describes the expected building, road, and amenity layers.

---

## Architecture

The repository is organized as a notebook-driven geospatial ML workflow with reusable Python modules under `floor_predictior/`.

- `pipeline.ipynb` shows the end-to-end flow: fetch a district boundary, load buildings and landuse, add road- and amenity-based context, build features, assign `is_living` labels, and then train or apply a classifier.
- `floor_predictior/osm_living_predictor/` contains the main processing steps used by that pipeline: boundary lookup, building and landuse preparation, road and amenity feature enrichment, feature assembly, and model handling.
- `floor_predictior/utils/` provides shared helpers and constants used during preprocessing and labeling.
- The saved `StoreyModelTrainer.joblib` artifact indicates that trained model state is persisted alongside the code for later reuse.
- Based on the provided tree, this is a single-repository pipeline rather than a multi-service system; no separate service boundaries or orchestration layer are evident.

---

## Contributing

- **[Report Issues](https://github.com/GeorgeKontsevik/floor-predictor/issues)**: Submit bugs found or log feature requests for the project.

- **[Submit Pull Requests](https://github.com/GeorgeKontsevik/floor-predictor/tree/main/CONTRIBUTING.md)**: To learn more about making a contribution to floor-predictor.

---

## License

This project is protected under the MIT License. For more details, refer to the [LICENSE](https://github.com/GeorgeKontsevik/floor-predictor/tree/main/LICENSE) file.

---

## Citation

If you use this software, please cite it as below.

### APA format:

    GeorgeKontsevik (2026). floor-predictor repository [Computer software]. https://github.com/GeorgeKontsevik/floor-predictor

### BibTeX format:

    @misc{floor-predictor,

        author = {GeorgeKontsevik},

        title = {floor-predictor repository},

        year = {2026},

        publisher = {github.com},

        journal = {github.com repository},

        howpublished = {\url{https://github.com/GeorgeKontsevik/floor-predictor}},

        url = {https://github.com/GeorgeKontsevik/floor-predictor}

    }

---