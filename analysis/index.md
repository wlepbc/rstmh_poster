---
layout: default
title: Methods
---

### Data

Annual summary data by Brazilian state were extracted via the [SINAN database’s web interface](http://tabnet2.datasus.gov.br/cgi/deftohtm.exe?idb2013/d0206.def) (Accessed April 7, 2016) for the years 1990 to 2012. The SINAN database is the Brazilian government’s repository for information on communicable diseases. The data retained for use in this study consisted of the annual number of new cases diagnosed (NC) for multibacillary and paucibacillary diagnoses combined for 1990 to 2012, and for multibacillary cases separately from 2000 to 2012, along with the population size. Equivalent data for 2013 and 2014 were retrieved from documents on the Brazilian government's health portal ([2013 summary table, accessed May 26, 2016](http://portalsaude.saude.gov.br/images/pdf/2014/dezembro/01/Dados-2013.pdf); [2014 summary table, accessed April 7, 2016](http://portalsaude.saude.gov.br/images/pdf/2015/julho/27/Dados-2014---final.pdf)).

### Analyses

Within-state analyses were performed for all 27 states. [Multiple inference](../methods/index.html#accounting-for-uncertainty-in-the-tpd-parameters) was used to account for uncertainty in the estimated parameters of the incubation period and detection delay distributions. For each state 1,000 runs of the back-calculation program were performed retaining 25 samples from the posterior probability distributions. In each of the 1,000 runs a different randomly selected sample from the joint posterior distribution of the time period distribution parameters was used. The results of the 1,000 runs were combined to give 25,000 samples.

Samples of the new case detection rate in 2020 were generated from the 25,000 samples of the number of new cases (NC). The population in 2020 was predicted assuming exponential growth (fitting to the available population data from 2000 onwards), with a different randomly selected value from the posterior distribution of population sizes in 2020 being used to calculate the NCDR for each NC sample.
