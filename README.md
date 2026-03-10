# mea-raster-asdr

Python script for analysing multielectrode array (MEA) recordings from the **Axion Maestro** system.

Generates per-well figures matching the output of the MATLAB pipeline (`Axion_TVAloopv3` / `v4`):

- **Top panel** — ASDR histogram (spike counts per 200 ms bin, summed across all 16 electrodes)
- **Lower rows** — Raster plot (one row per electrode, vertical tick per spike)
- **Standalone ASDR PNG** and **statistics CSV** also saved

---

## Supported input files

| Extension | Description |
|-----------|-------------|
| `.spk` | Axion spike file — recommended, no extra parameters needed |
| `.raw` | Axion Maestro continuous raw recording (spike detection on-the-fly) |
| `.npz` | Pre-parsed spike cache |

---

## Installation

```bash
pip install numpy matplotlib scipy
```

Python 3.8+ required.

---

## Usage

Edit the **USER SECTION** at the top of `mea_raster_asdr.py`:

```python
# ── Input file ────────────────────────────────────────────────
INPUT_FILE = r"C:\path\to\your_recording.spk"

# ── Output folder ─────────────────────────────────────────────
OUTPUT_DIR = None   # None = auto-create folder next to input file

# ── Wells to analyse ──────────────────────────────────────────
WELLS = ["A1"]              # single well
WELLS = ["A1", "B3", "C5"] # multiple wells
WELLS = ["ALL"]             # every well with spikes

# ── Time window (seconds) ─────────────────────────────────────
TIME_START = 0
TIME_END   = 420    # or None for full recording

# ── Recording duration (only needed for .raw / .npz) ──────────
REC_SECONDS = 360.0

# ── ASDR threshold (absolute spike count — drawn as red dashed line) ──
ASDR_THRESH = 10

# ── Y-axis maximum for ASDR panels (None = autoscale) ─────────
ASDR_Y_MAX = None   # e.g. 100
```

Then run:

```bash
python mea_raster_asdr.py
```

---

## Output

All figures are saved to `mea_raster_asdr_output/` next to the input file (or to `OUTPUT_DIR` if set):

| File | Description |
|------|-------------|
| `<stem>_<well>_raster_histogram.png` | Combined raster + ASDR figure |
| `<stem>_<well>_asdr_histogram.png` | Standalone ASDR histogram |
| `<stem>_stats.csv` | Per-well statistics (spike counts, rates, ISI, bursts) |

---

## Background

This script is a Python port of the MATLAB MEA analysis pipeline developed in the **Deane Lab**, University of Cambridge. It reads the Axion binary `.spk` container format based on the official AxIS MATLAB source (`AxisFile.m`, `BlockVectorData.m`, etc.) and replicates the spike detection (MAD threshold, identical to MATLAB) and ASDR histogram used in the original pipeline.
