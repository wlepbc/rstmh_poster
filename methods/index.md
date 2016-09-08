---
layout: default
title: Methods
---
* TOC
{:toc}

### Methods

Our approach is to use back-calculation in a Bayesian framework to provide probabalistic inferences about leprosy from reported diagnoses.

#### Basic back-calculation

For discrete time periods, such as with annually reported counts of diagnoses, the basic back-calculation equation is:

\\[ D_i = \sum_{j=1}^i I_jf_{i-j} \\]

where \\( D_i \\) is the number of new diagnoses in the \\( i^\text{th} \\) time interval, \\( I_j \\) is the expected number of infections in the \\( j^\text{th} \\) time interval and \\( f_{i-j} \\) is the probability that the time between infection and diagnosis is  \\( i-j \\) time intervals. In the simplest case \\( f_{i-j} \\) comes directly from the incubation period distribution. The logic of back-calculation is that given knowledge of \\( D \\) and \\( f \\), the convolution above allows estimation of \\( I \\).

#### Modifications

A number of modifications to the basic back-calculation have been made to suit analysis of the available data on leprosy.

+ The time from infection to diagnosis was considered as two periods:
     1. from infection to onset of symptoms (the incubation period distribution, IPD); and
     2. from onset of symptoms to diagnosis (the detection delay distribution, DDD).
+ Scaling of the hazard associated with the DDD by a treatment speed parameter,
+ Linking of the number of new infections in a year to the size of the infective pool at that time.


### Time Period Distributions (TPD)

As stated above, we consider the time from infection to diagnosis as two periods. The distributions of these time periods we refer to as the incubation period distribution (IPD) and the detection delay distribution (DDD).

Because leprosy is endemic in many countries and the IPD is long and variable, it is hard to find data with known exposure, clinical symptom onset and diagnosis times. These data are required to estimate the parameters of the IPD and DDD, although data on the DDD can be obtained separately. Here, as in [Crump and Medley (2015)](http://doi.org/10.1186/s13071-015-1142-5), we estimated the parameters of the IPD and DDD in a Bayesian setting from a small dataset derived from literature reports and consisting mainly of United States military veterans. The IPD and DDD were assumed to be gamma distributions with a common rate parameter and individual shape parameters.

#### Accounting for uncertainty in the TPD parameters

The back-calculation requires information on the TPD, in the form of parameters from which the distributions can be regenerated. In order that we include the uncertainty about our TPD parameter estimates, we perform **multiple inference**.

In multiple inference, a number (in our case 1,000) runs of the analysis are performed, each using a different randomly selected trio of TPD parameters from the joint posterior distribution of these values. We retain relatively few posterior samples from each of these runs (25 in this study), in order to keep them brief, and combine the samples across all runs to obtain 25,000 posterior samples for each analysis which reflect the variability in our TPD parameter estimates.

### The back-calculation model

We implemented our model in the [Stan probabalistic programming language](http://mc-stan.org). Stan performs Bayesian statistical inference via Markov Chain Monte Carlo sampling.

A Stan program consists of a number of structural blocks, including parameters and transformed parameters. The parameters are the values purely being sampled while the transformed parameters are functions of these and data.

#### Model description

The number of new sub-clinical cases in a year is assumed to be proportional to the size of the pool of infectives from the previous year. We assume undiagnosed, clinical cases to be infective.

#### Data

The main data which must be provided to the program are:
* annual number of new leprosy cases;
* annual number of new multibacillary (MB) leprosy cases;
* rate and shape parameters of IPD and DDD;
  * these vary across [multiple inference](#accounting-for-uncertainty-in-the-tpd-parameters) runs within an analysis; and
* definitions of time periods within which [case detection effort](#effort) is constant

#### Parameters

#### Transformed parameters
