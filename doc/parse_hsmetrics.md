# parse_hsmetrics.py

## Description:
Python script to create a Dictionary and csv file with the statistics of hsmetrics.out files obtained with [Picard CollectHsMetrics](https://broadinstitute.github.io/picard/picard-metric-definitions.html#HsMetrics) in the [BU-ISCIII-exome-pipeline](https://github.com/BU-ISCIII/exome_pipeline):

Metrics generated by CollectHsMetrics for the analysis of target-capture sequencing experiments. The metrics in this class fall broadly into three categories:

*   Basic sequencing metrics that are either generated as a baseline against which to evaluate other metrics or because they are used in the calculation of other metrics. This includes things like the genome size, the number of reads, the number of aligned reads etc.
*   Metrics that are intended for evaluating the performance of the wet-lab assay that generated the data. This group includes metrics like the number of bases mapping on/off/near baits, %selected, fold 80 base penalty, hs library size and the hs penalty metrics. These metrics are calculated prior to some of the filters are applied (e.g. low mapping quality reads, low base quality bases and bases overlapping in the middle of paired-end reads are all counted).
*   Metrics for assessing target coverage as a proxy for how well the data is likely to perform in downstream applications like variant calling. This group includes metrics like mean target coverage, the percentage of bases reaching various coverage levels, and the percentage of bases excluded by various filters. These metrics are computed using the strictest subset of the data, after all filters have been applied.

Prints summary statistics from .bam files in hsmetrics.out file:

- BAIT\_SET: The name of the bait set used in the hybrid selection.
- GENOME\_SIZE: The number of bases in the reference genome used for alignment.
- BAIT\_TERRITORY: The number of bases which are localized to one or more baits.
- TARGET\_TERRITORY:The unique number of target bases in the experiment, where the target sequence is usually exons etc.
- BAIT\_DESIGN\_EFFICIENCY:The ratio of TARGET\_TERRITORY/BAIT\_TERRITORY. A value of 1 indicates a perfect design efficiency, while a valud of 0.5 indicates that half of bases within the bait region are not within the target region.
- TOTAL\_READS:The total number of reads in the SAM or BAM file examined.
- PF\_READS: The total number of reads that pass the vendor's filter.
- PF\_UNIQUE\_READS: The number of PF reads that are not marked as duplicates.
- PCT\_PF\_READS: The fraction of reads passing the vendor's filter, PF\_READS/TOTAL\_READS.
- PCT\_PF\_UQ\_READS:The fraction of PF\_UNIQUE\_READS from the TOTAL\_READS, PF\_UNIQUE\_READS/TOTAL\_READS.
- PF\_UQ\_READS\_ALIGNED: The number of PF\_UNIQUE\_READS that aligned to the reference genome with a mapping score > 0.
- PCT\_PF\_UQ\_READS\_ALIGNED: The fraction of PF_UQ_READS_ALIGNED from the total number of PF reads.
- PF\_BASES\_ALIGNED: The number of PF unique bases that are aligned to the reference genome with mapping scores > 0.
- PF\_UQ\_BASES\_ALIGNED: The number of bases in the PF\_UQ\_READS\_ALIGNED reads. Accounts for clipping and gaps.
- ON\_BAIT\_BASES: The number of PF\_BASES\_ALIGNED that are mapped to the baited regions of the genome.
- NEAR\_BAIT\_BASES: The number of PF\_BASES\_ALIGNED that are mapped to within a fixed interval containing a baited region, but not within the baited section per se.
- OFF\_BAIT\_BASES:The number of PF\_BASES\_ALIGNED that are mapped away from any baited region.
- ON\_TARGET\_BASES: The number of PF\_BASES\_ALIGNED that are mapped to a targeted region of the genome.



```
##Example for runnig Picard CollectHsMetrics in SGE cluster:

qsub -V -b y -j y -l h_vmem=15g -cwd -N PICARDHSMETRICS.sample -q all.q
java -Xmx10g -jar /opt/picard-tools/picard-tools-1.140/picard.jar CalculateHsMetrics 
BI= /path/to/capture_targets.interval_list 
TI= /path/to/panel-targets.interval_list 
I=/path/to//sample.woduplicates.bam 
O=sample_hsMetrics.out VALIDATION_STRINGENCY='LENIENT'

```



## Input files:

 hsmetrics.out files including path where are stored 

```
--input /path/to/*hsmetrics.out
        
```
  
## Output files:
A dictionary converted to csv file with the hsMetrics statistics

Column names of the obtained csv file:


* sample
* hsMetrics\_AT\_DROPOUT
* hsMetrics\_BAIT\_DESIGN\_EFFICIENCY
* hsMetrics\_BAIT\_SET
* hsMetrics\_BAIT\_TERRITORY
* hsMetrics\_FOLD\_80\_BASE\_PENALTY
* hsMetrics\_FOLD\_ENRICHMENT
* hsMetrics\_GC\_DROPOUT
* hsMetrics\_GENOME\_SIZE
* hsMetrics\_HS\_LIBRARY\_SIZE
* hsMetrics\_HS\_PENALTY\_100X
* hsMetrics\_HS_PENALTY\_10X
* hsMetrics\_HS\_PENALTY\_20X
* hsMetrics_HS\_PENALTY\_30X
* hsMetrics\_HS_PENALTY\_40X
* hsMetrics\_HS_PENALTY\_50X
* hsMetrics\_LIBRARY
* hsMetrics\_MEAN\_BAIT\_COVERAGE
* hsMetrics\_MEAN\_TARGET\_COVERAGE
* hsMetrics\_NEAR\_BAIT\_BASES
* hsMetrics\_OFF\_BAIT\_BASES
* hsMetrics\_ON\_BAIT\_BASES
* hsMetrics\_ON\_BAIT\_VS\_SELECTED
* hsMetrics\_ON\_TARGET\_BASES
* hsMetrics\_PCT\_OFF\_BAIT
* hsMetrics\_PCT\_PF\_READS
* hsMetrics\_PCT\_PF\_UQ\_READS
* hsMetrics\_PCT\_PF\_UQ\_READS\_ALIGNED
* hsMetrics\_PCT\_SELECTED\_BASES
* hsMetrics\_PCT\_TARGET\_BASES\_100
* hsMetrics\_PCT\_TARGET\_BASES\_10X
* hsMetrics\_PCT\_TARGET\_BASES\_20X
* hsMetrics\_PCT\_TARGET\_BASES\_2X
* hsMetrics\_PCT\_TARGET\_BASES_30X
* hsMetrics\_PCT\_TARGET\_BASES\_40X
* hsMetrics\_PCT\_TARGET\_BASES\_50X
* hsMetrics\_PCT\_USABLE\_BASES\_ON\_BAIT
* hsMetrics\_PCT\_USABLE\_BASES\_ON\_TARGET
* hsMetrics\_PF\_READS
* hsMetrics\_PF\_UNIQUE_READS
* hsMetrics\_PF_UQ_BASES_ALIGNED
* hsMetrics\_PF\_UQ\_READS\_ALIGNED
* hsMetrics\_READ\_GROUP
* hsMetrics\_SAMPLE
* hsMetrics\_TARGET\_TERRITORY
* hsMetrics\_TOTAL\_READS
* hsMetrics\_ZERO\_CVG\_TARGETS\_PCT



```
--out /path/to/Results/dic_hsMetrics_all.csv
``` 

## Example

For running in a local computer:

```
python3 /path/to/parse_hsmetrics.py
--input /path/to/hsmetrics/*.out
--out /path/to/RESULTS/dic_hsmetrics.csv

```
 

For submission to the SGE cluster:

```
qsub -V -b y -j y -cwd -N "parse_hsmetrics_date" -q all.q python3 /path/to/parse_hsmetrics.py
--input /path/to/hsmetrics/*.out
--out /path/to/RESULTS/dic_hsmetrics.csv

```
   