# Network Traffic Analysis Toolkit

Analysis of ~2.07M network flow records (34,801 distinct IPs) covering basic traffic statistics, sublinear-space stream estimation, and multi-stage anomaly/threat detection — done in a single Jupyter notebook.

## Part 1 — Basic traffic statistics
Protocol breakdown, top talkers by byte volume, packet-size distribution, and detection of hosts with consistent long-duration communication patterns and traffic spikes (largest: 7.7 GB in a single hour window).

## Part 2 — Sublinear-space traffic estimation
Compares exact vs. approximate streaming algorithms on the same dataset:
- **Unique-IP estimation** — exact (hash set) vs. a bitmap-style probabilistic estimator: 34,801 exact vs. 34,805 estimated (0.012% error).
- **Heavy hitters** — exact destination-IP counts vs. a Count-Min Sketch (max error 0.586%).
- **Bloom filter** membership testing for blocklists — 0 false negatives, 1.2% false positive rate.

## Part 3 — Advanced anomaly detection
Standard-deviation-based thresholds surface hourly flow-count spikes and protocol-distribution shifts; behavioral analysis flags coordinated many-to-one targeting (e.g. one destination IP contacted by up to 468 distinct sources within a single hour) and long-duration/multi-protocol flows.

## Part 4 — Deep security threat analysis
Cross-references rotating-source targeting, encrypted-looking traffic on unusual ports (4444, 5555) carrying multi-GB payloads labeled `Attack`, and payload heuristics for C2-like repetition, then rolls everything up into a High/Medium/Low risk classification per host. The dominant finding: sustained, high-source-diversity targeting of a single destination IP, corroborated by large encrypted-like flows on non-standard ports.

## Running it

The notebook expects the six input CSV files (network flow records — not included here, as they're course-provided data) at a path resolved via:

```python
candidates = [
    Path('/content/drive/MyDrive/NSS_A3'),  # Google Colab
    Path('/content/drive/MyDrive'),
    Path('/content'),
    Path.cwd(),                             # local fallback
]
```

Point that at wherever the CSVs live (or run in Colab with Drive mounted) and run all cells top to bottom. Requires `pandas`, `numpy`.

See `report.pdf` for the full write-up and findings.
