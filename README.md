# CLUEsteringEvaluation
Final result from analysing CLUEstering algorithm along with integration with CMSSW. CERN team from TU/e EngD program 2024


README for CLUEsteringEvaluationPipeline

Overview
--------
This project implements a pipeline for evaluating and comparing particle clustering techniques using CLUEstering, CLUE3D, and simulated (Sim) data. The pipeline analyzes particle collision events and generates insights through visualizations and metrics, focusing on clustering quality, energy, size, and efficiency.

Features
--------
- Visualization-based Evaluation: Generate scatter plots and size-energy plots for clusters.
- Score-based Evaluation: Compare clustering results with simulation data using metrics like efficiency and merge rate.
- Configurable Pipeline: Supports single-particle and multi-particle collision clustering analysis with adjustable parameters.
- Comparison Modes: Allows per-event comparison for trackster (cluster) sizes, energies, and barycenters.

Requirements
------------
- Python (3.11.9 is used for the scripts)
- Required Libraries:
  - CLUEstering
  - uproot
  - awkward 
  - seaborn
  - preprocessing 
  - zscore
  - stats
  - Other libraries (os, sys, numpy, matplotlib, pandas, etc.)

Installation
------------
1. Install the dependencies:
2. Ensure the required data files are present in the directory specified in the configuration (root_names and scaled).
    -.root files should be accessed by accessing the folder "rootfiles"
    -scaled data which can be saved using the script "CLUEstering-Parameter-optimization.ipynb" with the name "ScaledEventData". This folder should be stored in the script directory in a separate folder. we use the name of this folder as a parameter. 

Usage
-----
The pipeline can be run end-to-end by executing the main script:

Configuration
-------------
The configuration options are located in `main`. Below are the key parameters you can modify:

- `root_names`: List of root files containing collision event data.
  - Example for single-particle data: ['histo_442306_0', 'histo_442306_1']
  - Example for two-particle data: ['histo_two']
- `scaled`: Folder name of pre-processed scaled data. Set `None` for raw data.
  - Example: `'./normalize_all_z_score'`
- `paramset`: Parameters controlling the clustering.
  - Example for single-particle data: [1.8, 1, 6.5]
  - Example for two-particle data: [0.5, 4, 1]
- `merge_threshold`: Threshold for merging clusters in the score evaluation.
  - Example: `0.6`
- `efficiency_threshold`: Threshold for efficiency analysis.
  - Example: `0.5`
- `multi_particle`: Flag for multi-particle analysis.
  - Set `True` for multi-particle; `False` for single-particle.

Pipeline Description
--------------------
1. CLUEstering Processing:
   - The `particle_CLUEStering()` function runs the CLUEstering algorithm on the input data.

2. CLUE3D and Sim Processing:
   - The `particle_CLUE3D_Sim()` function reads and processes data with CLUE3D and simulation for comparison.

3. Score-based Evaluation:
   - The `clustering_evaluation_by_score(Reco)` function computes efficiency and merge rate by comparing tracksters from Reco (either CLUEstering or CLUE3D) with Sim data. The error plots are also made in this function.

4. Visualization-based Evaluation:
   - The `clustering_evaluation_by_visualization()` function generates visual comparisons (e.g., size-energy plots, barycenter scatter plots) for detailed analysis.

Outputs
-------
1. Visualizations:
   - Size-energy plots for clusters from CLUEstering, CLUE3D, and Sim.
   - Scatter plots of points with barycenters to visualize clustering quality compared to simulation data.
   - Error plots of energy, barycenter phi and barycenter eta

2. Metrics:
   - Efficiency and merge rate between reconstructed (Reco) clusters and simulation data.

3. Logs:
   - Details of processed events and results, printed to the console.

Notes
-----
- Ensure all required data files are available before running the script.
- The configuration should be tailored to the type of analysis.
- A script file named 'CLUEstering-Parameter-optimization.ipynb' is provided in the directory of this project to find the best parameter combination for CLUEstering. The output is the optimum parameter set logged in console, and the scaled data is also saved in a folder named "ScaledEventData". The user should run this script with below configurations.
  - scale_method: the string with the name of the scaling method chosen from the list below.
      - 'x2 by event min max'
        'x2 by event z-score'
        'all features by event min max'
        'all features by event z-score'
        'x0 x1 min max adjust x2'
        'x0 x1 z-score adjust x2'
        'x2 by combined x0 x1 range'
      - Example: 'all features by event z-score'. This is the best scaling method that is currently used for the two particle data and fed to CLUEstering.
  - dc_list: The list of possible dcs
      - Example: [0.2, 0.5, 0.7]
  - rhoc_list: The list of possible rhocs.
      - Example: [4, 4.5, 5]
  - dm_list: The list of possible dms.
      - Example: [0.5, 1, 2]
  - root_names, merge_threshold, efficiency_threshold, multi_particle as in 'CLUEsteringEvaluationPipeline' 

