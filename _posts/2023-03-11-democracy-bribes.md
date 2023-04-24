---
title: "Democracy and Corruption"
date: 2023-03-11
published: true
tags: [dataviz]
excerpt: "Comparing democracy duration to the percentage of people who report paying bribes"
toc: true
toc_sticky: true
read_time: false
---
## Comparing democracy duration to the percentage of people who report paying bribes

I investigated the relationship between how long a country has or hasn't been a democracy and the percentage of people in
that country that report paying a bribe over the past year. After combining a dataset from Harvard that codes the democratic histories of countries and a dataset from
Transparency International that records the percentage of people who report paying a bribe in the last year my resulting dataset contains 97 data points of countries 

A scatterplot would be the most obvious visualization technique to use since 97 different data points can be plotted along the x and y axes that
represent my 2 dependent and independent variables, with color coding used for the third variable of democracy vs non-democracy, however, a more novel visualization
would add to such a scatterplot with the visual representation of the linear regressions of the x and y variables for both groups (democracies vs nondemocracies) to compare the correlation between the dependent and independent variables for the democracy and non-democracy groups. IUsing the
Seaborn library's jointplot I can include side plots that are histograms that show the distributions of the x and y attributes in what is called linear
regression with marginal distribution plots.

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/fig1.png)

We see that there is a negative correlationbetween how long a democratic government has been in power and bribe prevelence, whereas there is a slight positive correlation between bribe prevelance and the duration of non-democratic governments, suggesting that democracy may be helplful in lowering the prevelence of bribes in a country.

We can see that the top 10 countries that report the fewest bribes are mostly democratic countries:
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/Top 10 least corrupt.png)

Whereas the top 10 countries with the highest percentage of people reporting paying bribes in the last year are mostly non-democratic countries:
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/Top 10 most corrupt based on percentage that report paying bribe.png)

If we eliminate the outlier of India which has the highest percentage of people who report paying a bribe in the last year out of all countries, we then see an even stronger negative correlation between the amount of time that a country has been a democracy and the percentage who report paying bribes. Clearly though there are many other, stronger factors at play that determine the prevelance of bribes in a country, as some new democracies have relatively low levels of bribe paying, as do many non-democracies.  

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/noindia.png)
