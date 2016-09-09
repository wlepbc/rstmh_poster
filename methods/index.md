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

#### Our model

In our model we treat the time from infection to diagnosis as two periods:

* from infection to onset of symptoms (the incubation period); and
* from onset of symptoms to diagnosis (the detection delay).

This allows us to differentiate between sub-clinical and undiagnosed clinical cases.

The success rate of case detection may vary over time; with such things as awareness programmes and active case detection affecting the probability of being diagnosed in a given year. Therefore, we include case detection speed parameters (fitted within time periods) which scale the hazard function of the detection delay distribution.

In  [Crump and Medley (2015)](http://doi.org/10.1186/s13071-015-1142-5) the expected number of new subclinical cases were generated using a random walk procedure. That is, the expected number of new sub-clinical cases in any year was equal to the expected number of sub-clinical cases in the previous year plus a random deviate. For the current study the expected number of sub-clinical cases in any year is directly proportional to the size of the infective pool in the previous year. The scalar proportion is included as a parameter to be estimated.

### Implementation

The model was coded in the [Stan probabalistic programming language](http://mc-stan.org). Stan performs Bayesian statistical inference via Markov Chain Monte Carlo sampling.

All analyses were performed in the [R statistical environment](https://www.R-project.org/) via the [rstan interface package](http://mc-stan.org).

The analysis of each state entailed a number of runs using different TPD parameters. This [multiple inference](#accounting-for-uncertainty-in-the-tpd-parameters) loop was controlled at the R level, with the same back-calculation program called with data varying only for the TPD parameters.

#### Data

The main data which must be provided to the program are:
* annual numbers of new leprosy cases, and associated years;
* annual numbers of new multibacillary (MB) leprosy cases, and associated years;
* rate and shape parameters of IPD and DDD;
  * these vary across [multiple inference](#accounting-for-uncertainty-in-the-tpd-parameters) runs within an analysis; and
* definitions of time periods within which [case detection success](#effort) is constant.

#### The model

The progression from sub-clinical to clinical cases and subsequent diagnosis was described by a discrete time period model. Calendar years were used as the discrete time step, in keeping with the annual reporting of case detection summary information.

The time period for the analysis was set to start 30 years before the first observed record. Prior to this an equilibrium state was assumed.

The expected incidence of new sub-clinical cases in year \\( b \\), \\(S_b\\), was assumed to be directly proportional to the expected size of the infective pool in year \\( b-1 \\), \\(I_{b-1}\\):

\\[ \text{E}\left[ S_b \right] = q I_{b-1} \\]

where the parameter \\(q\\) had an informative Beta prior with expectation equal to \\(q_{eq}\\), the value of \\(q\\) at equilibrium, and \\(I_{b-1}\\) was the cumulative number of undiagnosed clinical cases up to year \\(b-1\\).

Progress of sub-clinical cases to clinical cases was controlled by the IPD. The expected number of new clinical cases in year \\(c\\) being:

\\[ \text{E}\left[O_c\right]=\sum_{b=1}^c S_bf_{c-b}\\]

where \\(f_{c-b}\\) was the probability that the time between infection and onset of clinical symptoms was \\(c-b\\) years.

Variations in case detection effectiveness are handled by scaling the hazard of being diagnosed in year \\(d\\) given that onset of clinical symptoms occurred in year \\(c\\) (\\(h_{dc}\\)). The probability of an individual being diagnosed in year \\(d\\) given that the onset of clinical symptoms was in year \\(c\\) was:

\\[ g_{dc} = \gamma_kh_{dc}\left( 1-G_{dc} \right) \\]

where \\(G_{dc}=\sum_{i=1}^{d-1}g_{ic}\\), case detection effectiveness in period \\(k\\) is modelled by \\(\gamma_k\\) and \\(h_{dc}\\) is the hazard function of the DDD. The expected number of new cases detected in year \\(d\\) is then:

\\[\text{E}\left[D_d\right]=\sum_{j=1}^d\text{E}\left[O_j\right]g_{dj}\\]

and the expected number of these which were multibacillary is \\( \text{E}\left[M_d\right] = \phi\text{E}\left[D_d\right] \\).

The \\(\gamma_k\\) parameters were given informative priors with an expectation of 1 and the prior for \\(\phi\\) was \\(\text{Beta}\left(3,3\right)\\).

Poisson likelihoods were assigned to the observed numbers of new cases and new multibacillary cases in each year, \\( D_d \sim \text{Poisson}\left(\text{E}\left[D_d\right]\right) \\) and \\(M_d \sim \text{Poisson}\left(\text{E}\left[M_d\right]\right) \\).

Because leprosy is endemic to Brazil and we require a pre-existing pool of infectives, an equilibrium is assumed prior to the analysis period. During this period \\(q_{eq}\\) is the average time for onset of symptoms to diagnosis and \\(\gamma_{eq} = 1\\). During this period the expected number of new cases per year is \\( D_{eq} \\), a parameter given a gamma distributed prior with expectation equal to the maximum observed number of cases and variance equal to the square of the expectation. Using an equilibrium period allows us to easily generate the numbers of sub-clinical and clinical infections from years prior to the analysis period. A 30 year interval between the equilibrium period and the start of the data observation period was used.

Samples at the observed level rather than on expectations were generated using binomial sampling.

#### Time Period Distributions (TPD)

As stated above, we consider the time from infection to diagnosis as two periods. The distributions of these time periods we refer to as the incubation period distribution (IPD) and the detection delay distribution (DDD).

Because leprosy is endemic in many countries and the incubation period is long and variable, it is hard to find data with known exposure, clinical symptom onset and diagnosis times. These data are required to estimate the parameters of the IPD and DDD, although data on the DDD can be obtained separately. Here, as in [Crump and Medley (2015)](http://doi.org/10.1186/s13071-015-1142-5), we estimated the parameters of the IPD and DDD in a Bayesian setting from a small dataset derived from literature reports and consisting mainly of United States military veterans. The IPD and DDD were assumed to be gamma distributions with a common rate parameter and individual shape parameters.

##### Accounting for uncertainty in the TPD parameters

The back-calculation requires information on the TPD, in the form of parameters from which the distributions can be regenerated. In order that we include the uncertainty about our TPD parameter estimates, we perform **multiple inference**.

In multiple inference, a number (in our case 1,000) runs of the analysis are performed, each using a different randomly selected trio of TPD parameters from the joint posterior distribution of these values. We retain relatively few posterior samples from each of these runs (25 in this study), in order to keep them brief, and combine the samples across all runs to obtain 25,000 posterior samples for each analysis which reflect the variability in our TPD parameter estimates.

