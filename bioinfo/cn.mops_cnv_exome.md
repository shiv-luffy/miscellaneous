### Calling CNVs from exome data using cn.mcops package

The cn.mops package is part of the Bioconductor (http://www.bioconductor.org) project.
It allows us to detect copy number variations (CNVs) from next generation sequencing (NGS) data sets based on a generative model.

***Note:*** it is best to run 10 samples to call CNV(atleast 6 samples)

**To install the package run**  
```r
BiocManager::install("cn.mops")
```
**Import the package**  
```r
library(cn.mops)
```
**bam files(samples)**
```r
BAMFiles<-c("bams/RT045-P.bam","bams/RT001-P.bam","bams/RT041-P.bam")
```
**Genomic coordinates or bed file**  
```r
segments <- read.table("/path/to/the/target_regions_covered2.bed",sep="\t",as.is=TRUE)
```
**Get GRanges object for the genomic coordinates**  
```r
gr<-GRanges(segments[,1],IRanges(segments[,2],segments[,3]))
```
**Generate read count matrix using ```getSegmentReadCountsFromBAM``` function**  
```r
X<-getSegmentReadCountsFromBAM(BAMFiles,GR=gr)
```
**use the matrix as input for the ```cn.mops``` function**  
```r
resCNMOPS<-exomecn.mops(X)
```
**calculate integer copy numbers**  
```r
resCNMOPS<-calcIntegerCopyNumbers(resCNMOPS)
```
**create a dataframe**  
```r
CNVs<-as.data.frame(cnvs(resCNMOPS))
```
**output into a csv file**  
```r
write.csv(CNVs,file="RT001_RT041_RT045_CNV_cnmops.csv")
```
**plot to visualize**  
```r
png(filename="chr1_RT045.png")
r <- cn.mops(X[1:200, ])
segplot(r,sampleIdx=1)
dev.off()
```
