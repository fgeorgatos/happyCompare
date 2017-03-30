README
================

-   [happyCompare](#happycompare)
    -   [Demo datasets](#demo-datasets)
        -   [`happyCompare_list`](#happycompare_list)
    -   [Examples](#examples)
        -   [Static reports](#static-reports)

happyCompare
============

Reporting toolbox for happy output

Demo datasets
-------------

#### `happyCompare_list`

A demo `happyCompare_list` object, obtained by running hap.py on hap.py example data and loading the results into R using `happyCompare::read_samplesheet()`.

**Step 1: run hap.py**

    $HAPPY_ROOT=/path/to/hap.py/repo/root
    $HAPPY_BIN=/path/to/hap.py/binnary

    # fixed hap.py resources
    truth=$HAPPY_ROOT/example/happy/PG_NA12878_hg38-chr21.vcf.gz
    conf=$HAPPY_ROOT/example/happy/PG_Conf_hg38-chr21.bed.gz
    ref=$HAPPY_ROOT/example/happy/hg38.chr21.fa

    # query VCFs
    gatk_q=$HAPPY_ROOT/example/happy/NA12878-GATK3-chr21.vcf.gz
    freebayes_q=$HAPPY_ROOT/example/happy/NA12878-Freebayes-chr21.vcf.gz
    platypus_q=$HAPPY_ROOT/example/happy/NA12878-Platypus-chr21.vcf.gz

    # run hap.py
    mkdir -p $outdir
    $HAPPY_BIN -f $conf -r $ref --no-json $truth $gatk_q --roc-filter LowQual --roc QUAL -o $outdir/gatk
    $HAPPY_BIN -f $conf -r $ref --no-json $truth $freebayes_q --roc-filter LowQual --roc QUAL -o $outdir/freebayes
    $HAPPY_BIN -f $conf -r $ref --no-json $truth $platypus_q --roc QUAL -o $outdir/platypus

**Step 2: Create a happyCompare samplesheet**

    Group.Id,Sample.Id,Replicate.Id,happy_prefix
    freebayes,NA12878,NA12878_freebayes,output/freebayes
    gatk,NA12878,NA12878_gatk,output/gatk
    platypus,NA12878,NA12878_platypus,output/platypus

**Step 3: Load data into R**

``` r
library(happyCompare)

happyCompare_list = read_samplesheet(samplesheet_path = "happyCompare/happyCompare_samplesheet.csv")
save(happyCompare_list, file = "happyCompare/happyCompare_list.RData")
```

Examples
--------

### Static reports

**Example germline report:** [germline.html](inst/examples/germline.html)

``` bash
DATA="C:/Users/mgonzalez/SublimeProjects/happy-example-data"
HAPPYCOMPARE="C:/Users/mgonzalez/SublimeProjects/happyCompare"
RESULTS_ROOT="C:/Users/mgonzalez/SublimeProjects/happy-example-data"
Rscript --vanilla $HAPPYCOMPARE/exec/happyCompare.R \
        --input_template $HAPPYCOMPARE/inst/rmd/germline.Rmd \
        --output_dir $HAPPYCOMPARE/inst/examples \
        --samplesheet $DATA/happyCompare/happyCompare_samplesheet.csv \
        --root_dir $RESULTS_ROOT
```

    ## [DONE] Output: C:/Users/mgonzalez/SublimeProjects/happyCompare/inst/examples
