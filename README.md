# Mutation rate modeling in human genome 

Poisson regression model will be used to learn how genomic features influence mutation rate, ideally at the single base level using de novo mutation data, with the aim of providing insight on mutagenic mechanism. 

The pipeline will proceed in the following. 

## Step 1: Prepare baseline mutation rate 

To improve the model fitting, we consider k-mer mutation rate as offset in the model, for example k=3, 7. (k=7 is recommended by [Jedidiah Carlson, et al 2018])

[Jedidiah Carlson, et al 2018]:https://www.nature.com/articles/s41467-018-05936-5

## Step 2: Combine mutation count and all features in a matrix 

## Step 3: Run Generalized Linear Model (GLM)

## Step 4: Model diagnostic 

