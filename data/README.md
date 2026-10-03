# Data

## Dataset and Attribution

This project uses the UCI Machine Learning Repository dataset **Default of Credit Card Clients**.

- Creator: I-Cheng Yeh
- Source: UCI Machine Learning Repository
- UCI dataset page: https://archive.ics.uci.edu/dataset/350/default
- DOI: https://doi.org/10.24432/C55S3H
- License: Creative Commons Attribution 4.0 International (CC BY 4.0)

Attribution:

> Yeh, I. (2009). Default of Credit Card Clients [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C55S3H

## Local Raw Data

The expected local file is [`raw/UCI_Credit_Card.csv`](raw/UCI_Credit_Card.csv). It contains 30,000 data rows and 25 columns: one ID column, 23 predictor columns, and one binary target column.

The target is `default.payment.next.month`, where `1` indicates default and `0` indicates non-default.

The file under `raw/` is immutable source data for this project: do not edit, clean, reorder, or overwrite it directly. Any future derived dataset must be created reproducibly at a separate path.

## Reproducing the Local Data Layout

Future users should obtain **Default of Credit Card Clients** from the UCI dataset page linked above and place the CSV at:

```text
data/raw/UCI_Credit_Card.csv
```

The analysis notebooks expect that exact repository-relative path and filename. GitHub should not be treated as the primary dataset distribution source. The CSV is already tracked in this project's frozen history, so Stage 6.1 keeps the current file and history intact rather than rewriting history or creating ambiguity in the completed analytical record.
