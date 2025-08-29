# Heat-Kernel Multi-View Learning — MATLAB implementation

Short description

- Heat-Kernel Multi-View Learning implements the heat‑kernel enhanced multi‑view clustering algorithms described in the manuscript. It includes:
  - E‑MVFCM: view‑weighted kernel fuzzy c‑means with heat‑kernel coefficients
  - EB‑MVFCM: bi‑level extension that also learns per‑feature weights
  - MCMC wrapper (Metropolis–Hastings) for joint inference over (m, α, c)
  - GPU-aware and vectorized implementations for faster inner solves

Status

- Code will be publicly released on GitHub upon acceptance of the manuscript.
- Current folder: `access-initial-submission/` (main demo scripts and figures).

Requirements

- MATLAB R2024b or newer (tested); earlier versions may work but may require minor edits.
- Recommended toolboxes (optional but speed/utility improvements):
  - Parallel Computing Toolbox (for `parfor`, `gpuArray`)
  - Statistics and Machine Learning Toolbox (kmeans)
  - Signal Processing / Image Processing (only for optional demo preprocessing)
- Hardware: CPU is sufficient. GPU support implemented (NVIDIA + Parallel Computing Toolbox) — optional.

Quickstart (local MATLAB)

1. Add repository to MATLAB path, or cd into repository root:
   - In MATLAB: addpath(genpath('E:\EB-MVFCM\access-initial-submission'))
2. Prepare data (examples expect `breast.mat` or synthetic demo scripts).
3. Run demo (example):
   - From MATLAB command window:
     ```matlab
     % run EB-MVFCM demo (auto-detect GPU)
     run('E:\EB-MVFCM\access-initial-submission\BIC_main_EB_MVFCM.m');
     ```
   - Or from PowerShell (headless):
     ```powershell
     matlab -batch "run('E:\EB-MVFCM\access-initial-submission\BIC_main_EB_MVFCM.m')"
     ```

Recommended example `inPara` (MATLAB snippet)

```matlab
% minimal inPara example used by BIC_main_EB_MVFCM.m
inPara.m = 1.2;        % fuzzifier
inPara.alpha = 5;      % view exponent
inPara.beta = 2;       % feature exponent (EB-MVFCM)
inPara.cluster_n = 4;  % initial number of clusters to try
inPara.maxIter = 200;  % inner E-MVFCM iterations
inPara.thresh = 1e-6;  % convergence threshold
% enable GPU if available (script auto-detects if not set)
% inPara.use_gpu = true;
```

Main scripts

- `BIC_main_EB_MVFCM.m` — demo / driver for EB‑MVFCM (TCGA-BRCA demo included)
- `custom_MCMC_BIC_v4.m` — MCMC wrapper and grid-search utilities (parameter selection)
- `E_MVFCM.m` — core E‑MVFCM implementation (vectorized / gpu branches)
- `EB_MVFCM.m` — bi-level algorithm (GPU-first implementation with CPU fallback)

Outputs and logging

- Scripts save results (memberships, centers, V/W weights, objective history) into timestamped `.mat` files in the working directory.
- Checkpointing enabled for long MCMC runs; figures exported as PNG/EPS where configured.

Reproducibility notes

- Fix RNG for deterministic runs: `rng(seed)` at top of scripts.
- Record MATLAB version, toolbox list: `ver` and `gpuDevice` (if used); include these in any results repository release.
- Recommended pipeline: coarse grid (cheap inner iters) → refine best region → final full-precision run → (optional) MCMC around region of interest.

Performance tips

- Use vectorized E_MVFCM (already implemented) for large n and D.
- Enable GPU (`use_gpu = true`) only if `gpuDeviceCount>0` and GPU memory is sufficient; prefer `single` precision on very high‑dim data to reduce memory.
- Use `parfor` to parallelize independent parameter combos (requires Parallel Toolbox).

Citation and license

- Please cite the manuscript when using these implementations. BibTeX citation (example):
  ```
  @article{sinaga2025heatkernel,
    author = {K. P. Sinaga and S. Colantonio},
    title  = {Heat-Kernel Multi-View Learning},
    year   = {202x},
    note   = {manuscript / xxxx -- code release at acceptance}
  }
  ```

Contributing & contact

- Contributions welcome via GitHub pull requests after public release.
- For questions, bug reports or to request datasets/figures, contact: kristinapestaria.sinaga@isti.cnr.it



