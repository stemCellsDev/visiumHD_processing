# Visium HD Processing

## Introduction
Visium HD is a sequencing-based spatial transcriptomics technology that reaches single cell resolution thanks to a grid of 2 um squares. These can be binned to customize the resolution, and/or segmentation can be applied. The data consists of a H&E slide, a cytAssist image and the gene expression matrix. Data can be processed with Seurat or scanpy. 

This repository encompasses pre-processing, segmentation and downstream analysis using scanpy.

## Data

Human embryo head section of 8 wpc. A second sample restricted to the EOM and corresponding to 10 wpc is used in pseudotime analysis.

## Structure of repository

Start by generating the feature-barcode matrix using codes of *src* folder. Follow by *segmentation*. Explore how some genes of interest express on the tissue using *Markers_visualization* notebook. And then *single_cell_level*, followed by *pseudobulk_level*, followed by *megabulk_level*.  
Overall the pipeline encompasses: tissue segmentation, QC, anatomical clusters definition using known markers expression on tissue and Regions of Interest (defined using softwares like Fiji or QuPath), Transcription Factor enrichment analysis and differential expression analysis (across anatomical clusters) - both at single cell and pseudobulk levels - validation at megabulk level. 

A pseudotime analysis was also conducted with a second visium HD sample owing to the same type of tissue but from a different developmental stage: 10 wpc.

```plaintext
├── Notebooks/
│   ├── Benchmarking/
|        *Comparison of visium HD and scRNA-seq sensitivity.*
│   ├── Downstream_Analysis/
│   │   ├── Markers_Identification/
│   │   │   ├── differential_expression_analysis/
|                *Derive genes differentially expressed across the various anatomical clusters (using DESeq2, R-based. Results used in the three following folders)*
│   │   │   ├── megabulk_level/
|                *Aggregate all cells owing to a same anatomical cluster to display candidate markers expression.*
│   │   │   ├── pseudobulk_level/
|                *Aggregate single cells of anatomical clusters into pseudobulks to increase signal. Drawback: results might vary across rounds of pseudobulking as it randomly split the clusters altho they're not totally homogenous. Encompasses: known markers on expression in clusters, DEA with DESeq2, TF enrichment analysis with decoupler.*
│   │   │   ├── single_cell_level/
|               *Processing at single cell level: known markers expression on tissue, DEA using scanpy.tl_rank_gene_groups, TF enrichment analysis with decoupler.*
│   │   │   ├── Downstream_analysis_2.ipynb
|                *Code encompassing single cell and pseudobulk analysis.*
│   │   │   ├── Markers_visualization.ipynb
|                *Display any gene expression on tissue.*
│   │   └── Pseudotime/
|            *Diffusion pseudotime of scanpy between two stages visium HD samples.* 
│   ├── Segmentation/
│   │   ├── Bin2cell/
│   │   └── StarDist/
|            **Preferred to Bin2cell. Note that segmentation is now integrated to visium HD workflow, but it was not when I wrote these codes in 2025.**
|           *Includes quality control and normalization.*
│   └── sopa/
|        *Prefer the use of the other scripts from this repo. But sopa allows the concatenation of the complete workflow.*
└── src/
    *space ranger count function i.e. feature-barcode matrices generation*
```

**We worked with embryo head sample to study craniofacial muscles, but this pipeline can be adapted to any sample and organism.**

## Guidelines to work with this repository

- Start with segmentation: run notebook /Notebooks/Segmentation/StarDist/nuclei_segmentation.ipynb
- Perform Pseudotime analysis, as explained in PDF Files/cellID_poster.pdf, by running the notebook: /Notebooks/Downstream_Analysis/Pseudotime/Pseudotime_notebook.ipynb
- To focus on the 8 wpc section (whole head), and derive TFs and markers for each muscle : the analysis is done at 3 levels (as explained in file PDF Files/COMPLETE_EXPLANATIONS.pdf):
   - Perform the analysis at the single cell level: /Notebooks/Downstream_Analysis/Markers_Identification/single_cell_level/Markers_Identification_Single_Cell_Level.ipynb
   Note that this analysis is based on Differential Expression Analysis done by DESeq2 in /Notebooks/Downstream_Analysis/Markers_Identification/differential_expression_analysis/DEA_DESeq2.ipynb. I used a different notebook at it is R-based. The theory and design for the model fitting is also detailed in PDF Files/COMPLETE_EXPLANATIONS.pdf.
   - Perform the analysis at the pseudobulk level: /Notebooks/Downstream_Analysis/Markers_Identification/pseudobulk_level/Markers_Identification_Pseudobulk.ipynb 
   - Visualize results of single cell and pseudobulk analysis at the megabulk level (grouping cells owing to a same muscle): /Notebooks/Downstream_Analysis/Markers_Identification/megabulk_level/markers_on_megabulks.ipynb   

## Results Section

All analysis reports can be found in folder Results/. The file names talk for themselves and are extensively detailed in PDF Files/COMPLETE EXPLANATIONS.pdf

## Contact

Script generated by: Naomie Pont
Date: 2024-2025
Mail: naomie.pont@gmail.com
At Institut Pasteur, Paris. Stem Cells and Develoment Unit.
