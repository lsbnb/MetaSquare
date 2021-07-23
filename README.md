# MetaSquare 1.0

## Metasquare for Nanopore 16S full-length ampricons analysis

1. Download **Metasquare_Kraken2_database** from to the https://hub.docker.com/r/lsbnb/metasquare_db 
2. Transfer **Metasquare_Kraken2_database** to the ***output_dir***
3. Run Porechop to clean up the sequences before classification
```
$docker run --rm -v $(pwd):/data --name=porechop -it robegan21/porechop
$cd data
$porechop -i input_reads.fastq.gz -o output_reads_1.fastq.gz
$porechop -i output_reads_1.fastq.gz -b output_dir/seq.fastq
```
4. Install [**Kraken2**](https://github.com/DerrickWood/kraken2)
5. Run Kraken2 to Classification at ***output_dir***
```
kraken2 -db Metasquare_Kraken2_database --threads N --report seq.fasta.report seq.fastq
```
6. The .report file could be vizualized and analyzed through online-tool [**Pavian**](http://mirai.iis.sinica.edu.tw:5000/)
## Metasquare for illumina 16S variants 3-4 ampricons analysis
1. Download combine6DB_V3V4_classifier.qza to target folder from https://hub.docker.com/r/lsbnb/metasquare_db
2. Initialize **QIIME2** 2019.10 version
```
$docker run -t -i -v $(pwd):/data quay.io/qiime2/core:2019.10 qiime
$cd data
```
3. Processed data through [QIIME2 pipline](https://docs.qiime2.org/2021.4/tutorials/moving-pictures/)
4. Run QIIME2 clasaification
```
qiime feature-classifier classify-sklearn \
--i-classifier Metasquare_QIIME2_V3V4_classifier.qza \
--i-reads rep-seqs.qza \
--p-n-jobs N \  
--o-classification taxonomy.qza
```
5. Export QIIME2 visualized file
```
qiime taxa barplot \
--i-table table.qza \
--i-taxonomy taxonomy.qza \
--m-metadata-file sample-metadata.tsv \
--o-visualization taxa-bar-plots.qzv
```
6. QZV file could be analysis on website [**QIIME 2 View**](https://view.qiime2.org/)

**Version history:**

First Version: v1.0, 2, June, 2021

**Reference:**

1.https://github.com/rrwick/Porechop

2.Wood, D.E., Lu, J. & Langmead, B. Improved metagenomic analysis with Kraken 2. Genome Biol 20, 257 (2019). https://doi.org/10.1186/s13059-019-1891-0

3.Bolyen E, et. al 2019. Reproducible, interactive, scalable and extensible microbiome data science using QIIME 2. Nature Biotechnology 37: 852–857. https://doi.org/10.1038/s41587-019-0209-9

