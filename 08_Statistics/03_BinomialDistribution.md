## Summary The binomial distribution | Theory

- The binomial distribution models probabilities of success in a sequence of independent events, like coin flips.
- Coin flips showcase binary outcomes with equal probabilities for each event.
- It's a discrete distribution with parameters: n (number of events) and p (probability of success).
- The distribution graph for 10 coin flips peaks at five heads, with smaller chances for extremes like zero or ten heads.
- To find probabilities, add individual outcomes, like the chance of getting seven or fewer heads in 10 flips (94.5%).
- Conversely, the probability of eight or more heads is obtained by subtracting from 100% (5.5%).
- Expected value is found by multiplying n by p; for 10 flips, it's 5 heads. p can be derived if n and expected value are known.
- Independence is crucial; events must not affect each other for the binomial distribution to apply.
- Sampling without replacement alters probabilities between events, invalidating the binomial distribution's accuracy.
- It's useful in diverse scenarios: clinical trials, sports betting—where binary outcomes occur independently.

## Summary The normal distribution | Theory

- The normal distribution is a fundamental continuous probability distribution, widely applicable in statistics.
- Its shape resembles a bell curve and is symmetrical, with the left being a mirror image of the right.
- The area beneath the curve equals one, and the probability never truly reaches zero.
- Described by mean and standard deviation, it maintains the same shape but with different scales.
-  Within one, two, and three standard deviations of the mean lie approximately 68%, 95%, and 99.7% of the distribution, respectively (the 68-95-99.7 rule).
- Real-world data often mimics the normal distribution, impacting statistical hypothesis testing.
- Skewness indicates the direction data tails off, while kurtosis describes extreme values in the distribution. Positive skewness peaks on the left and tails off to the right, while negative skewness is the opposite.
- Kurtosis also categorizes distributions: positive (leptokurtic), normal (mesokurtic), and negative (platykurtic), based on peak and standard deviation.

## Summary The central limit theorem | Theory
- Central Limit Theorem Overview
  - Understanding the significance of the normal distribution.
-Rolling a Die
  - Taking means from multiple sets reveals varying averages.
- Multiple Set Analysis
  - Repeating the process multiple times demonstrates different mean values for each set.
- 10 Sets of Five Die Rolls
  - Conducting 10 sets of five die rolls provides a range of mean values.
- Sampling Distributions
  - Plotting means leads to sampling distributions, specifically of the sample mean.
- Multiple Sampling Rounds
  - Iterating through 100, 1000, 10,000, 100,000, and 1 million samples highlights the convergence to a normal distribution.
- Central Limit Theorem's Definition
  - Explaining the central limit theorem: the shift towards normality as sample size increases. Criteria include randomness and independence.
- Standard Deviation and the CLT
   - Applying the CLT to standard deviation, showing how the distribution of sample standard deviations behaves.
- Proportions and the CLT
  - Observing how the CLT applies to proportions using dice roll examples.
- Sampling Distribution of Proportion
  - Visualizing the distribution of fours rolled in each sample, resembling a normal distribution.
- Mean of Sampling Distribution
  - Understanding how mean estimates are derived from normal sampling distributions.
- Benefits of the CLT
  - Utilizing the CLT for estimation in large populations, saving time and resources in data collection.



## Summary The Poisson distribution | Theory
The Poisson distribution characterizes the likelihood of a specific number of events occurring within a fixed time or space, rooted in the concept of Poisson processes and governed by the parameter lambda ($\lambda$).

### Facts
- Poisson Processes: Events occurring with a known average but random time or space intervals, evident in scenarios like animal adoptions, restaurant arrivals, or website visits.
- Poisson Distribution: Describes the probability of events within a fixed timeframe, enabling calculations for various scenarios like adoption rates or patron arrivals.
- Lambda ($\lambda$): Represents the average event count, shaping the distribution and acting as its peak or most probable value.
- Lambda's Impact: Changes in lambda alter the distribution's shape while maintaining its peak at the lambda value.
- Central Limit Theorem: Applies to Poisson distributions; with a large sample size, sample means form a normal distribution.
- Probability Calculations: Determining probabilities for specific event counts, like the likelihood of a precise number of patrons or occurrences above a certain count, involves measuring bar heights within the distribution.

