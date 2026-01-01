Pipeline Overview
Structural connectomes and synthetic data generation

Prior to applying Dynamic Mode Decomposition (DMD), structural brain connectivity matrices were loaded from atlas-based diffusion MRI tractography for three parcellations (DK68, Schaefer100, Schaefer200). Each connectome was represented as a weighted, undirected graph and characterized using standard graph-theoretic measures, including density, degree and strength distributions, Laplacian spectra, and community structure via Louvain modularity.

Based on the empirical DK68 connectome, two classes of synthetic region-wise time series were generated:

Linear diffusion dynamics, defined by a graph Laplacian–driven process with additive noise, producing high-dimensional, dissipative activity.

Nonlinear Hopf oscillator dynamics, in which each region is modeled as a Hopf oscillator coupled through the structural connectivity, yielding coherent oscillatory activity.

For Hopf simulations, an initial transient period was discarded to retain steady-state dynamics. All synthetic time series were stored in atlas-parcellated form (regions × time) under data/processed/ and used as controlled benchmarks for subsequent analysis.

Koopman analysis with DMD and Hankel-DMD

DMD was applied to preprocessed regional time series (N regions × T time points). For Hopf dynamics, signals were mean-centered and optionally delay-embedded using Hankel-DMD to better capture oscillatory structure. DMD yields complex eigenvalues and temporal coordinates associated with Koopman modes.

For diffusion dynamics, the DMD spectrum is broad and dissipative, reflecting high-dimensional stabilization. In contrast, Hopf dynamics consistently exhibit a small number of dominant modes with eigenvalues near the unit circle, indicating low-dimensional oscillatory structure that is invariant across parcellations.

The real and imaginary parts of selected DMD temporal coordinates are saved as low-dimensional latent state variables and used as inputs for sparse equation discovery.

Sparse Identification of Nonlinear Dynamics (SINDy)

Sparse Identification of Nonlinear Dynamics (SINDy) is applied to the low-dimensional latent coordinates obtained from DMD/Hankel-DMD. Using a low-order polynomial library and sparse regression, SINDy recovers interpretable ordinary differential equations governing the latent dynamics. For Hopf-derived latents, the recovered equations are consistent with normal-form oscillatory dynamics, validating the DMD→SINDy pipeline on synthetic data.

Network-constrained SINDy

To recover region-level dynamics constrained by anatomy, SINDy is extended to a network-constrained formulation, where each region’s time derivative is modeled as a combination of local nonlinear terms and coupling through the empirical structural connectivity matrix. Both global delays and edge-specific delays (derived from tract lengths) are supported.

This formulation enables direct comparison between learned functional coupling and the underlying structural connectome, and allows systematic evaluation of scalability across DK68, Schaefer100, and Schaefer200 parcellations.

Application to real fMRI

The validated pipeline is finally applied to real resting-state fMRI data (DK68, 48 subjects). Regional time series are z-scored per subject, temporally smoothed, and differentiated prior to Network-SINDy fitting. Ablation analyses (local vs network vs full models) and per-subject fits are used to assess robustness and interpretability.

On real data, the network-driven term explains the majority of predictive structure, with a consistent global delay of approximately 2–3 TRs across subjects, while local nonlinear terms provide limited additional explanatory power.
