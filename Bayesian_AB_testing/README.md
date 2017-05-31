# Bayesian A/B Testing

A/B testing is an extremely common and useful method for optimizing content, such as websites or marketing emails.  

Typically, A/B test results are evaluated using traditional, Frequentist hypothesis testing (or, perhaps without any statistical method other than comparing total results).  Arguably, Bayesian hypothesis testing is more appropriate for A/B tests, since the evaluation is *based on the actual data*.  Arguably, Bayes is more appropriate for everything.

This mini project is an implementation of a Bayesian A/B test using the PyMC library for Python.  The beauty of PyMC is it allows you to conduct the Markov Chain Monte Carlo simulation needed to evaluate your data with just a few lines of code.

The code and results can be viewed in a [jupyter notebook on my Github profile](https://github.com/MikeS-nth/portfolio/blob/master/Bayesian_AB_testing/Bayesian_AB_testing.ipynb).
