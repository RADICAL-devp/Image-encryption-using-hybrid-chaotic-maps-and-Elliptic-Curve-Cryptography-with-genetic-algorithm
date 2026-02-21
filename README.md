# Hybrid Image Encryption System

  A four-phase high-security image encryption pipeline combining <b>Lorenz Chaos</b>, <b>Hybrid LPWLCM Diffusion</b>, <b>ECC ElGamal</b>, and <b>Genetic Algorithm Optimization</b>.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11%2B-blue.svg" />
  <img src="https://img.shields.io/badge/Status-Research%20Grade-success" />
  <img src="https://img.shields.io/badge/Security-High-red" />
</p>



<p align="center">
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" width="50" />
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/numpy/numpy-original.svg" width="50" />
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/opencv/opencv-original.svg" width="50" />
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/matplotlib/matplotlib-original.svg" width="50" />
</p>

**Core Technologies**

- Python 3.11+
- NumPy — vectorized numerical computation
- OpenCV — image I/O and processing
- Matplotlib — statistical visualization
- Numba — JIT acceleration *(optional but recommended)*
- Multiprocessing — parallel ECC operations

---

## Table of Contents

- [Overview](#-overview)
- [Algorithm Pipeline](#-algorithm-pipeline)
- [Mathematical Foundations](#-mathematical-foundations)
- [Security Results](#-security-results)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Usage](#-usage)
- [Output Files](#-output-files)
- [Limitations](#-limitations)

---

## Overview

This project implements a **hybrid chaos–cryptography image encryption framework** inspired by:

> *"Novel image encryption algorithm utilizing hybrid chaotic maps and Elliptic Curve Cryptography with genetic algorithm"*

The system encrypts grayscale images through **four tightly coupled cryptographic phases**, each targeting a specific security primitive:

| Phase | Technique | Cryptographic Role |
|-------|-----------|-------------------|
| 1 | Lorenz Chaotic Permutation | Confusion — spatial decorrelation |
| 2 | Hybrid LPWLCM XOR | Diffusion — global pixel influence |
| 3 | ECC ElGamal (P-256) | Public-key semantic security |
| 4 | Genetic Algorithm | Residual correlation minimization |

---

## Algorithm Pipeline

```
Plain Image  (M × N grayscale)
        │
        ▼
┌───────────────────────────────┐
│  Phase 1 — Lorenz Confusion   │
│  Permute pixel positions      │
│  A'[i] = A[p_i]               │
└───────────────────────────────┘
        │
        ▼
┌───────────────────────────────┐
│  Phase 2 — LPWLCM Diffusion   │
│  XOR every pixel              │
│  I_d(i,j) = I_c(i,j) ⊕ S     │
└───────────────────────────────┘
        │
        ▼
┌───────────────────────────────┐
│  Phase 3 — ECC ElGamal        │
│  Per-pixel encryption         │
│  C1 = r·G,  C2 = M + (r·K).x │
└───────────────────────────────┘
        │
        ▼
┌───────────────────────────────┐
│  Phase 4 — GA Optimization    │
│  Minimize inter-pixel         │
│  correlation over 100 gens    │
└───────────────────────────────┘
        │
        ▼
  Final Cipher Image
```

---

## Mathematical Foundations

### Lorenz Chaotic System (Confusion)

```
dx/dt = σ(y − x)
dy/dt = x(ρ − z) − y
dz/dt = xy − βz
```

**Parameters:** σ = 10,  ρ = 28,  β = 2.66

The Lorenz attractor provides extreme sensitivity to initial conditions, producing a non-periodic permutation key that destroys spatial correlation.

---

### Hybrid LPWLCM (Diffusion)

```
         ⎧  r·x·(1−x)      if x < 0.5
f(x) =   ⎨  x / p          if 0.5 ≤ x < p
         ⎩  (1−x) / (1−p)  if x ≥ p
```

**Parameters:** r = 3.99,  p = 0.7

Combines logistic map nonlinearity with PWLCM diffusion strength, improving keystream uniformity and diffusion quality.

---

### Elliptic Curve ElGamal

Curve: `y² = x³ + ax + b (mod p)`

Security relies on the **Elliptic Curve Discrete Logarithm Problem (ECDLP)**.

**Encryption (per pixel M):**
```
r  ← random in [1, n−1]
C1 = r · G
S  = r · PublicKey
C2 = (M + S.x) mod n

Ciphertext: (C1, C2)
```

**Decryption:**
```
S = PrivateKey · C1
M = (C2 − S.x) mod n  &  0xFF
```

---

### Genetic Algorithm Optimization

**Fitness function:**
```
fitness(I) = ( |r_H| + |r_V| + |r_D| ) / 3
```

where Pearson correlation: `r = cov(τ,ν) / √(D(τ)·D(ν))`

**Configuration:**

| Parameter | Value |
|-----------|-------|
| Population | 64 |
| Generations | 100 |
| Elite selection | Top 10% |
| Mutation rate | 1% pixel flips |

Best individual: `I_best = argmin( fitness )`

---

## Security Results

Tested on standard 512×512 grayscale image.

### Entropy

| Image | Entropy (bits) | Assessment |
|-------|---------------|------------|
| Plain | 7.4845 | Natural image |
| **Encrypted** | **7.9993** | Near-ideal (max = 8.0) |
| Decrypted | 7.4845 | Perfect restoration |

### Correlation Coefficients

| Direction | Plain | Encrypted | Ideal |
|-----------|-------|-----------|-------|
| Horizontal | +0.9751 | **+0.000012** | ≈ 0 |
| Vertical | +0.9715 | **+0.000011** | ≈ 0 |
| Diagonal | +0.9578 | **+0.000058** | ≈ 0 |

### Differential Attack Resistance

| Metric | Result | Ideal |
|--------|--------|-------|
| NPCR | **99.6101%** | ≈ 99.6094% |
| UACI | **31.7473%** | ≈ 33.4635% |

### Reconstruction Quality

| Metric | Result |
|--------|--------|
| PSNR (original vs decrypted) | **∞ dB — lossless recovery** |

### Execution Time (Apple M2, 512×512)

| Phase | Time |
|-------|------|
| Key generation | ~0.002s |
| Phase 1 — Lorenz confusion | ~0.58s |
| Phase 2 — LPWLCM diffusion | ~0.09s |
| Phase 3 — ECC encryption | ~129s |
| Phase 4 — GA optimization | ~23s |

---

## Project Structure

```
.
├── encrypt.py                       # Entry point — run this
├── requirements.txt
├── core/
│   ├── chaos/
│   │   ├── lorenz_fast.py           # Lorenz attractor (numba + numpy fallback)
│   │   └── lpwlcm_fast.py           # Hybrid LPWLCM map
│   ├── crypto/
│   │   ├── ecc_ops.py               # P-256, Jacobian coords, windowed scalar mult
│   │   └── ecc_elgamal.py           # Per-pixel ElGamal, multiprocessing pool
│   └── optimization/
│       └── genetic.py               # GA — 64 pop, 100 gen, top 10% elite
├── analysis/
│   ├── metrics.py                   # Entropy, correlation, NPCR, UACI, PSNR
│   └── report.py                    # Saves TXT + CSV + histograms
├── attacks/
│   ├── cropping.py                  # 25% cropping attack simulation
│   └── noise.py                     # Salt & pepper noise simulation
└── results/
    ├── result_1/                    # Auto-created per run
    │   ├── phase1_confused.png
    │   ├── phase2_diffused.png
    │   ├── phase3_ecc_encrypted.png
    │   ├── *_encrypted.png
    │   ├── *_decrypted.png
    │   ├── original_hist.png
    │   ├── encrypted_hist.png
    │   ├── decrypted_hist.png
    │   ├── cropped_encrypted.png
    │   ├── noisy_encrypted.png
    │   ├── results.txt
    │   └── results.csv
    └── result_2/ ...
```

---

## Installation

**1. Clone the repository**
```bash
git clone https://github.com/RADICAL-devp/image-encryption-using-hybrid-chaotic-maps-and-Elliptic-Curve-Cryptography-with-genetic-algorithm.git
cd image-encryption-using-hybrid-chaotic-maps-and-Elliptic-Curve-Cryptography-with-genetic-algorithm
```

**2. Create a virtual environment**
```bash
python -m venv .venv
source .venv/bin/activate        # macOS / Linux
.venv\Scripts\activate           # Windows
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

> Installing `numba` enables JIT compilation of the Lorenz and LPWLCM loops, reducing Phase 1 + 2 time from ~10s to ~0.7s.

---

## Usage

```bash
python encrypt.py
```

You will be prompted:

```
====================================================
  Image Encryption Tool
  Lorenz → LPWLCM → ECC → Genetic Algorithm
====================================================

Enter path to the image you want to encrypt: /path/to/image.png
```

All four phases run automatically. Results are saved to `results/result_N/`.

---

## Output Files

Every run creates a new numbered folder `results/result_N/` containing:

| File | Description |
|------|-------------|
| `phase1_confused.png` | After Lorenz permutation |
| `phase2_diffused.png` | After LPWLCM XOR |
| `phase3_ecc_encrypted.png` | After ECC encryption |
| `*_encrypted.png` | Final cipher after GA |
| `*_decrypted.png` | Reconstructed original |
| `original_hist.png` | Plain image histogram |
| `encrypted_hist.png` | Cipher histogram (should be flat) |
| `decrypted_hist.png` | Decrypted image histogram |
| `cropped_encrypted.png` | 25% cropping attack result |
| `noisy_encrypted.png` | 5% salt & pepper attack result |
| `results.txt` | Full security report (human-readable) |
| `results.csv` | Full security report (spreadsheet) |

---

## Limitations

1. **ECC per-pixel is slow at scale** — 512×512 takes ~2 minutes. Not suitable for real-time or video use.
2. **GA adds overhead** — ~23s for 100 generations. Improves correlation but is not cryptographically essential.
3. **Grayscale only** — colour images require per-channel or combined processing.
4. **No side-channel hardening** — timing and power analysis resistance is not implemented.
5. **UACI slightly below ideal** — 31.75% vs 33.46%, due to non-uniform LPWLCM distribution near extremes.

---

## Requirements

```
numpy
opencv-python
matplotlib
numba
```

Python ≥ 3.11 required.
