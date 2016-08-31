---
layout: default
title: Methods
---

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
