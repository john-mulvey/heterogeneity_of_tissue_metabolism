This repo contains the code required to reproduce in full the analysis presented in our paper: Predicting metabolic preferences through transcriptomics: a data-driven approach to align metabolic signatures with gene expression profiles.

In order to re-run our analysis, please follow these steps:

1. Clone this repository to your local machine:
```bash
git clone https://github.com/john-mulvey/heterogeneity_of_tissue_metabolism.git
cd heterogeneity_of_tissue_metabolism
```

This directory is laid out as follows:
├── data/          
├── analysis/
│   ├── 01_01_metabolomics_heatmap.Rmd
│   ├── 02_GTEx_transcriptome.Rmd
│   ├── 03_03_discriminatory_marker_selection.Rmd
├── results/        
│   ├── data/       
│   └── plots/
├── LICENCE.md
└── README.md

2. Download the data file containing the RNAseq data from the GTEx portal (https://gtexportal.org/home/downloads/adult-gtex/bulk_tissue_expression):

```bash
wget -O data/GTEx_Analysis_2017-06-05_v8_RNASeQCv1.1.9_gene_reads.gct.gz \
  https://storage.googleapis.com/adult-gtex/bulk-gex/v8/rna-seq/GTEx_Analysis_2017-06-05_v8_RNASeQCv1.1.9_gene_reads.gct.gz
gunzip data/GTEx_Analysis_2017-06-05_v8_RNASeQCv1.1.9_gene_reads.gct.gz
```

Verify that the following file is present in the `data` directory:
`data/GTEx_Analysis_2017-06-05_v8_RNASeQCv1.1.9_gene_reads.gct`

3. Proceed running the numbered .Rmd files in order. A rendered .html version of each file and any output is also provided.