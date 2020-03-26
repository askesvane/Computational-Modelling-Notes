Chapter 7 - interactions
========================

Loading data

    # Packages
    library(pacman)
    p_load(rethinking, brms, tidyverse, patchwork)

    # Data
    data(rugged)
    d <- rugged

    detach(package:rethinking, unload = T)

Make data subsets

    # make log version of outcome
    d <- d %>% mutate(
      log_gdp = log(rgdppc_2000)
    )

    # extract countries with GDP data
    dd <- d %>% filter(complete.cases(rgdppc_2000))

    # split countries into Africa and not-Africa
    d.A1 <- dd %>% filter(cont_africa == 1) # Afrika
    d.A0 <- dd %>% filter(cont_africa == 0) # not Afrika

Fit models to the two data subsets

    b7.1 <-
      brm(data = d.A1, family = gaussian, # Afrikan data
          log_gdp ~ 1 + rugged,
          prior = c(prior(normal(8, 100), class = Intercept),
                    prior(normal(0, 1), class = b),
                    prior(uniform(0, 10), class = sigma)),
          iter = 2000, warmup = 1000, chains = 4, cores = 4,
          seed = 7)

    ## Compiling the C++ model

    ## Start sampling

    ## Warning: There were 1 transitions after warmup that exceeded the maximum treedepth. Increase max_treedepth above 10. See
    ## http://mc-stan.org/misc/warnings.html#maximum-treedepth-exceeded

    ## Warning: Examine the pairs() plot to diagnose sampling problems

    b7.2 <-
      update(b7.1, # Not Afrikan data
             newdata = d.A0)

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.035 seconds (Warm-up)
    ## Chain 1:                0.045 seconds (Sampling)
    ## Chain 1:                0.08 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.025 seconds (Warm-up)
    ## Chain 2:                0.029 seconds (Sampling)
    ## Chain 2:                0.054 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.029 seconds (Warm-up)
    ## Chain 3:                0.063 seconds (Sampling)
    ## Chain 3:                0.092 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.027 seconds (Warm-up)
    ## Chain 4:                0.06 seconds (Sampling)
    ## Chain 4:                0.087 seconds (Total)
    ## Chain 4:

    ## Warning: There were 1 transitions after warmup that exceeded the maximum treedepth. Increase max_treedepth above 10. See
    ## http://mc-stan.org/misc/warnings.html#maximum-treedepth-exceeded

    ## Warning: Examine the pairs() plot to diagnose sampling problems

Plot posterior predictions

    africa <- d.A1 %>%
      ggplot(aes(x = rugged, y = log_gdp)) +
      stat_smooth(method = "lm", fullrange = T, size = 1/2,
                  color = "firebrick4", fill = "firebrick", alpha = 1/5) +
      geom_point(size = 1.5, color = "firebrick4", alpha = 1/2) +
      scale_x_continuous("Rugged", limits = c(0, 55)) +
      coord_cartesian(xlim = 0:7, ylim = 6:10) +
      ylab("Log of GDP") +
      theme_bw() + ggtitle("Africa") 
      
    not_africa <- d.A0 %>%
      ggplot(aes(x = rugged, y = log_gdp)) +
      stat_smooth(method = "lm", fullrange = T, size = 1/2,
                  color = "firebrick4", fill = "firebrick", alpha = 1/5) +
      geom_point(size = 1.5, color = "firebrick4", alpha = 1/2) +
      scale_x_continuous("Rugged", limits = c(0, 55)) +
      coord_cartesian(xlim = 0:7, ylim = 6:10) +
      ylab("Log of GDP") +
      theme_bw() + ggtitle("Not Africa")
      

    not_africa + africa

![](7---interactions_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    # slope is positive within africa but negative outside africa

It is better if we keep the data in the same data set. Multilevel models
borrow data from the different categories in order to improve estimates
in all categories - and many other reasons. But even if we added the
dummy variable cont\_africa, this will not solve the problem - as a
normal linear regression does not account for different slopes for the
categories of the dummy.

Comapre the models with/without the dummy cont\_africa as predictor ( but no interaction effect)
================================================================================================

    # Start with the model I made before (without the dummy cont_africa) - just with all the data
    b7.3 <-
      update(b7.1, newdata = dd) # all data

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.033 seconds (Warm-up)
    ## Chain 1:                0.073 seconds (Sampling)
    ## Chain 1:                0.106 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.055 seconds (Warm-up)
    ## Chain 2:                0.068 seconds (Sampling)
    ## Chain 2:                0.123 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.036 seconds (Warm-up)
    ## Chain 3:                0.047 seconds (Sampling)
    ## Chain 3:                0.083 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.053 seconds (Warm-up)
    ## Chain 4:                0.173 seconds (Sampling)
    ## Chain 4:                0.226 seconds (Total)
    ## Chain 4:

    # Now the same but added is the dummy as a predictor (in the formula)
    b7.4 <-
      update(b7.3,
             newdata = dd,
             formula = log_gdp ~ 1 + rugged + cont_africa) 

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.033 seconds (Warm-up)
    ## Chain 1:                0.065 seconds (Sampling)
    ## Chain 1:                0.098 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.044 seconds (Warm-up)
    ## Chain 2:                0.07 seconds (Sampling)
    ## Chain 2:                0.114 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.05 seconds (Warm-up)
    ## Chain 3:                0.039 seconds (Sampling)
    ## Chain 3:                0.089 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.044 seconds (Warm-up)
    ## Chain 4:                0.067 seconds (Sampling)
    ## Chain 4:                0.111 seconds (Total)
    ## Chain 4:

    # compute information criteria for both
    b7.3 <- add_criterion(b7.3, c("loo", "waic"))
    b7.4 <- add_criterion(b7.4, c("loo", "waic"))


    # compare IC
    loo_compare(b7.3, b7.4,
                criterion = "waic")

    ##      elpd_diff se_diff
    ## b7.4   0.0       0.0  
    ## b7.3 -31.6       7.3

    loo_compare(b7.3, b7.4,
                criterion = "loo")

    ##      elpd_diff se_diff
    ## b7.4   0.0       0.0  
    ## b7.3 -31.6       7.3

    # loo and waic agree - the model with the dummy fits teh data way better

    # compute weight
    model_weights(b7.3, b7.4,
                  weights = "waic") %>% 
      round(digits = 3)

    ## b7.3 b7.4 
    ##    0    1

    # The model with the dummy has all the weight :)

adding a linear interaction
===========================

We want the realtionship between Y (GDP) and R (rugged) to vary as a
function of A (dummy varible)

(the model comparisons do not work) add\_criterion() and
model\_weigths() when using brms.

    # Define new model with interaction effect 
    b7.5 <-
      update(b7.4,
             newdata = dd,
             formula = log_gdp ~ 1 + rugged*cont_africa) # syntax as lmer()

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.049 seconds (Warm-up)
    ## Chain 1:                0.042 seconds (Sampling)
    ## Chain 1:                0.091 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.045 seconds (Warm-up)
    ## Chain 2:                0.038 seconds (Sampling)
    ## Chain 2:                0.083 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.042 seconds (Warm-up)
    ## Chain 3:                0.036 seconds (Sampling)
    ## Chain 3:                0.078 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.047 seconds (Warm-up)
    ## Chain 4:                0.041 seconds (Sampling)
    ## Chain 4:                0.088 seconds (Total)
    ## Chain 4:

    b7.5b <-
      update(b7.4,
             newdata = dd,
             formula = log_gdp ~ 1 + rugged + cont_africa + rugged:cont_africa) 

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.044 seconds (Warm-up)
    ## Chain 1:                0.051 seconds (Sampling)
    ## Chain 1:                0.095 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.046 seconds (Warm-up)
    ## Chain 2:                0.042 seconds (Sampling)
    ## Chain 2:                0.088 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.04 seconds (Warm-up)
    ## Chain 3:                0.037 seconds (Sampling)
    ## Chain 3:                0.077 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL 'd17ad44a3d728c149e5d93af8572a666' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.043 seconds (Warm-up)
    ## Chain 4:                0.044 seconds (Sampling)
    ## Chain 4:                0.087 seconds (Total)
    ## Chain 4:

    # add IC 
    b7.5 <- add_criterion(b7.5,c("loo","waic"))

    # compare all 3 models
    l <- loo_compare(b7.3, b7.4, b7.5, criterion = "waic")

    print(l, simplify = F)

    ##      elpd_diff se_diff elpd_waic se_elpd_waic p_waic se_p_waic waic  
    ## b7.4    0.0       0.0  -238.1       7.4          4.2    0.8     476.2
    ## b7.5    0.0       0.0  -238.1       7.4          4.2    0.8     476.2
    ## b7.3  -31.6       7.3  -269.7       6.5          2.6    0.3     539.4
    ##      se_waic
    ## b7.4   14.8 
    ## b7.5   14.8 
    ## b7.3   13.0

    # weight
    model_weights(b7.3, b7.4, b7.5,
                  weights = "loo") %>% 
      round(digits = 3)

    ## b7.3 b7.4 b7.5 
    ##  0.0  0.5  0.5

    b7.5$loo # This is not working!

    ## NULL

Interpreting interaction effect
===============================

and compare and plot slopes for the dummy

    ## Parameter estimates 
    posterior_summary(b7.5)

    ##                          Estimate  Est.Error          Q2.5         Q97.5
    ## b_Intercept             9.1809931 0.13909665    8.90557865    9.45394597
    ## b_rugged               -0.1829584 0.07816325   -0.34318362   -0.03241465
    ## b_cont_africa          -1.8395136 0.22397838   -2.27921100   -1.39621838
    ## b_rugged:cont_africa    0.3457096 0.13109509    0.09285277    0.60121497
    ## sigma                   0.9515611 0.05309050    0.85710841    1.05976962
    ## Intercept               8.5174437 0.07428775    8.36785408    8.66370510
    ## lp__                 -244.4804325 1.60470370 -248.35558124 -242.33972537

    posterior_summary(b7.4)

    ##                    Estimate  Est.Error         Q2.5         Q97.5
    ## b_Intercept      9.01368556 0.12802960    8.7559086    9.26803037
    ## b_rugged        -0.06458226 0.06387331   -0.1905924    0.06374285
    ## b_cont_africa   -1.42826553 0.16373432   -1.7429859   -1.11287412
    ## sigma            0.97333402 0.05490910    0.8713517    1.09057768
    ## Intercept        8.51590909 0.07513785    8.3710069    8.66489299
    ## lp__          -246.62665845 1.43754735 -250.3551579 -244.82323804

    # Does not calculate the gamma (the slope conditioned on cont_africa). Therefore, we have to do it manually

    # within Africa
    fixef(b7.5)[2, 1] + fixef(b7.5)[4, 1] * 1

    ## [1] 0.1627512

    # outside Africa
    fixef(b7.5)[2, 1] + fixef(b7.5)[4, 1] * 0

    ## [1] -0.1829584

    #But those are only map values, from a sample of the posterior distribution we can get uncertainties around those MAP's
    post <- posterior_samples(b7.5) 

    post %>%
      transmute(gamma_Africa    = b_rugged + `b_rugged:cont_africa`, # based on computing gamma when A == 1
                gamma_notAfrica = b_rugged) %>% # based on computing gamma when A == 0 (and the interaction effect disappears)
      gather(key, value) %>%
      group_by(key) %>%
      summarise(mean = mean(value))

    ## # A tibble: 2 x 2
    ##   key               mean
    ##   <chr>            <dbl>
    ## 1 gamma_Africa     0.163
    ## 2 gamma_notAfrica -0.183

    # plot the distributions (of uncertainty around the MAP values - the slopes for the dummy)
    p_load(ggthemes)

    post %>%
      transmute(gamma_Africa    = b_rugged + `b_rugged:cont_africa`,
                gamma_notAfrica = b_rugged) %>%
      gather(key, value) %>%
      
      ggplot(aes(x = value, group = key, color = key, fill = key)) +
      geom_density(alpha = 1/4) +
      scale_colour_pander() +
      scale_fill_pander() +
      scale_x_continuous(expression(gamma), expand = c(0, 0)) +
      scale_y_continuous(NULL, breaks = NULL) +
      ggtitle("Terraine Ruggedness slopes",
              subtitle = "Blue = African nations, Green = others") +
      theme_pander() + 
      theme(text = element_text(family = "Times"),
            legend.position = "none")

    ## Warning in grid.Call(C_stringMetric, as.graphicsAnnot(x$label)): font
    ## family not found in Windows font database

    ## Warning in grid.Call(C_stringMetric, as.graphicsAnnot(x$label)): font
    ## family not found in Windows font database

    ## Warning in grid.Call(C_stringMetric, as.graphicsAnnot(x$label)): font
    ## family not found in Windows font database

    ## Warning in grid.Call(C_stringMetric, as.graphicsAnnot(x$label)): font
    ## family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call.graphics(C_text, as.graphicsAnnot(x$label), x$x,
    ## x$y, : font family not found in Windows font database

    ## Warning in grid.Call.graphics(C_text, as.graphicsAnnot(x$label), x$x,
    ## x$y, : font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

![](7---interactions_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    # nations outside Africa has a positive relationship between GDP and rugged, with a distribution of uncertainty around

    #what’s the probability (according to this model and these data) that
    #the slope within Africa is less than the slope outside of Africa?
    post %>%
      mutate(gamma_Africa    = b_rugged + `b_rugged:cont_africa`,
             gamma_notAfrica = b_rugged) %>% 
      mutate(diff            = gamma_Africa - gamma_notAfrica) %>% # what proportion of the differences (for each sample of the posterior) is below 0?
      summarise(Proportion_of_the_difference_below_0 = sum(diff < 0) / length(diff))

    ##   Proportion_of_the_difference_below_0
    ## 1                              0.00325

CONTINUOUS INTERACTIONS
=======================

load data

    # The data
    library(rethinking)

    ## rethinking (Version 1.60)

    ## 
    ## Attaching package: 'rethinking'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     map

    ## The following objects are masked from 'package:brms':
    ## 
    ##     LOO, stancode, WAIC

    data(tulips)
    d <- tulips
    head(d)

    ##   bed water shade blooms
    ## 1   a     1     1   0.00
    ## 2   a     1     2   0.00
    ## 3   a     1     3 111.04
    ## 4   a     2     1 183.47
    ## 5   a     2     2  59.16
    ## 6   a     2     3  76.75

    # done in online version
    detach(package:rethinking, unload = T)
    library(brms)
    rm(tulips)

making models with interaction to compare
-----------------------------------------

Making 2 models - one where the parameters interact and one without
interaction

    # Without
    b7.6 <-
      brm(data = d, family = gaussian,
          blooms ~ 1 + water + shade,
          prior = c(prior(normal(0, 100), class = Intercept),
                    prior(normal(0, 100), class = b),
                    prior(uniform(0, 100), class = sigma)),
          iter = 2000, warmup = 1000, cores = 4, chains = 4,
          seed = 7)

    ## Compiling the C++ model

    ## Start sampling

    ## Warning: There were 60 divergent transitions after warmup. Increasing adapt_delta above 0.8 may help. See
    ## http://mc-stan.org/misc/warnings.html#divergent-transitions-after-warmup

    ## Warning: Examine the pairs() plot to diagnose sampling problems

    # yieled divergent transitions --> update with better prior and 'control' element (whatever that means)

    b7.6 <-
      update(b7.6,
             prior = c(prior(normal(0, 100), class = Intercept),
                       prior(normal(0, 100), class = b),
                       prior(cauchy(0, 10), class = sigma)),
             control = list(adapt_delta = 0.9), # apparently good to add.
             seed = 7)

    ## The desired updates require recompiling the model

    ## Compiling the C++ model

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.15 seconds (Warm-up)
    ## Chain 1:                0.027 seconds (Sampling)
    ## Chain 1:                0.177 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.102 seconds (Warm-up)
    ## Chain 2:                0.028 seconds (Sampling)
    ## Chain 2:                0.13 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.129 seconds (Warm-up)
    ## Chain 3:                0.027 seconds (Sampling)
    ## Chain 3:                0.156 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.136 seconds (Warm-up)
    ## Chain 4:                0.025 seconds (Sampling)
    ## Chain 4:                0.161 seconds (Total)
    ## Chain 4:

    posterior_summary(b7.6) %>% round(digits = 2)

    ##             Estimate Est.Error    Q2.5   Q97.5
    ## b_Intercept    60.41     42.99  -23.19  145.14
    ## b_water        74.12     14.45   45.21  102.52
    ## b_shade       -40.75     14.47  -69.44  -11.95
    ## sigma          61.29      8.98   47.01   81.45
    ## Intercept     127.15     11.40  104.73  150.38
    ## lp__         -169.72      1.45 -173.28 -167.86

    # with interaction effect
    b7.7 <- 
      update(b7.6, 
             formula = blooms ~ 1 + water + shade + water:shade)

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.163 seconds (Warm-up)
    ## Chain 1:                0.065 seconds (Sampling)
    ## Chain 1:                0.228 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.224 seconds (Warm-up)
    ## Chain 2:                0.075 seconds (Sampling)
    ## Chain 2:                0.299 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.172 seconds (Warm-up)
    ## Chain 3:                0.067 seconds (Sampling)
    ## Chain 3:                0.239 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL '6fed7faf40078d989a7bbb43586ab9e7' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.182 seconds (Warm-up)
    ## Chain 4:                0.072 seconds (Sampling)
    ## Chain 4:                0.254 seconds (Total)
    ## Chain 4:

    posterior_summary(b7.6) %>% round(digits = 2)

    ##             Estimate Est.Error    Q2.5   Q97.5
    ## b_Intercept    60.41     42.99  -23.19  145.14
    ## b_water        74.12     14.45   45.21  102.52
    ## b_shade       -40.75     14.47  -69.44  -11.95
    ## sigma          61.29      8.98   47.01   81.45
    ## Intercept     127.15     11.40  104.73  150.38
    ## lp__         -169.72      1.45 -173.28 -167.86

    posterior_summary(b7.7) %>% round(digits = 2)

    ##               Estimate Est.Error    Q2.5   Q97.5
    ## b_Intercept    -106.98     62.34 -228.07   18.27
    ## b_water         159.44     29.06   98.42  214.77
    ## b_shade          43.49     28.87  -14.24   97.92
    ## b_water:shade   -42.81     13.46  -68.12  -15.75
    ## sigma            50.05      7.90   37.50   67.84
    ## Intercept       127.63      9.53  109.03  146.26
    ## lp__           -170.61      1.70 -174.91 -168.38

    # model comparison with WAIC
    b7.6 <- add_criterion(b7.6, "waic")
    b7.7 <- add_criterion(b7.7, "waic")

    w <- loo_compare(b7.6, b7.7, criterion = "waic")

    print(w, simplify = F)

    ##      elpd_diff se_diff elpd_waic se_elpd_waic p_waic se_p_waic waic  
    ## b7.7    0.0       0.0  -146.7       4.0          4.6    1.2     293.4
    ## b7.6   -5.3       2.7  -152.0       3.8          4.0    1.0     303.9
    ##      se_waic
    ## b7.7    8.0 
    ## b7.6    7.7

    # cbind() trick to convert from elpd metric to more traditional WAIC metric
    cbind(waic_diff = w[, 1] * -2,
          se        = w[, 2] *  2)

    ##      waic_diff       se
    ## b7.7   0.00000 0.000000
    ## b7.6  10.54751 5.379819

    # and weight 
    model_weights(b7.6, b7.7, weights = "waic") # almost all weight goes to interaction model.

    ##        b7.6        b7.7 
    ## 0.005098197 0.994901803

I do it after mean centering

    # mean center to two new columns
    # it is better to meancenter beacuse then the intercept will be the empirical mean - and the search when compiling the model will work better(i think)
    d <- d %>% mutate(
      water_c = water - mean(water),
      shade_c = shade - mean(shade)
    )

    # compile new model without interaction
    b7.8 <-
      brm(data = d, family = gaussian,
          blooms ~ 1 + water_c + shade_c,
          prior = c(prior(normal(130, 100), class = Intercept),# updating prior for intercept (after summary, I know the mean
                    prior(normal(0, 100), class = b),          # of outcome varible is around 130)
                    prior(cauchy(0, 10), class = sigma)),
          iter = 2000, warmup = 1000, chains = 4, cores = 4,
          control = list(adapt_delta = 0.9),
          seed = 7)

    ## Compiling the C++ model

    ## Start sampling

    # with interaction
    b7.9 <- 
      update(b7.8, 
             formula = blooms ~ 1 + water_c + shade_c + water_c:shade_c)

    ## Start sampling

    ## 
    ## SAMPLING FOR MODEL '2b3dbf52b7a9e56421baf65d2fddd1e4' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 0 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.151 seconds (Warm-up)
    ## Chain 1:                0.031 seconds (Sampling)
    ## Chain 1:                0.182 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL '2b3dbf52b7a9e56421baf65d2fddd1e4' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 0 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.143 seconds (Warm-up)
    ## Chain 2:                0.03 seconds (Sampling)
    ## Chain 2:                0.173 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL '2b3dbf52b7a9e56421baf65d2fddd1e4' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 0 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.145 seconds (Warm-up)
    ## Chain 3:                0.035 seconds (Sampling)
    ## Chain 3:                0.18 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL '2b3dbf52b7a9e56421baf65d2fddd1e4' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 0 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.14 seconds (Warm-up)
    ## Chain 4:                0.042 seconds (Sampling)
    ## Chain 4:                0.182 seconds (Total)
    ## Chain 4:

    # Results
    posterior_summary(b7.8) %>% round(digits = 2)

    ##             Estimate Est.Error    Q2.5   Q97.5
    ## b_Intercept   128.80     11.39  105.98  151.15
    ## b_water_c      74.50     14.66   45.58  102.87
    ## b_shade_c     -40.65     14.10  -68.06  -12.89
    ## sigma          61.28      9.21   46.65   81.92
    ## Intercept     128.80     11.39  105.98  151.15
    ## lp__         -168.91      1.46 -172.59 -167.06

    posterior_summary(b7.9) %>% round(digits = 2)

    ##                   Estimate Est.Error    Q2.5   Q97.5
    ## b_Intercept         129.05      9.82  109.39  148.30
    ## b_water_c            74.99     11.91   50.81   98.36
    ## b_shade_c           -41.03     11.63  -63.89  -18.68
    ## b_water_c:shade_c   -51.80     14.23  -79.88  -23.94
    ## sigma                49.65      7.52   37.77   67.40
    ## Intercept           129.05      9.82  109.39  148.30
    ## lp__               -168.59      1.77 -173.08 -166.27

    # because we have mean centered the predictor variables,
    # the intercept becomes the grand mean of the outcome



    # Model comparison 
    b7.8 <- add_criterion(b7.8, "waic")
    b7.9 <- add_criterion(b7.9, "waic")

    h <- loo_compare(b7.8, b7.9, criterion = "waic")

    print(h, simplify = F)

    ##      elpd_diff se_diff elpd_waic se_elpd_waic p_waic se_p_waic waic  
    ## b7.9    0.0       0.0  -146.6       4.1          4.7    1.2     293.2
    ## b7.8   -5.4       3.3  -152.0       3.8          4.0    1.0     303.9
    ##      se_waic
    ## b7.9    8.1 
    ## b7.8    7.7

    # cbind() trick to convert from elpd metric to more traditional WAIC metric
    cbind(waic_diff = w[, 1] * -2,
          se        = w[, 2] *  2)

    ##      waic_diff       se
    ## b7.7   0.00000 0.000000
    ## b7.6  10.54751 5.379819

    # and weight 
    model_weights(b7.8, b7.9, weights = "waic") # most weight ascribed to b7.9

    ##        b7.8        b7.9 
    ## 0.004666002 0.995333998

    # comparative summary of the 2 models
    tibble(model  = str_c("b7.", 8:9)) %>% 
      mutate(fit  = purrr::map(model, get)) %>% 
      mutate(tidy = purrr::map(fit, broom::tidy)) %>% 
      unnest(tidy) %>% 
      filter(term != "lp__") %>% 
      select(term, estimate, model) %>% 
      spread(key = model, value = estimate) %>% 
      mutate_if(is.double, round, digits = 2)

    ## # A tibble: 6 x 3
    ##   term               b7.8  b7.9
    ##   <chr>             <dbl> <dbl>
    ## 1 b_Intercept       129.  129. 
    ## 2 b_shade_c         -40.6 -41.0
    ## 3 b_water_c          74.5  75.0
    ## 4 b_water_c:shade_c  NA   -51.8
    ## 5 Intercept         129.  129. 
    ## 6 sigma              61.3  49.6

Plot implied predictions - TRIPTYCH (good way to plot interactions)

    # three plot made to be viewed together


    # loop over values of water_c and plot predicitons
    shade_seq <- -1:1

    for(w in -1:1) {
      # define the subset of the original data
      dt <- d[d$water_c == w, ]
      # defining our new data
      nd <- tibble(water_c = w, shade_c = shade_seq)
      # use our sampling skills, like before
      f <- 
        fitted(b7.9, newdata = nd) %>%
        as_tibble() %>%
        bind_cols(nd)
      
      # specify our custom plot
      fig <- 
        ggplot() +
        geom_smooth(data = f,
                    aes(x = shade_c, y = Estimate, ymin = Q2.5, ymax = Q97.5),
                    stat = "identity", 
                    fill = "#CC79A7", color = "#CC79A7", alpha = 1/5, size = 1/2) +
        geom_point(data = dt, 
                   aes(x = shade_c, y = blooms),
                   shape = 1, color = "#CC79A7") +
        coord_cartesian(xlim = range(d$shade_c), 
                        ylim = range(d$blooms)) +
        scale_x_continuous("Shade (centered)", breaks = c(-1, 0, 1)) +
        labs("Blooms", 
             title = paste("Water (centered) =", w)) +
        theme_pander() + 
        theme(text = element_text(family = "Times"))

      # save to plotting
      if (w == -1){
        fig_1 <- fig
      } else if (w == 0){
        fig_2 <- fig
      } else {
        fig_3 <- fig
      }

    }

    # Using patchwork to plot them next to eachother
    fig_1 + fig_2 + fig_3

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call.graphics(C_text, as.graphicsAnnot(x$label), x$x,
    ## x$y, : font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call.graphics(C_text, as.graphicsAnnot(x$label), x$x,
    ## x$y, : font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

    ## Warning in grid.Call.graphics(C_text, as.graphicsAnnot(x$label), x$x,
    ## x$y, : font family not found in Windows font database

    ## Warning in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y, :
    ## font family not found in Windows font database

![](7---interactions_files/figure-markdown_strict/unnamed-chunk-11-1.png)
