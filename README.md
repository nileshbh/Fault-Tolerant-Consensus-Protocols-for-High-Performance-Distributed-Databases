# Fault-Tolerant-Consensus-Protocols-for-High-Performance-Distributed-Databases
# Fault-Tolerant Consensus Protocols for High-Performance Distributed Databases

This repository contains a **trace-driven experimental notebook** to evaluate and compare fault-tolerant consensus protocols used in distributed databases:

- **Paxos**
- **Raft**
- **PBFT**
- **HotStuff**

The experiment pipeline ingests workload/resource traces, derives a transaction arrival series, injects failures, and measures protocol behavior across scenarios.

## Repository Structure

- `fault_new (2).ipynb` — end-to-end experiment notebook (data loading, preprocessing, protocol simulation/evaluation, statistical tests, plots, and exports)
- `README.md` — project overview and usage guide

## What the Notebook Does

The notebook implements a full workflow:

1. **Load workload trace CSV** (Colab upload or local fallback)
2. **Align schema** for required fields such as:
   - `creation_time`
   - `deletion_time`
   - `cpu_request`
   - `memory_request`
   - optional `gpu_request`, `disk_request`
3. **Bucketize trace into time series** (default: 60-second buckets)
4. **Map resources to transaction arrivals** using configurable coefficients
5. **Model consensus protocols** (CFT/BFT abstractions)
6. **Inject faults** (crash, Byzantine, partition)
7. **Run multi-trial experiments** (default: `k=5`)
8. **Compute metrics** such as throughput, p50/p95 latency, and availability
9. **Run statistical analysis** (Mann-Whitney U test, Cohen’s d)
10. **Generate sensitivity analysis and figures**
11. **Export CSV/JSON outputs**

## Experimental Scenarios

The notebook evaluates multiple system conditions:

- `baseline`
- `peak`
- `failure`
- `recovery`

Fault schedule codes used in the experiment:

- `0` = no fault
- `1` = crash fault
- `2` = Byzantine fault
- `3` = network partition

## Key Configuration Parameters

Some important defaults inside the notebook:

- `BUCKET_SEC = 60`
- `K_TRIALS = 5`
- `n_replicas = 7`

Resource-to-transaction mapping coefficients:

- `alpha_cpu = 1.0`
- `alpha_gpu = 0.5`
- `alpha_mem = 0.2`
- `alpha_disk = 0.1`
- `beta_scale = 0.05`

## Requirements

Use Python 3 with common scientific packages:

- `numpy`
- `pandas`
- `scipy`
- `matplotlib`

If running in **Google Colab**, the notebook supports interactive CSV upload via `google.colab.files.upload()`.

## How to Run

### Option A: Google Colab

1. Open `fault_new (2).ipynb` in Colab.
2. Run all cells.
3. Upload your trace CSV when prompted (e.g., `disaggregated_DLRM_trace.csv`).
4. Review generated tables/plots and exported artifacts.

### Option B: Local Jupyter

1. Install dependencies.
2. Place your dataset at:
   - `/mnt/data/disaggregated_DLRM_trace.csv`
   - or update the dataset path in the notebook.
3. Run the notebook end-to-end.

## Output Artifacts

By default, outputs are written to:

- `/content/outputs/raw_trial_results.csv`
- `/content/outputs/experiment_summary_mean_sd.csv`
- `/content/outputs/stat_tests_raft_baseline_latency.csv`
- `/content/outputs/sensitivity_mapping.csv`
- `/content/outputs/consensus_results_summary_mean_sd.csv`
- `/content/outputs/experiment_config.json`
- `/content/outputs/figures/throughput_mean_sd.png`
- `/content/outputs/figures/latency_boxplot_baseline.png`
- `/content/outputs/figures/p95_latency_by_scenario.png`

## Notes

- The notebook can optionally call an external real testbed function (if available) such as `run_experiment(...)`; otherwise it falls back to an internal simulator.
- If your CSV uses different column names, update the schema mapping section in the notebook.
- This repository is currently notebook-centric (100% Jupyter Notebook).

## Citation / Reproducibility

If this repository supports a paper/report, include:

- exact trace dataset version
- notebook commit hash
- experiment configuration (`experiment_config.json`)
- number of trials and statistical test settings
