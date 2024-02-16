# Mitoprofiles

Aiming to search for unique gene expression patterns characteristic of high metabolic-rate organs in cancer, an automated analysis pipeline was developed to ensure efficient, and reproducible data analysis.
Therefore, for this study, we used the R programming language (version 4.2.2), which is freely available
and particularly suited for statistical analysis and graphical data visualization. Additionally, this language
is supported by an active development community, which greatly extends its functionality.

To analyze, explore, and understand the data, the free open-source integrated development environment (IDE) RStudio® (version 2023.03.0) was used. All data analyses were undertaken using custom R
scripts, implementing additional functions to develop a pipeline suited to answer the research questions.
As such, the R analysis pipeline presented in this repository showcases the programming work developed
throughout this project. All analyses were conducted in a Linux environment (Ubuntu distribution version
22.04.2), running on a virtual machine capable of storing big data and facilitating faster analyses.


## 1.download_data.Rmd
### Used packages:
- here
- TCGAbiolinks
- SummarizedExperiment
- tidyverse
- readxl

### Input:
- URL's from public sources of data

### Output:
- RDS files with data from the 6 organs selected

### What does it do?
- Downloads transcriptomic RNA data from six different organs (brain, liver, kidney, bladder, colon, skin) from TCGA and GTEx
- Downloads the MitoCarta data


## 2.format_data.Rmd
### Used packages:
- here
- SummarizedExperiment
- tidyverse

### Input:
- RDS files created in the previous script

### Output:
- RDS files with clean and formated data and PCA results

### What does it do?
- Creates a metadata table with sample ids, cancer status, organ, and metabolic rate
- Create a list with read counts per organ from TCGA, GTEx, and both (combined dataset)
- Performing PCA analysis with count data from TCGA and GTEx to confirm that the data from two different sources is comparable


## 2.1.compare_tcga_gtex_gencode_v26_v36.Rmd
### Used packages:
- here
- tidyverse

### Input:
- GTF files with the versions 26 and 36 of the human genome and RDS file of the PCA result obtained in the previous script

### Output:
- PCA plot of the combined TCGA and GTEx data

### What does it do?
- Compare the genes ids present in Gencode v26 and Gencode v36 annotation files, since these were used for the annotation of the TCGA and GTEx read counts
- Plot and visualize the PCA results


## 3.diff_expression.Rmd
### Used packages:
- here
- tidyverse
- edgeR

### Input:
- RDS files created by script "2.format_data.Rmd" except the PCA results

### Output:
- RDS files containing the differential expression analysis results for each dataset (TCGA and GTEx)
- RDS files for the three linear models tested: with interation, only interation and no interation

### What does it do?
- Performs differential expression analysis using a design matrix, and contrast matrix, both used by the linear model defined
- Several alternative linear models, and alternative contrasts were tested, for each individual dataset (TCGA and GTEx) and the combined one


## 3.1.diff_expression_visualization_v2.Rmd
### Used packages:
- here
- tidyverse
- mitocarta

### Input:
- RDS files created on the previous script

### Output:
- Boxplots of the most extremely expressed genes for the different conditions
- PNGs files with the boxplots of the most extremely expressed genes for the different conditions
- RDS files with the coefficient tables and the top genes tables

### What does it do?
- Creates function that allows to plot the counts for extreme LFC differentially expressed genes. The plot shows multiple boxplots for each gene ID corresponding to the different organs being evaluated as well as their disease state (non_cancer or cancer)


## 3.2.diff_expression_visualization.Rmd
### Used packages:
- here
- tidyverse
- patchwork

### Input:
- RDS files created on the previous script

### Output:
- Boxplots of the most extremely expressed genes for the different conditions
- Plots of the likelihood test coefficient as well as the log fold change, and log counts per million

### What does it do?
- Allows us to visualize the coefficients obtained from the likelihood test made in the previous script in order to see their distribution in the different conditions


## 4.functional_enrichment.Rmd
### Used packages:
- here
- tidyverse
- mitocarta
- edgeR
- AnnotationDbi
- org.Hs.eg.db
- gprofiler2

### Input:
- RDS files created on the script "3.diff_expression.Rmd"

### Output:
- 

### What does it do?
- Performs functional enrichment analysis (GO categories and KEGG pathways) of the genes present in the dataset


## 5.clustering.Rmd
### Used packages:
- here
- tidyverse
- mitocarta
- edgeR
- RColorBrewer
- biclust

### Input:
- RDS file created on the script "3.diff_expression.Rmd"

### Output:
- Dendrograms of each different hierarchical clustering
- Heatmap of the hierarchical clustering
- Plots of the clusters formed using a soft fuzzy clustering

### What does it do?
- Performs hierarchical and soft fuzzy c-means clustering of the normalized read counts (CPMs)
