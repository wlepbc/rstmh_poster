---
layout: default
title: Methods
---

### Analyses

We analysed data from all 27 Brazilian states. Each state was analysed independently using the same model.

#### Data

State-level time series of annual summary data on the:

 * number of new cases (1990-2014);
 * number of new multibacillary cases (2000-2014); and
 * population (2000-2012)

were compiled from three sources. The sources were the [SINAN database’s web interface](http://tabnet2.datasus.gov.br/cgi/deftohtm.exe?idb2013/d0206.def) (Accessed April 7, 2016) for the years 1990 to 2012 and two documents retrieved from the Brazilian government's health portal ([2013, accessed May 26, 2016](http://portalsaude.saude.gov.br/images/pdf/2014/dezembro/01/Dados-2013.pdf) and [2014, accessed April 7, 2016](http://portalsaude.saude.gov.br/images/pdf/2015/julho/27/Dados-2014---final.pdf)). The SINAN database is the Brazilian government’s repository for information on communicable diseases.

The population data was used to predict the states' population numbers in 2020, which were used in the calculation of the new case detection rate.

#### Reported values

As an example of estimation of the underlying prevalence of leprosy, we report the maximum _a posteriori_ estimates of the cumulative number of sub-clinical and clinical cases in 2014 (the last year with observations). These values, the modes of the posterior probability distributions, are displayed on a map of Brazil.

Our example for forecasting here is the prediction of the probability that NCDR is less than 1 per 10,000 of the state population in 2020. This is an example, and we are not concerned with demonstrating the quality of the forecasts here.

In addition, the poster contains a number of summary graphics resulting from the analysis of Espirito Santo. These illustrate the fit to the observed data as well as the estimates of case detection success parameters.

#### Procedure

Within-state analyses were performed for all 27 states. [Multiple inference](../methods/index.html#accounting-for-uncertainty-in-the-tpd-parameters) was used to account for uncertainty in the estimated parameters of the incubation period and detection delay distributions. For each state 1,000 runs of the back-calculation program were performed retaining 25 samples from the posterior probability distributions. In each of the 1,000 runs a different randomly selected sample from the joint posterior distribution of the time period distribution (TPD) parameters was used in order to fully reflect the uncertainty about the TPD parameters while not updating their estimates. The results of the 1,000 runs were combined to give 25,000 samples.

Samples of the new case detection rate in 2020 were generated from the 25,000 samples of the number of new cases (NC). The population in 2020 was predicted assuming exponential growth (fitting to the available population data from 2000 onwards), with a different randomly selected value from the posterior distribution of population sizes in 2020 being used to calculate the NCDR for each NC sample. The probability of the NCDR being less than 1 per 10,000 of the state population was then the proportion of 2020 NCDR samples that were less than this cut-off.
