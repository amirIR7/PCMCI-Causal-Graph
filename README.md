# PCMCI Causal Graph

This repository contains a PCMCI-based causal-discovery workflow for analysing lagged causal relationships among wildfire activity, atmospheric circulation, land-surface conditions, and other environmental variables.

The workflow uses the Tigramite implementation of PCMCI with robust partial correlation, predefined link assumptions, lagged-dependency diagnostics, and causal-graph visualisation.

## Related Publication

The analysis and causal graph in this repository are associated with the following publication:

- **IEEE Xplore document:** 11016109
- **DOI:** [10.1109/JSTARS.2025.3573263](https://doi.org/10.1109/JSTARS.2025.3573263)
- **IEEE Xplore:** [View the publication](https://ieeexplore.ieee.org/document/11016109)

When using this repository, please cite the associated article using the official citation information provided by IEEE Xplore.

## Repository Structure

```text
PCMCI-Causal-Graph/
├── Code/
│   └── pcmci_causal_graph.ipynb
├── Dataset/
│   ├── PCMIC_India_20112021_withNAO_withV.csv
│   └── README.md
├── Output/
│   ├── Figures/
│   │   └── pcmci_SEA_JSTAR.png
│   └── Results/
│       ├── pcmci_val_matrix.npy
│       ├── pcmci_p_matrix.npy
│       └── pcmci_graph.npy
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Main Features

- Reads multivariate environmental time-series data from a CSV file
- Converts the input data into a Tigramite dataframe
- Displays time-series plots and lagged-correlation diagnostics
- Uses PCMCI for lagged causal discovery
- Uses `RobustParCorr` as the conditional-independence test
- Supports predefined directed-link assumptions
- Supports exclusion of selected candidate links
- Produces a directed causal graph
- Visualises positive and negative MCI values
- Saves a publication-quality causal-graph figure
- Exports PCMCI value, p-value, and graph matrices

## Input Dataset

The main input file is:

```text
Dataset/PCMIC_India_20112021_withNAO_withV.csv
```

The CSV file must contain:

- A `date` column
- One column for each environmental or wildfire variable
- Numerical values suitable for PCMCI analysis

The `date` column is converted into a Pandas datetime index.

The workflow expects at least nine variables because the publication configuration uses variable indices from `0` to `8`.

## Variable Labels

Selected input variables are renamed for graph visualisation:

| Original variable | Display label |
|---|---|
| `v` | V₃₀₀ |
| `Z_an` | ΔZ₅₀₀ |
| `VPD_an` | ΔVPD |
| `SKT_an` | ΔSKT |
| `TP_an` | ΔTP |
| `SM_an` | ΔSM |

Other variables retain the names provided in the input CSV file.

## PCMCI Configuration

The main causal-discovery analysis uses:

```text
tau_min = 1
tau_max = 10
pc_alpha = None
alpha_level = 0.01
conditional-independence test = RobustParCorr
```

The workflow also uses predefined link assumptions to reproduce the causal structure examined in the associated publication.

## Link Assumptions

The notebook contains two types of prior assumptions:

1. **Excluded candidate links**

   Selected links are removed from the set of candidate causal relationships.

2. **Forced directed links**

   Selected source-to-target relationships are defined as directed links at specific time lags.

These assumptions are explicitly listed in the notebook so that the publication configuration can be inspected and reproduced.

## Manual MCI-Value Overrides

The notebook includes selected manually assigned MCI values used to reproduce the causal-graph configuration presented in the publication.

These values overwrite selected entries in the PCMCI `val_matrix` after the PCMCI calculation.

This section is clearly separated in the notebook from the original PCMCI output.

For a fully data-driven analysis, users should remove or disable the manual MCI-value overrides and analyse the unmodified PCMCI results.

## Installation

Clone the repository:

```bash
git clone https://github.com/amirIR7/PCMCI-Causal-Graph.git
cd PCMCI-Causal-Graph
```

Install the required Python packages:

```bash
pip install -r requirements.txt
```

The main dependencies are:

- NumPy
- Pandas
- Matplotlib
- Seaborn
- Tigramite
- NetworkX
- Jupyter Notebook

## Usage

Enter the `Code` directory:

```bash
cd Code
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open:

```text
pcmci_causal_graph.ipynb
```

Run all notebook cells in sequence.

The notebook automatically reads the dataset from:

```text
../Dataset/PCMIC_India_20112021_withNAO_withV.csv
```

## Diagnostic Plots

Before running the final PCMCI analysis, the notebook produces diagnostic visualisations including:

- Multivariate time-series plots
- Pairwise scatter plots
- Lagged-dependency plots
- Partial-correlation functions for lags up to `tau_max = 10`

These plots help evaluate the temporal relationships among the input variables.

## Causal Graph Output

The generated causal graph is saved as:

```text
Output/Figures/pcmci_SEA_JSTAR.png
```

The graph includes:

- Directed lagged causal links
- MCI-based link colours
- Auto-MCI node colours
- Curved arrows
- Link and node colour bars
- Publication-quality figure resolution

## Numerical Outputs

The workflow saves the numerical PCMCI results in:

```text
Output/Results/pcmci_val_matrix.npy
Output/Results/pcmci_p_matrix.npy
Output/Results/pcmci_graph.npy
```

### Output descriptions

| File | Description |
|---|---|
| `pcmci_val_matrix.npy` | PCMCI test-statistic or MCI-value matrix |
| `pcmci_p_matrix.npy` | PCMCI p-value matrix |
| `pcmci_graph.npy` | PCMCI graph structure |

These files can be loaded using NumPy:

```python
import numpy as np

val_matrix = np.load(
    "Output/Results/pcmci_val_matrix.npy",
    allow_pickle=True
)

p_matrix = np.load(
    "Output/Results/pcmci_p_matrix.npy",
    allow_pickle=True
)

graph = np.load(
    "Output/Results/pcmci_graph.npy",
    allow_pickle=True
)
```

## Reproducibility Notes

To reproduce the published graph:

1. Use the same input CSV file.
2. Preserve the original variable order.
3. Use the same PCMCI parameters.
4. Keep the predefined excluded and forced links.
5. Keep the manual MCI-value overrides.
6. Use compatible versions of Tigramite and its dependencies.

Changing the variable order may change the meaning of the source and target indices used in the link assumptions.

For improved reproducibility, users may create an environment-specific package list using:

```bash
pip freeze > requirements-lock.txt
```

## Dataset Considerations

Before redistributing the input CSV file, confirm that the original source datasets and licences permit public redistribution.

The repository licence applies to the source code and does not automatically grant redistribution rights for third-party datasets.

When dataset redistribution is restricted, provide:

- Data-source references
- Download instructions
- Preprocessing instructions
- A small example dataset, where permitted

## Licence

The source code in this repository is released under the MIT License unless otherwise stated.

See the `LICENSE` file for details.

The licence applies to the repository code and does not automatically apply to external datasets or third-party software.

## Citation

When using this repository, workflow, or causal graph, please cite the associated publication:

```bibtex
@article{irawan2025pcmci,
  author  = {Irawan, Amir Mustofa and others},
  title   = {See the official article title on IEEE Xplore},
  journal = {IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing},
  year    = {2025},
  doi     = {10.1109/JSTARS.2025.3573263}
}
```

Please replace the temporary title and author list above with the complete official citation exported from IEEE Xplore.

## Author

**Amir Mustofa Irawan**

Assistant Professor, Department of Climatology  
Indonesia State College of Meteorology, Climatology, and Geophysics  
Indonesian Agency for Meteorology, Climatology, and Geophysics
CommSensLab, Department of Signal Theory and Communications, Universitat Politècnica de Catalunya, 08034 Barcelona, Spain
