---
title: 'Parameter estimation in R: a simple stomatal conductance model example'
author: Koen Hufkens
layout: post
permalink: /2016/10/02/paramater-estimation-in-r-a-simple-stomatal-conductance-model-example/
categories:
  - R
  - Research
  - Science
  - Software
tags:
  - R
  - research
  - science
  - software
  - stomatal conductance
---
Most modeling approaches use some sort of optimization method to estimate parameters. These parameters are the knobs one has to turn to tune a statistical or mechanistic model exactly right to make it fit the data. Below I give a quick example of parameter estimation (in R) using a stomatal conductance (g<sub>s</sub>) model.

A collaborator in the lab came to me with the question to quickly introduce him to parameter estimation in R in order to optimize a stomatal conductance model. This is the document and code I used to briefly explain the methodology used.

In this example a model calculates stomatal conductance based upon environmental variables and measurements of g<sub>max</sub> (or the maximum diffuse stomatal conductance). This model illustrates nicely how one can use R and an optimizer to estimate complex model parameters of non-linear models.

The framework I put together estimates parameters using a [Generalized Simulated Annealing optimization R package](https://cran.r-project.org/web/packages/GenSA/index.html). However, different optimization packages and methods exist. Some worth mentioning are the default 'optim' function from the 'stats' package and [DiffeRential Evolution Adaptive Metropolis (DREAM)](http://dream.r-forge.r-project.org/). However, I found for smaller project and quick model development GenSA works really well.

The crude stomatal conductance model as formulated borrows heavily from model structures as described by Jarvis (1976) and White et al. 1999, both succintly described in Damour et al. (2010). The latter paper describes a variety of stomatal conductance models, which could be used with the framework as outlined in this example.

In this model example stomatal conductance is modelled as:

g<sub>s</sub> = g<sub>max</sub> * f(CO<sub>2</sub>) * f(VPD) * f(PAR)

Here CO<sub>2</sub> and VPD (vapour pressure deficit) response curves are modelled using an epxonential equation, while PAR (photosynthetically active radiation) is modelled using Michaelis-Menten kinetics and g<sub>max</sub> is measured and calculated as defined by Franks &amp; Beerling (2009).

- f(CO<sub>2</sub>) = c1 * e <sup>-c<sub>2</sub>[CO<sub>2</sub>]</sup>

- f(VPD) = v1 * e <sup>-v<sub>2</sub>[VPD]</sup>

- f(PAR) = ( 2000 * PAR) / (p1 + PAR)

Or condensed and written into an R function this gives:

```r
gs.model = function(par = par, data = data) {

# put variables in readable format
vpd = data$vpd
co2 = data$co2
par_val = data$par
gmax = data$gmax

# unfold parameters
# for clarity
c1 = par[1]
c2 = par[2]
v2 = par[3]
p1 = par[4]
#p2 = par[5]

# model formulation
gs = gmax * c1 * exp(c2 * co2) * exp(v2 * vpd) * (( 2000 * par_val) / (p1 + par_val))

# return stomatal conductance
return(gs)
}
```
Here, the 'data' and 'par' variables are a data frame and vector including the model drivers and parameters respectively.

The optimization minimizes a cost function which in this example is defined as the root mean squared error (RMSE) between the observed and the predicted values.

```r
# run model and compare to true values
# returns the RMSE
cost.function = function(data, par) {
obs = as.vector(data$COND)
pred = as.vector(gs.model(par = par, data = data))
s = (obs - pred)^2
RMSE = sqrt(sum(s, na.rm = TRUE)/length(pred))
return(RMSE)
}
```

In the final optimization will iteratively step through the parameter space, running the cost function (which in terms calls the main model), and find an optimal solution to the problem (i.e. minimize the RMSE). Often starting model parameters and limits to the parameter space are required. **Defining the limits of your parameter space** well can significantly reduce the time needed to converge upon a solution. The main reason is that the model structure on it's own does not have any sense of the physical reality of the world. If measured values can never be lower than 0 it does not make sense to look for a multiplicative parameter in the negative range of the parameter space.

```r
# starting model parameters
par = c(0.65, -0.002, -0.689, 0.05)

# limits to the parameter space
lower = c(0,-10,-100,0)
upper = c(100,0,0,100)

# optimize the model parameters
optim.par = GenSA(
par = par,
fn = cost.function,
data = data,
lower = lower,
upper = upper,
control=list(temperature=4000)
)
```

As the data I used in the development of this model aren't published yet I can't include any graphs. However, I think that this description can give people a quick introduction in getting started with model development (in R).

## References

Damour G., Simonneau T., Cochard H., Urban L. (2010) An overview of models of stomatal conductance at the leaf level. Plant Cell &amp; Environment 2010 33, 1419-38.

Jarvis P.G. (1976) The interpretation of the variations in leaf water potential and stomatal conductance found in canopies in the field. Philosophical Transactions of the Royal Society of London. Series B 273, 593–610.

White D.A., Beadle C., Sands P.J., Worledge D. &amp; Honeysett J.L.(1999) Quantifying the effect of cumulative water stress on sto- matal conductance of *Eucalyptus globulus* and *Eucalyptus nitens*: a phenomenological approach. Australian Journal of Plant Physiology 26, 17–27.

Franks, P. J. &amp; Beerling, D. J. (2009) Maximum leaf conductance driven by CO2 effects on stomatal size and density over geologic time. Proceedings of the National Academy of Sciences 106, 10343–10347.