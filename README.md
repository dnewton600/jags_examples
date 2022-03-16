## Jags Examples

This is a central repository for some of the Jags (MCMC library) files I have written for various statistical inference problems. Included are several flavors of hierarchical models. 

Below is an example (taken from [example_run_rjags.Rmd](example_run_rjags.Rmd) ) of how to run mcmc for a varying slope/intercept model. The model data, parameters to save, and initializations will of course need to change based on the particular application, but I think it is at least useful to have a basic template.

```
model_data = list(
  N = nrow(mydata), # number of observations
  N_groups = length(unique(mydata$group)), # number of labs
  groups = mydata$group,
  y = mydata$y,
  x = mydata$x
)

parameters_to_save = c('beta_0','beta_1','sigma_0','sigma_1','sigma_eps')

model_inits = function() {
  list(
    beta_0=rnorm(1,5,1),
    beta_1=rnorm(1,1,1),
    sigma_0=rexp(1/2),
    sigma_1=rexp(1/2),
    sigma_eps=rexp(1/2)
  )
}


jags_out = jags(data = model_data,
                inits = model_inits,
                model.file='jags_model.txt',
                parameters.to.save = parameters_to_save,
                n.chains = 4,
                n.thin=2,
                n.iter = 10000,
                n.burnin = 2000,
                quiet=TRUE)

p_samples = jags_out$BUGSoutput$sims.list
```
