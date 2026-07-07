# floor-predictor

OSM-based living-building and floor-count prediction.

## Scheme

```mermaid
flowchart LR
    A[Inputs] --> B[Run: pipeline.ipynb]
    B --> C[Checked outputs]
    C --> D[Paper / thesis use]
```

## Main Result

![Main result](docs/readme_result.svg)

## Run

Entrypoint: `pipeline.ipynb`

Human:

```bash
pip install -e . && jupyter notebook pipeline.ipynb
```

Agent:

Reuse existing processors before adding data cleaners.

## Publication

No standalone publication tracked.

## Next Steps / Heuristics

Heuristic: random-forest baseline first; only add model complexity after checked error cases.
