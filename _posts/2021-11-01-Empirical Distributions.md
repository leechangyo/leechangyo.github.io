---
layout: post
title: Empirical Distributions(经验分布函数)
category: calculus
tag: calculus
---

经验分布函数
https://machinelearningmastery.com/empirical-distribution-function-in-python/
https://kjhov195.github.io/2019-12-28-edf/
https://www.statlect.com/asymptotic-theory/empirical-distribution
https://towardsdatascience.com/what-why-and-how-to-read-empirical-cdf-123e2b922480
https://rdrr.io/cran/spatstat.geom/man/ewcdf.html

The empirical distribution, or empirical distribution function, can be used to describe a sample of observations of a given variable. Its value at a given point is equal to the proportion of observations from the sample that are less than or equal to that point.


An empirical distribution function provides a way to model and sample cumulative probabilities for a data sample that does not fit a standard probability distribution.

As such, it is sometimes called the empirical cumulative distribution function, or ECDF for short.

why use

Some data samples cannot be summarized using a standard distribution.

Empirical Distribution Function

Typically, the distribution of observations for a data sample fits a well-known probability distribution.

For example, the heights of humans will fit the normal (Gaussian) probability distribution

This is not always the case. Sometimes the observations in a collected data sample do not fit any known probability distribution and cannot be easily forced into an existing distribution by data transforms or parameterization of the distribution function.

Instead, an empirical probability distribution must be used.

There are two main types of probability distribution functions we may need to sample; they are:

Probability Density Function (PDF).
Cumulative Distribution Function (CDF).

The PDF returns the expected probability for observing a value. For discrete data, the PDF is referred to as a Probability Mass Function (PMF). The CDF returns the expected probability for observing a value less than or equal to a given value.

An empirical probability density function can be fit and used for a data sampling using a nonparametric density estimation method, such as Kernel Density Estimation (KDE).

The EDF is calculated by ordering all of the unique observations in the data sample and calculating the cumulative probability for each as the number of observations less than or equal to a given observation divided by the total number of observations.

As follows:
 - EDF(x) = number of observations <= x / n

Like other cumulative distribution functions, the sum of probabilities will proceed from 0.0 to 1.0 as the observations in the domain are enumerated from smallest to largest.


-- using cumulative distribution function
Use the CDF to determine the probability that a random observation that is taken from the population will be less than or equal to a certain value. You can also use this information to determine the probability that an observation will be greater than a certain value, or between two values.

The cumulative distribution function is illustrated in Figure 20.4(b). It shows that the probability of X being less than or equal to xl is FX(xl)

using this is for reading more easily on the probabilty of dataset cumulateively.

in my project case, using ecdf, get probability of some specific data in gaussian distributiuon of ellip submap(check, probabiltiy how much closed ellipes centor or not)

probability(error) -> update weight -> update mean  

https://support.minitab.com/en-us/minitab-express/1/help-and-how-to/basic-statistics/probability-distributions/supporting-topics/basics/using-the-cumulative-distribution-function-cdf/
