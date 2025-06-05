# Single-Cell RNA Sequencing

### Dataset location

Folder: REDACTED

Annotated & curated dataset:
  * `v4_11integrated_cleaned.h5ad` 
  * `v4_11integrated_cleaned_small.h5ad` is a random subsample of 8 samples from above
  * `v4_11integrated_cleaned.h5seurat` Seurat version of above
  * `sc-meta.csv` cell metadata of above

Raw counts & unfiltered dataset:
  * `v1_01merged_cleaned.h5ad` raw counts & unfiltered dataset
  * `v1_01merged_cleaned.h5seurat` Seurat version of above

Misc:
  * `scvi-model-v3.model` scVI model for final round of integration of `v4_11integrated_cleaned` (see below)
  * `v4_11integrated_cleaned-markers.csv` markers used for annotation

The integrated dataset in H5AD format contains all available SCRIPT BAL samples as of Oct 2022 with few additional bronchial brushing and biopsy samples. The dataset integrated only filtered_feature_matrix for each sample with low-quality cells and doublets removed. A separate H5AD file with all cells form filtered_feature_matrix is available. Same two files are available in h5seurat format for loading into Seurat via SeuratDisk package.

`v4_11integrated_cleaned.h5ad` has 1940832 cells and 26626 genes.
  * `adata.X` contains log(normalized + 1) counts, normalization to 10000 counts per each cell
  * `adata.raw.X` is the same
  * `adata.layers['counts']` contains raw integer UMI counts
  * `adata.var.highly_variable` indicates 3000 highly-variable genes (see below)

Total number of patients: 171, samples: 266 (260 BAL samples), 301 single-cell libraries.

`v1_01merged_cleaned.h5ad` has 2562237 cells
  * `adata.X` contains raw integer UMI counts
  * `adata.obs_names` corresponds to `adata.obs_names` in `v4_11integrated_cleaned.h5ad` to join cells types & clusters
  * has ~600k cells that were removed during our annotation process for `v4_11integrated_cleaned.h5ad`

Total number of patients: 174, samples: 269 (263 BAL samples), 304 single-cell libraries.

### Data dictionary for the single cell related columns from 11integrated.h5ad 

| Column name                | data type | description                                                                                                                                                                                                                                                                                                                                                                                          |
| -------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| scanpy_index               | string    | These are the unique identifiers (barcodes) that are attached to each cell during sequencing library preparation. When aggregating samples, scanpy will append numbers at the end to ensure that there are unique barcodes across samples. We have the anonymized library ID tagged at the beginning as well. The true barcode that was used for sequencing is just the nucleotide sequence. |
| library_id                 | string    | library preparation ID for example _Library_9T_4911_. **NB:** several single-cell libraries could be prepared from the same BAL sample. |
| sample_id | string | Sample ID, corresponds to `sample_id` in [Clinical data](Clinical-Metadata.md) for BAL samples. **NB:** several single-cell libraries were prepared from some samples. |
| sample_type | string | `BAL` for BAL samples, `BR` for bronchial brushing samples, `PB` for postmortem biopsy samples. |
| patient_id | string | Patient ID, corresponds to `Patient_id` in [Clinical data](Clinical-Metadata.md) |
| chemistry                  | string    | Type of chemistry used for preparing the sample info single-cell RNA-sequencing library  |
| protocol | string | Single-cell library preparation protocol: `fresh_to_sc` for sample→flow cytometry→single-cell library or `cryo_to_sc` for sample→flow cytometry→cryopreservation→thawing→single-cell library. Can be `NA` (in h5seurat: `'NA'`). |
| sequencing_depth | int | Mean number of reads per cell. See [10x cellranger count metrics](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/gex-metrics). Can be `NA` (in h5seurat: `0`). **NB:** this is approximate information, not from the exact cellranger run used for integration.  |
| sequencing_saturation | percent | Fraction of reads originating from an already-observed UMI. See [10x cellranger count metrics](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/gex-metrics). Can be `NA` (in h5seurat: `0`). **NB:** this is approximate information, not from the exact cellranger run used for integration. |
| fraction_reads_in_cells | percent | The fraction of cell-barcoded, confidently mapped reads. See [10x cellranger count metrics](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/gex-metrics). Can be `NA` (in h5seurat: `0`). **NB:** this is approximate information, not from the exact cellranger run used for integration. |
| index                      | string    | the index used for demultiplexing raw reads. Each sample has a unique index such that, when demultiplexing raw data from a sequencing run containing many samples, you can assign reads to each sample based on the index. Can be `NA` (in h5seurat: `'NA'`).                                                                                                                                                                |
| batch                      | int64     | This is the same as library ID, just in a numerical encoding. Scanpy by default creates this column when aggregating samples.                                                                                                                                                                                                                                                                        |
| n_genes_by_counts          | int64     | Number of genes expressed a cell                                                                                                                                                                                                                                                                                                                                                                           |
| total_counts               | float64   | Total UMI counts.                                                                                                                                                                                                                                                                                                                                                                                    |
| pct_counts_in_top_10_genes | float64   | Percentage of cell UMIs coming from top 10 most expressed genes in a cell                                                                                                                                                                                                                                                                                                                                                                                                 |
| pct_counts_in_top_20_genes | float64   | Percentage of cell UMIs coming from top 20 most expressed genes in a cell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| total_counts_mito          | float64   | Number of mitochondrial UMIs in a cell                                                                                                                                                                                                                                                                                                                                                     |
| pct_counts_mito            | float64   | Percentage of mitochondrial UMIs in a cell                                                                                                                                                                                                                                                                                              |
| total_counts_ribo          | float64   | Number of ribosomal UMIs in a cell                                                                                                                                                                                                                                                                                                                                                       |
| pct_counts_ribo            | float64   | Percentage of ribosomal UMIs in a cell                                                                                                                                                                                                                                                                                                    |
| n_counts                   | float64   | Same as total_counts                                                                                                                                                                                                                                                                                                                                                                                 |
| cell_type                  | object    | Manually annotated cell types (Misharin, Markov)                                                                                                                                                                                                                                                                                                                                                                                |
| leiden                     | int64     | Clustering results by Leiden algorithm                                                                                                                                                                                                                                                                                                                                                               |
                             

NOTE: If you've never worked with this data type before, we've included some links at the bottom of this description.

1. [AnnData](https://anndata.readthedocs.io/en/latest/generated/anndata.AnnData.html)

## Loading data in Seurat
We have tested the following loading in the `r-hackathon` environment
```
## Reading raw counts data
obj <- LoadH5Seurat(
  "v4_11integrated_cleaned_small.h5seurat", 
  assays = list("counts" = "counts"), 
  verbose = TRUE
)
GetAssayData(obj, "counts")["MALAT1", 1:10]

## Reading scaled data (log-normalized)
obj <- LoadH5Seurat(
  "v4_11integrated_cleaned_small.h5seurat", 
  assays = "RNA", 
  verbose = TRUE
)
GetAssayData(obj, "counts")["MALAT1", 1:10]
```

## Single-cell library preparation
Single-cell RNA-seq was performed using Chromium Next GEM Single Cell 5’ reagents V1 or V2 (10x Genomics, protocol CG000331 Rev A) according to the `chemistry` metadata. 
Cells were loaded on 10x Genomics Chip A or K after flow cytometry directly, or immediately after retrieving from -80°C freezer, rapidly thawing in water bath at 37°C and gently mixing by pipetting, according to the `protocol` metadata. Cell we loaded with Chromium Single Cell 5’ gel beads and reagents, and added to RT mix. 
The volume of the single cell suspension was calculated using protocol CG000331 Rev A (10x Genomics) based on concentration at the time of cryopreservation (for cryopreserved samples) and aiming to capture 5000–10000 cells per library. 
Libraries were prepared according to the manufacturer’s protocol (10x Genomics, CG000331 Rev A). After quality checks, single-cell RNA-seq libraries were pooled and sequenced on a NovaSeq 6000, HiSeq 4000 or NextSeq 2000 instruments.
A fraction of samples were split in multiple aliquots for preparation of multiple libraries as technical replicates. They could have been prepared using different protocols.

## Single-cell data processing
After sequencing, reads were demultiplexed with `bcl2fastq` run via `cellranger mkfastq` command into reads corresponding to specific libraries. Each library's reads were processed with `cellranger count` using cellranger 7 in `exon-only` mode and aligned to combined GRCh38.93 and SARS-CoV-2 (NC_045512.2) genomes. We used cellranger built-in algorithm for calling cells from empty barcodes.

Downstream processing was done in python with `scanpy` package. After loading each count matrix separately, they were joined with the `concatenate` function with `outer` join strategy. Cells with less than 200 counts and genes expressed in less than 50 cells were removed. Mitochondrial genes were defined by `MT-` prefix, and ribosomal by `RPS` or `RPL` prefixes.

3000 highly-variable genes were selected with [Pearson residuals](https://scanpy.readthedocs.io/en/stable/generated/scanpy.experimental.pp.highly_variable_genes.html) method and `sample_id` as batch covariate.
Next, we trained scVI model with `n_latent=20, n_layers=2, gene_likelihood="nb"` parameters for 400 epochs and obtained latent representations for all cells. We constructed kNN-graph on these representations and performed leiden clustering with default parameters, and calculated table of marker genes for each cluster. Clusters with low UMI counts, high % mitochondrial counts and mitochondrial genes at the top of marker gene list were excluded.

To remove low-quality cells, we repeated this process for 2 more times starting with recomputing highly-variable genes. ScVI model from the third (last) iteration of this process is provided. Finally, we annotated clusters based on their corresponding marker genes manually, and performed additional clustering for 2 clusters that had dendritic cells to resolve those populations.
