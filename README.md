```markdown
# A Comprehensive Benchmark for Evaluation Inference Strategies on Large Scale Tabular Datasets

This repository evaluates three distinct machine learning inference strategies—serial, shared-memory parallel (Joblib), and distributed-memory (MPI)—to address the scalability challenges of processing massive tabular datasets[cite: 6, 7]. The project provides concrete guidance on selecting appropriate inference backends based on system architecture, operational context, and dataset size[cite: 6, 12]. 

For the complete academic methodology, literature review, and extended discussion, refer to the comprehensive dissertation document provided in `HPC_ML.pdf`[cite: 1, 3].

## Dataset and Model Setup
*   **Dataset:** The project utilizes the HIGGS dataset, acting as a representative benchmark for high-volume scientific data[cite: 6]. The original dataset contains over 11 million simulated particle collision events with 28 real-valued kinematic features[cite: 7, 10].
*   **Data Preparation:** A stratified subset of 786,341 events was extracted using probabilistic row skipping to fit in-memory processing constraints while maintaining class label proportions[cite: 10]. All features were standardized to zero mean and unit variance using z-score normalization[cite: 10].
*   **Model:** A Random Forest classifier consisting of 100 decision trees was trained using Scikit-learn[cite: 10].
*   **Validation Split:** The training utilized an 80/20 train-test split, resulting in a holdout test set of 157,269 samples[cite: 10].
*   **Production Simulation:** The inference benchmarks were executed over the remaining 8,656,137 rows to accurately simulate a massive, real-world deployment scenario[cite: 10].

## Evaluated Inference Strategies
1.  **Serial Inference:** Executed in a single-threaded environment to serve as a baseline for latency and throughput[cite: 10]. This method benefits from predictable memory access patterns and no thread management overhead[cite: 12].
2.  **Parallel Inference (Joblib):** Utilizes shared-memory parallelism by dividing the dataset across 8 logical CPU cores on a single host[cite: 10]. Each core independently performs inference on data chunks[cite: 10].
3.  **MPI-based Distributed Inference:** Uses `mpi4py` to orchestrate 4 distributed processes (ranks)[cite: 10]. The data is partitioned in a round-robin distribution, and predictions are returned to the primary rank for consolidation[cite: 10]. 

## Key Results and Performance
The Random Forest classifier achieved robust predictive metrics on the high-energy physics data, yielding **73.30% accuracy** and an **81.25% ROC-AUC**[cite: 11]. 

The inference execution on the 8.65 million row dataset yielded the following performance benchmarks:
*   **Serial Execution:** Completed in **139.86 seconds** (~61,918 rows/second)[cite: 11]. This was the fastest method, excelling due to minimal coordination overhead and consistent cache locality[cite: 11].
*   **Joblib (8 Cores):** Completed in **231.98 seconds** (~37,292 rows/second)[cite: 11]. The expected linear speedup was throttled by inter-core communication, shared cache contention, and Python's Global Interpreter Lock (GIL)[cite: 11].
*   **MPI (4 Ranks):** Completed in **584.52 seconds** (~14,807 rows/second)[cite: 11]. Despite its theoretical scalability in HPC clusters, the MPI strategy suffered from severe overhead due to message-passing latency, process synchronization, and redundant full-model loading in each rank[cite: 11, 12].

## Evaluation Metrics
The system's efficiency and accuracy were evaluated using the following core formulas[cite: 14]:
*   **Accuracy:** (TP + TN) / (TP + TN + FP + FN)[cite: 14]
*   **Throughput (Q):** N / T (where N is dataset size and T is total runtime)[cite: 14]
*   **Speedup (S):** T1 / Tp (where T1 is single-processor runtime and Tp is multi-processor runtime)[cite: 14]
*   **Parallel Efficiency (E):** S / p = T1 / (p * Tp)[cite: 14]
*   **Overhead (O):** Tp - (T1 / p)[cite: 14]

## Future Work
Future iterations of this research will explore model interpretability, computational cost optimizations, and sensitivity to input features to better determine the most suitable models for varying forecasting tasks[cite: 14].

## Author and Acknowledgements
*   **Author:** SSR Subramanya Hemant Konduri (B.Tech., Computer Science and Engineering, SRM Institute of Science and Technology)[cite: 1, 4]
*   **Supervision:** Dr. Kalyan Netti, Chief Scientist, IT Department, CSIR-NGRI, Hyderabad[cite: 1, 3]
*   **Acknowledgements:** Special thanks to Dr. Prakash Kumar (Director, N.G.R.I.) and the IT department members for their support, facilities, and guidance[cite: 4].

```
