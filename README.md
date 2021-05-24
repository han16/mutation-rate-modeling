# Mutation rate modeling in human genome 

Poisson regression model will be used to learn how genomic features influence mutation rate, ideally at the single base level using de novo mutation data, with the aim of providing insight on mutagenic mechanism. 

The pipeline will proceed in the following. 

## Step 1: Prepare baseline mutation rate 

To improve the model fitting, we consider k-mer mutation rate as offset in the model, for example k=3, 7. (k=7 is recommended by [Jedidiah Carlson, et al 2018])

[Jedidiah Carlson, et al 2018]:https://www.nature.com/articles/s41467-018-05936-5

## Step 2: Combine mutation count and all features in a matrix 

### Step 2.1 Get mutation count and covariate features by mutation type 

Get mutation subtype, such as `A->C`, `A->G`, and so on. In total there are 6 mutation subtypes as one strand and its complement strand are counted as one subtype, for instance, `A->C` includes both `A->C` and `T->G`. 

```
step1_get.mutation.data.of.mutation.type.sh

```

Covariate features are discrete or continuous. Given a window, for discrete features we count how many are overlapping with the genomic window, and for continuous features, we use the mean as the covariate value.    

```
step1_extract.cova.feature.sh
```

### Step 2.2 Split into CpG and non-CpG

For cytosine, such as `C->A`, `C->G`, `C->T` split into CpG and non-CpG. Make sure the strand is plus or minus strand, which will lead to identifying CpG sites. If the current position at S is C, then it's CpG when the position at S+1 is G for plus strand. Given a G at current position S (when we look at subtype `G->T` corresponding to `C->A` in the complementary strand), it's a CpG when the position at S-1 is C for minus strand.    

```
step2_split.base.C.into.CpG.nonCpG.sh
```

### Step 2.3 Get mutation count on every chromosome for each mutation subtype 

Due to memory limit, it's feasible to split into chromosome-wide analysis. That is on each chromosome, get mutation count for every mutation type. 

```
step3_get.mutation.data.on.every.chromsome.every.mutation.type.sh
```


### Step 2.4 Combine mutation count, baseline mutation rate and all features together 

Given a window size say 100bp, pool together baseline mutation rate in 3-mer model for every mutation type, and other covariate features.  

```
step4_combine.cova.feature.mutation.and.baseline.rate.sh
```


## Step 3: Run Generalized Linear Model (GLM)

It's computational intractable to run Poisson regression via GLM on ~3B/100bp windows directly. Observing the mutation counts  and some covariate features are sparse, thus many windows share similar features. `Split` which divides into Groups and Reassembles is employed multiple times to perform the analysis.  

```
step5_poiss.reg.by.cate.R
```


## Step 4: Model diagnostic 

