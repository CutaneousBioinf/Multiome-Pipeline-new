# Multiome-Pipeline
This pipeline comprises 4 steps as can be seen below. The `output_dir` option in the bash code specifies the directory where the output pdf files will be stored, the `output_dir` in the config.yml file specifies where the .RDS files or .csv files generated intermediately will be stored, and the `dir_config` input argument specifies the path of config.yml

## To use the pipeline:
Run the following sample bash code for each step, please note that all the input arguments should be listed in the config.yml.

## Config file:

The example config.yml should look like this: 

```{bash}
default:
#all the input argument needed for Step1_Pre-processing
  dir_gene : '/home/yuntian/net/psoriasis/psorgenom/alextsoi/Prometheus_multiome/analysis.aggr/results/SSc_multiome_skin_newATAC/outs/filtered_feature_bc_matrix.h5'
  dir_ATAC : '/home/yuntian/net/psoriasis/psorgenom/alextsoi/Prometheus_multiome/analysis.aggr/results/SSc_multiome_skin_newATAC/outs/atac_fragments.tsv.gz'
  forceOption : 'TRUE'
  batch_data : '/home/yuntian/multiome/Prometheus_skin_newATAC/skin.input.correct.csv'
  dir_cluster : '/home/yuntian/net/psoriasis/psorgenom/alextsoi/Prometheus_multiome/analysis.aggr/results/SSc_multiome_skin_newATAC/outs/analysis/clustering/gex/graphclust/clusters.csv'
  aggr_input : '/home/yuntian/net/psoriasis/psorgenom/alextsoi/Prometheus_multiome/analysis.aggr/data/skin.aggr.meta.new.csv'
  output_dir : '/home/yuntian/multiome/Prometheus_skin_newATAC'
  
#all the input argument needed for Step2_QC
  nCount_ATAC_min : 250
  nCount_ATAC_max : 25000
  nCount_RNA_min : 250
  nCount_RNA_max : 25000
  MT : 25
  nucleosomesignal : 4
  TSS_enrichment : 2
  qc_data: 'TRUE'
  
#all the input argument needed for Step3_batch_correction
  dir_macs2 : '/home/alextsoi/Software/miniconda3/bin/macs2'
 
  
#all the input argument needed for Step4_with_cell_type_part1
  dir_celltype : '/home/yuntian/multiome/Prometheus_skin_newATAC/prometheus_skin_annotations.csv'
  adjust.cov : 'Age,Sex'
  test_option1 : 'LR'
  cell_type_order : 'Keratinized Keratinocytes,Differentiated Keratinocytes,Basal Keratinocytes,Fibroblasts,Pericytes,Melanocytes,Nerve Cells,Eccrine Cells,Myeloid Cells,Monocytes,Mast Cells,T Cells,Endothelial Cells,L Endothelial Cells,Smooth Muscle Cells,B Cells,Langerhans Cells,Follicle Cells'
  condition_order : 'Scleroderma,Control'
  
#all the input argument needed for Step4_with_cell_type_part2
  test_option2 : 'LR'
  VOI : 'Condition'
  adjust.cov2 : 'Age,Sex'
  cond1 : 'Scleroderma'
  cond2 : 'Control'


```

--dir_gene: the directory of filtered_feature_bc_matrix.h5

--dir_ATAC: the directory of fragment files, *atac_fragments.tsv.gz*

--forceOption: Should we allow very high contamination fractions to be used or not, usually when we use the clustering info from cellranger, we won't have this problem, but alwasys set it as TRUE to make sure the pipeline could run smoothly

--batch_data: sample info file, including *Sample_ID, Core_Sample_Name, Condition, Tissue, Batch, Sex, Race, Age*, if the last three demographic variables are not available, don't include them in the csv file. The *Core_Sample_Name* doesn't need to follow a particular namining rules. The **Sample_ID** (1st column) should match the **library_id** you used in the upstream CellRanger aggr run, both order and name. Please see the following picture as an example: 

*aggr input file:*

![image](https://user-images.githubusercontent.com/97702059/179427925-18d5347d-008a-48af-8d2f-6b35f33adee6.png)

*sample info file:*

![image](https://user-images.githubusercontent.com/97702059/182306601-781ff025-ae9b-4631-b3c0-47c999b86ae3.png)

Please note that we assume that after the 6th column, the rest of the columns store demographic variables we should potentially adjusted when we try to find DEGs and differentially accessible peaks. If you don't have information for the first 6th columns, please fill them with NA or unknown.

--dir_cluster: clustering info used for ambient RNA removal, *e.g. /outs/analysis/clustering/gex/graphclust/clusters.csv*

--aggr_input: the directory of the aggr run input file

--output_dir: the directory of the output files saved during the process, *e.g. output_dir='/home/yuntian/multiome/Prometheus_skin0718'*, please do not include the slash at the end

--nCount_ATAC_min : 250

--nCount_ATAC_max : 25000

--nCount_RNA_min : 250

--nCount_RNA_max : 25000

--MT : 25

--nucleosomesignal : 4

--TSS_enrichment : 2

--qc_data: whether the threshold for the QC step should be generated based data or use the default one, qc_data=TRUE means do not use the default thresholds

--dir_macs2: the directory of macs2

--dir_celltype: the directory of cell type annotation

--cell_type_order: self-defined order of cell types, in order to better view the visulization results *_e.g. 'Keratinized Keratinocytes,Differentiated Keratinocytes,Basal Keratinocytes,Fibroblasts,Pericytes,Melanocytes,Nerve Cells,Eccrine Cells,Myeloid Cells,Monocytes,Mast Cells,T Cells,Endothelial Cells,L Endothelial Cells,Smooth Muscle Cells'*. If you don't specific requirements for the order, please write *cell_type_order=''*

--condition_order: self-defined order of conditions, in order to better view the comparison results of cell type proportion historgram, especially when we have multiple conditions *_e.g. 'CTRL,AD_LES,PN_LES,AD_NONLES,PN_NONLES'*. If you don't have specific preference, please write *condition_order=''*

--adjust.cov: covariates need to be adjusted, *e.g. 'Age, Sex'*, please note those variables must be included in the sample input file, and the names should be consistent. If there is no covariate need to be adjusted, please write *adjust.cov=''*

--test_option1: denotes which test to use, *e.g. 'LR', 'poisson'*

--test_option2: denotes which test to use, *e.g. 'LR', 'poisson'*

--VOI: variable of interest, *e.g. 'Condition'*, please note this variable must be included in the sample input file, and the name should be consistent with the colname in the sample input file

--adjust.cov2: covariates need to be adjusted, *e.g. 'Age, Sex'*, please note those variables must be included in the sample input file, and the names should be consistent. If there is no covariate need to be adjusted, please write *adjust.cov=''*

--cond1: comparison condition 1, should be the same as the word using in sample info file, *e.g. CTRL*

--cond2: comparison condition 2



## Step 1: Pre-processing using cellranger

This R script will do the pre-processing including ambient RNA removal and doublet removal, after running, 'before_QC.RDS' and 'annotations.RDS' will be saved for following analysis.

`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step1_pre-processing.rmd',params=list(dir_config = '/home/yuntian/multiome/version9_test/config.yml'),clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`

## Step 2: QC

This R script will do the quality control, after running, 'after_QC.RDS' will be saved for following analysis.

`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step2_QC.rmd',params=list(dir_config = '/home/yuntian/multiome/version9_test/config.yml'),clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`

## Step 3: Process cluster markers

This R script will do the peak recalling, batch correction and process cluster markers, after running, 'after_link_peaks.RDS' and 'cluster_markers.RDS' will be saved for cell type annotation and following analysis.

`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step3_batch_correction.rmd',params=list(dir_config = '/home/yuntian/multiome/version9_test/config.yml'),clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`

## Step 4: Cell type annotation and marker plotting

### part one

This R script will do the UMAP, cell type proportion histogram, find top marker genes for each cell type with feature plot and dotplot, feature plots for marker genes based on prior knowledge (skin_cluster_marker_panel.png), and motif analysis.

`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step4_with_cell_type_part1.rmd',params=list(dir_config = '/home/yuntian/multiome/version9_test/config.yml'),clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`

#### output files (saved files besides saved RDS files and pdf document)

```{bash}
1. **gene markers for each cell type_RNA.csv**: the gene marks found for each cell type. Please note those markers satisfy "min.pct > 0.1" and "log2 fold change > 0.25", but has not been filtered by "adjusted p value"

2. **rna_top_gene_celltype.csv**: the most upregulated genes from each cluster, with the lowest p_val_adj and highest avg_log2FC

3. **skin/pbmc_cluster_marker_panel.png**: feature plots for some cluster markers identified before

4. **gene markers for each cell type_ATAC.csv**: the differentially accessible peaks for each cell type
```

### part two

This R script will find marker genes for comparison between different conditions of each cell type.

`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step4_with_cell_type_part2.rmd',params=list(dir_config = '/home/yuntian/multiome/version9_test/config.yml'),clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`

#### output files (saved files besides saved RDS files and pdf document)

```{bash}
1. **cond1 vs cond2 test_option 1.2_0.25_SCT.csv**: DEGs found from specified condition comparison within each cell type. Please note those markers satisfy "min.pct > 0.25" and "log2FC > log2(1.2)", but has not been filtered by "adjusted p value". e.g. LESIONAL vs CTRL Poisson 1.2_0.25_SCT.csv means we identify differentially expressed genes between lesional and ctrol using a poisson generalized linear model

2. **sig_ cond1 vs cond2 test_option 1.2_0.25_SCT.csv**: significant DEGs filtered with "adjusted_p_value < 0.05"

3. **sig_pos cond1 vs cond2 test_option 1.2_0.25_SCT.csv**: significant DEGs filtered with "adjusted_p_value < 0.05" and "avg_log2FC > 0", i.e those sig. DEGs are up in cond1

4. **sig_neg cond1 vs cond2 test_option 1.2_0.25_SCT.csv**: significant DEGs filtered with "adjusted_p_value < 0.05" and "avg_log2FC < 0", i.e those sig. DEGs are up in cond2

5. **cond1 vs cond2 test_option 1.2_0.05_peaks.csv**: differentially accessible peaks found from specified condition comparison within each cell type. Please note those markers satisfy "min.pct > 0.05" and "log2FC > log2(1.2)", but has not been filtered by "adjusted p value". e.g. LESIONAL vs CTRL LR 1.2_0.05_peaks.csv means we utilize a logistic regression framework to determine differentially accessible peaks
```
Please note that in all of the test results, if the **avg_log2FC > 0**, it means this gene is up in **cond1**, and if **avg_log2FC < 0**, it means this gene is down in **cond1**.

