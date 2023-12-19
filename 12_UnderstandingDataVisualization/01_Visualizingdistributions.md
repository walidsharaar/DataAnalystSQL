## Summary A plot tells a thousand words | Theory
- Introduction: Richie introduces the course on understanding plots and their significance in interpreting diverse datasets.
- Learning Objectives: Learn to choose appropriate plots for different data types, interpret common plot types (histograms, scatter plots, etc.), and grasp best practices while avoiding common errors.
- Insight Approaches: Three ways to understand datasets - summary statistics (quantity and variation), statistical modeling (linear, logistic regression), and visualizing data through plots, with a focus on the latter in the course.
- Datasaurus Dozen: A set of 13 datasets with x and y coordinates illustrating that despite identical statistical measures (means, standard deviations), their plots are vastly different.
- Statistical Consistency: Despite identical means and standard deviations for x and y values across datasets, the visual representation starkly differs.
- Importance of Plots: Visualizing datasets through scatter plots highlights their diversity, showcasing the significance of plotting data for comprehensive understanding.
- Data Types: Acknowledgment of continuous (numeric) and categorical (classified, textual) variables, with examples like heights for continuous and eye color for categorical.
- Dual Nature of Variables: Explanation that certain variables can be treated as either continuous or categorical, such as age (continuous) or age groups (categorical), depending on the context and question posed.

## Histograms | Theory
- Histograms are visual representations used to understand the distribution, modality, skewness, and kurtosis of data. They assist in analyzing data patterns through binning continuous variables.

### Facts
- Histograms are useful for understanding the distribution shape of a continuous variable.
- The dataset examines the ages of English and British monarchs when they ascended to the throne, revealing insights into their distributions.
- The x-axis represents age intervals (bins), and the y-axis displays the count of monarchs within each age range.
- Altering the binwidth significantly impacts the histogram's insight, with wider bins causing noise and narrower ones losing detail.
- Determining the optimal binwidth often requires experimentation with various values.
- The modality of a distribution signifies the number of peaks, categorizing distributions as unimodal, bimodal, etc.
- Skewness indicates the symmetry of a distribution, with left-skewed distributions having outliers on the left and vice versa.
- Assessing skewness helps identify whether a distribution is symmetric or not.
- Kurtosis measures the number of extreme values in a distribution, distinguishing between mesokurtic, leptokurtic, and platykurtic distributions.
- Analyzing kurtosis aids in understanding the distribution's peak and the frequency of extreme values.


## Box plots | Theory
- Box plots offer a space-efficient way to compare distributions of a continuous variable across different categories, aiding in visualizing trends and comparisons.

### Facts
- Individual histograms pose challenges when comparing multiple distributions.
- Drawing histograms in separate panels aids comparison, yet aligning them in a single column can lead to space issues and readability problems.
- Box plots split data by category, displaying quartiles, medians, and outliers efficiently.
- Comparing histograms with box plots reveals clearer insights, showcasing median ages and quartile ranges.
- Box plot whiskers extend to 1.5 times the interquartile range, encompassing most data points while indicating outliers.
- Comparing royal houses' monarch ascension ages through box plots reveals historical trends, displaying a gradual increase in ascension ages over time.

## Scatter plots | Theory
- Scatter plots are essential for visualizing the relationship between two continuous variables, offering insights into correlations and trends.

### Facts
- Scatter plots are used to explore the relationship between two continuous variables.
- Los Angeles County home price dataset includes bedrooms, sale prices, and square footage.
- Logarithmic scales in scatter plots help evenly distribute data points for better readability.
- Correlation in scatter plots indicates the strength and direction of relationships between variables.
- Sometimes, complex data shapes may not align with traditional correlation interpretations.
- Adding trend lines (straight or smooth curves) aids in understanding relationships between variables in scatter plots.
