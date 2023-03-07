## CCPlotR

### A small R package for visualising results from tools that predict cell-cell interactions from scRNA-seq data

This R package makes generic plots that can be used to visualise results from multiple tools such as Liana, CellPhoneDB, NATMI etc. All it requires as input is a dataframe with columns `source`, `target`, `ligand`, `receptor` and `score`. It should look something like this:

| source | target | ligand   | receptor | score |
| ------ | ------ | -------- | -------- | ----- |
| B      | CD8 T  | HLA-DQA1 | LAG3     | 7.22  |
| B      | CD8 T  | HLA-DRA  | LAG3     | 5.59  |
| CD8 T  | NK     | B2M      | KIR2DL3  | 5.52  |
| B      | CD8 T  | HLA-DQA2 | LAG3     | 5.41  |
| NK     | B      | LGALS1   | CD69     | 4.15  |
| B      | CD8 T  | ICAM3    | ITGAL    | 2.34  |

For some of the plots, there is an option to also show the expression of the ligands and receptors in each cell type. For those plots, a second dataframe is required, which holds the mean expression values for each gene in each cell type and should look something like this:

| cell_type | gene   | mean_exp |
| --------- | ------ | -------- |
| B         | ACTR2  | 0.363    |
| B         | ADA    | 0.0170   |
| B         | ADAM10 | 0.0833   |
| B         | ADAM28 | 0.487    |
| B         | ADCY7  | 0.0336   |
| B         | ADRB2  | 0.0178   |

The package comes with toy datasets (`toy_data`, `toy_exp`) which you can see for examples of input data.

--------------------

### Installation

The R package can be installed by running:

```R
devtools::install_github("Sarah145/CCPlotR")
```

### Plot types

The package contains functions for making five types of plots: `cc_heatmap`, `cc_dotplot`, `cc_network`, `cc_circos` and `cc_arrow`. Below are some examples of each plot type.

#### Heatmaps (`cc_heatmap`)

This function can generate two types of heatmaps. Option A just displays the total number of interactions between each pair of cell types and option B shows the ligands, receptors and cell types involved in each interaction as well as their score. For option B, only a small portion of top interactions are shown to avoid cluttering the plot. 

```R
library(CCPlotR)
cc_heatmap(toy_data)
cc_heatmap(toy_data, option = 'B', n_top_ints = 30)
```

<img src="https://github.com/Sarah145/CCPlotR/blob/main/plots/heatmaps.png">

#### Dotplots (`cc_dotplot`)

This function can generate two types of dotplots. Option A just displays the total number of interactions between each pair of cell types and option B shows the ligands, receptors and cell types involved in each interaction as well as their score. For option B, only a small portion of top interactions are shown to avoid cluttering the plot. 

```R
cc_dotplot(toy_data)
cc_dotplot(toy_data, option = 'B', n_top_ints = 30)
```

<img src="https://github.com/Sarah145/CCPlotR/blob/main/plots/dotplots.png">

#### Network (`cc_network`)

This function will generate a network plots where the nodes are cell types and the weight of the edges corresponds to the total number of interactions between a given pair of cell types.

```R
cc_network(toy_data)
```

<img src="https://github.com/Sarah145/CCPlotR/blob/main/plots/network.png">

#### Circos plot (`cc_circos`)

This function can generate three different types of circos plots. Option A generates a circos plot where the width of the links represents the total number of interactions between each pair of cell types. Option B generates a circos plot showing the ligands, receptors and cell types involved in the top portion of interactions. Option C expands on option B by also showing the mean expression of the ligand and receptor genes in each cell type. In options B and C, the weight of the links represents the score of the interaction.

```R
cc_circos(toy_data)
cc_circos(toy_data, option = 'B', n_top_ints = 25)
cc_circos(toy_data, option = 'C', n_top_ints = 35, exp_df = toy_exp)
```

<img src="https://github.com/Sarah145/CCPlotR/blob/main/plots/circos_plots.png">

#### Paired arrow plot (`cc_arrow`)

This function generates plots showing the interactions between a given pair of cell types. Option A just shows which ligands/receptors are interacting between a pair of cell types and option B also shows the expression of the ligand/receptor genes in each cell type. In both options, the weight of the arrow represents the score of the interaction.

```R
cc_arrow(toy_data, cell_types = c('NK', 'CD8 T'), n_top_ints = 25)
cc_arrow(toy_data, cell_types = c('NK', 'CD8 T'), option = 'B', n_top_ints = 25, exp_df = toy_exp)
```

<img src="https://github.com/Sarah145/CCPlotR/blob/main/plots/arrow_plots.png">

