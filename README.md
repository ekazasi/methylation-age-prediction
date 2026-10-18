# Predicting age from DNA methylation

Project for BERN02, Lund University.

## Motivation
DNA methylation, a chemical mark on DNA, changes with age in a predictable way, so it can be used to estimate a person's age. Methylation data has many more features (CpG sites) than samples, so ordinary linear regression cannot be fitted well and shrinkage methods are needed. I compare ridge, lasso and the Bayesian lasso (Blasso). Which one predicts age best and does keeping only a few sites (lasso) help or not? The error is measured in years on samples the models have not seen.

## Data
Public blood methylation data, GEO accession [GSE40279](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE40279): 656 whole-blood samples, Illumina 450k array, age as a continuous response (Hannum et al. 2013. Genome-wide methylation profiles reveal quantitative views of human aging rates. Molecular cell, 49(2), 359–367. https://doi.org/10.1016/j.molcel.2012.10.016).
The notebook downloads the data from GEO (about 1.1 GB, once) and keeps the 20,000 CpG sites with the highest variance. The choice uses no age information. Ages come from the sample titles that GEO lists (for example `age 67y 1001`), read from the top of the series matrix file. Raw data is not stored in this repository.

## Methods
Ridge and lasso regression, with 5-fold cross-validation used to tune the penalty and to see how well each model performs (error in years) and a separate test set for the final comparison. A Bayesian lasso (Park and Casella 2008), fitted by Gibbs sampling, is the third model. Two versions are run: lambda with its own prior (did not converge) and lambda fixed from the cross-validated lasso (used for the comparison). Uncertainty intervals come from the bootstrap (ridge and lasso) and from the posterior (Bayesian lasso).

## Blasso references
- Park, T., & Casella, G. (2008). The Bayesian Lasso. Journal of the American Statistical Association, 103(482), 681–686. https://doi.org/10.1198/016214508000000337
- https://github.com/ogrecio/BLasso

## How to run
```bash
conda env create -f environment.yml
conda activate methylation-age
```
Run the notebook from top to bottom. The first run downloads the data into `data/raw/`, but later runs reuse it.

## Repository structure
| Path | Content |
|---|---|
| `age_prediction_methylation_final.ipynb` | Full analysis (data, models, figures, FAIR principles) |
| `environment.yml` | environment |
| `figures/` | Figures produced by the notebook |
| `results/` | Result tables produced by the notebook |
| `data/` | Created when the notebook runs |

## License
MIT (see `LICENSE`). The data belongs to its original authors and is distributed by GEO.
