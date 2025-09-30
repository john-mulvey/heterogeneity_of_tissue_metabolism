---
date: 2022-12-05
project_id:
tags:
status:
lab: ploeg_lab
---
Tags:

## Project summary


## Activity overview
```ActivityHistory
project folder here (and added into activity plugin tracking)
```

## Lab book

Sunday, December 4th 2022
## Notes
Re wrote some of the analysis code to make it more interpretable what is going on -  not least because it wasn't clear to me when I was looking back at it. Done up to like ~285

Played with differential expression analysis a bit - seems like the reason why so many genes are DE between heart and kidney might jsut be because they are such different tissues. If we look between kidney cortex and medulla, there are only ~200 after filtering for corrected pval and logFC.
## To do
- [ ] Check my understanding of how edgeR is doing the normalisation, that I have correctly extracted _corrected_ read counts for (e.g. PCA and Heatmap) and that I am not doing anything I shouldn't be when calcualting TPM
	- ???Repeat using DEseq2 as a sanity check?

__________________________
# entry_date:: Sunday, December 18th 2022
## Notes
Still working on refactoring code to make it understandable what is being done and how.
- Still not sure that I have extracted properly normalised counts - certainly based upon sample-wise desnity plots, this does not seem to be the case
- Filtering genes to
	- protein coding
	- not lowly expressed
	- ???expressed in a certain proportion of samples?
- Filtering samples for
	- >10 samples per tissue
	- ???RNA integrity score
- ~~Batch correction - to use a data driven approach, selecting known covariates that correlate best with principle components
	- ~~As before this can be plotted in e.g. heatmaps, but otherwise these covariates will be directly included in the design matrix when testing for differential expression
	- ~~to use: ComBat with covariates identified from PCA or sva?
- Machine learning: select a small number of genes that best classify tissue type from the gene expression profile
	- [x] tidymodels then use a model whicch will select a very small number of features 
		- e.g. lasso regression
		- see https://rpubs.com/eR_ic/classification
		- NB that this is the same problem as for assigning markers to discriminate cell cluster in scRNAseq analysis
	- [x] repeat but with heart tissue combined
	- Check analogy to how the human protein atlas defines tissue specific/enriched genes???
- Heatmaps
	- Need to make a decision on heat maps - which one do we want in the paper, rather than doing all combinations just in case...
	- plot predefinied genes of interest, clustered only within GEM pathway groups
		- individual samples shown
		- averaged per tissue
	- [ ] change colour scheme where values not below 0
- Plot mean vs variance and mean vs CV for all genes
- GSEA on anova gene results - and then filter for metabolic terms
- [ ] protein_coding_XXX not working: check rownames in gene count matrices?
	- log_tpm_grouped_tmm_goi has rownames matched to ensembl_gene_id but ungrouped has GTEs_ENSG???
-  ~~To identify metabolic genes that are tissue specific: to use TissueEnrich R pacakge [[@jain_2019]]
	- Is this analagous to how the human protein atlas defines tissue specific/enriched genes???
- [ ] Do I need to explicitly TMM normalise _after_ calculating TPM? see HPA pipeline https://www.proteinatlas.org/about/assays+annotation#hpa_rna
- [x] Volcano plots for each tissue vs average of all other tissues - need to specify contrasts individually in order to have separate p values.
- [ ] For all plots, map heart LV and atrial appendage as a single tissue type
- [ ] change colour scheme for heatmaps showing logTPM rather than expression levels
		library(RColorBrewer)
		color = colorRampPalette(brewer.pal(n = 100, name = "YlOrRd"))(100)

__________________________
# entry_date:: Wednesday, February 15th 2023
## Notes
Meeting with Lente and Jan 20230206
- [ ] Throughout analysis, specifcy heart as a single tisuse containing both LV and AA rather than to distinguish the tissues
- [ ] Heatmap:
	- to arrange genes by metabolic pathway, and then cluster genes based upon their expression values within the pathway
- [ ] Send Lente a csv for all genes that have tissue specific expression !!!
	- [ ] Check raw counts for a few genes that have _very_ significant p-values between tissues
- [ ] Double check that mito genes have been included rather than filtered out
	- [ ] Send data for comparison of nucleor vs mitochondrial encoded mitochondrial genes
- [ ] ??? To calculate an enrichement of metabolic pathways using e.g. GSEA between tissues
	- Note underlying assumptions made about metabolic control theory by doing
- NB to ensure .svg file extension is present when exporting plots to send
- Agreed to meet again ~the end of Feb
- To query re authorship??? Have spent a lot do time doing this project, would seem reasonable if this was reflected as second/joint first authorship

__________________________
# entry_date:: Monday, July 17th 2023
I have performed additional analysis. Amongst the key differences are:
- tissues are different: I have taken just the Lv from the heart
- differential expression testing: I have performed pairwise comparisons between the tissues
- I have added calculation of enrichment of the custom metabolic pathways that were extracted from the human GEM by Marleen
- (I have added a regular GSEA for the pairwise comparisons as well)
- better plotting of the heatmap - pathways are shown as blocks, and then genes are clsutered within each pathway by their expression values

Package to share:
- heatmap of ANOVA
- csv containing results of ANOVA
- enrichment of custom metabolic pathways
- csv expression values of genes signifciant in anova, grouped per tissue
- csv all expression values, grouped per tissue

To do still...
- [ ] heatmap - label rows with gene symbol (dataframes are currently using ENSG)
	- check title alignment
- [ ] mito vs nuclear encoded genes
- [ ] Filtering for RNA Integrity Number (RIN) - to show EDA both before and after to demonstrate why this was performed
- [ ] Check numbers of tissue samples
- [ ] Design matrix: to include donor as a covariate???


__________________________
# entry_date:: Saturday, July 22nd 2023
Had meeting with Jan and Lente, action points for me were:
- [ ] write methods section for the bioinformatics parts
- [x] create a heatmap for Lente's AV metabolomics data summary
- [ ] GSEA plots
	- [x] liver vs muscle is a different width to the other plots
	- [ ] ?to create a patchwork with a single legend
- [ ] Heatmap for discriminatory markers: double check that colour legend scale is correct
- [x] add decision tree plot to google drive, so that Lente can make an equivalent (but cleaner) version for the paper

__________________________
# entry_date:: Wednesday, August 16th 2023
- [x] update AV metabolomics heatmap with
	- [x] colour scale centred on 0
	- [x] legend order the same as the order in which pathways appear

??? Is it still worthwhile doing some analysis on the [[@wang_2019]] proteomic dataset? Since it would be nice to propose markers where changes are observed across both transcript and protein

__________________________
# entry_date:: Friday, February 7th 2025
Overarching comments from the reviewers:
- make clear the motivation of the study: to attempt to define practically measured markers applicable to ex vivo (inc in vitro) experimental settings that could verify that metabolism recapitulates tissue metabolism in vivo
- more details in stats in main text
- add number of genes and metabolites


Comments for email
- short title - to include "organ specific"
- sorry to Jan re abstract
- graphical abstract 
	- remove "single cells" since we don't use any of this data
	- "complexity of metabolic preference" in last panel to be "complexity of metabolic regualtion"?
- changes to text - I've tried to make the rationale of the study more clear, since it seems we lost the reviewer from the start last time






