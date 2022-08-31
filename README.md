# Multiome-Pipeline-new

# Multiome-Pipeline
This pipeline comprises 4 steps as can be seen below. The `output_dir` option in the bash code specifies the directory where the output pdf files will be stored

## To use the pipeline:
Run the following sample bash code for each step, please note that all the input arguments should be listed in the config.yml.

example bash command: 

`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step1_pre-processing.rmd',clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`


`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step2_QC.rmd',clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`


`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/step3_batch_correction.rmd',clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`


`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/Step4_with_cell_type_part1.rmd',clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`


`/usr/bin/Rscript -e "rmarkdown::render('/home/yuntian/multiome/version9/Step4_with_cell_type_part2.rmd',clean=TRUE, output_dir = '/home/yuntian/multiome/version9_test/')"`
