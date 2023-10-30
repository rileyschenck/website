---
title: "Investigating Sacramento Campaign Finance Data with Streamlit"
date: 2023-08-03
published: true
tags: [ Sacramento, campaign finance, contributions, altair, charts]
excerpt: "Designed to empower journalist investigations with powerful filtering, aggregation, and visualization tools"

toc: true
toc_sticky: true
---
 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/intro.png)

My [Streamlit app](https://sac-campaign-finance-bnxtfmrjdaslx5nyst3fzn.streamlit.app/) aggregates public campaign finance disclosures and allows users to sift through and analyze the data with a high degree of granularity to gain insights into how our local political campaigns are financed. Here is a quick tutorial of how you may use the tool:

Visualizing the full dataset while aggregating the bar chart on "Campaigns/PACs-all years" shows the most expensive campaigns from 2014-23. We can see that "Steinberg for Sacramento Mayor" has raked in more cash than any other campaign with $3.2 million in total contributions. You may notice that the 14th most expensive campaign is also in support of Steinberg, "Mayor Darrell Steinberg Committee for Sacramento's Future." Let's dive a little deeper into these two campaigns. 

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg1.png)

We can filter the dataset by "Steinberg for Sacramento Mayor" in the "Campaigns/PACs-all years" field on the left to investigate the contributions made to that particular campaign accross all years. Aggregating the bar chart based on "Contributor Last Name" reveals that campaign's top contributors which are mostly trade unions, professional associations, and other special interest groups. However, the top congributor by aggregated last name is "Barkett" with a total of $23,000.

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg2.png)
 
If we also filter "Contributor Last Name" by "Barkett" we can see in the resulting filtered dataset that the "Barkett" contributions correspond to a family from La Jolla, California:  

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg3.png)

Toggling the campaign's contributions by date instead of year reveals that interestingly, $1.4 million out of the campaign's $3.2 million total came on a single day: March 22, 2016:

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg4.png)

When filtering for "Mayor Darrell Steinberg Committee for Sacramento's Future" we can see that despite totalling less than a third of the amount raised for "Steinberg for Sacramento Mayor," the total contributions by last name to this committee are much larger, with "Members' Voice of the State Building and Construction Trades Council of California" as the top contributor at $110,722. 

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg5.png)

Filtering the entire dataset for "Members' Voice of the State Building and Construction Trades Council of California" under "Contributor Last Name" shows that the committee has only ever contributed to Steinberg's mayoral campaign, campaigns in support of local ballot measures A and U, and to a campaign in opposition to measure G. 

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg6.png)

We can see that the "Contributor Type" for contributors to "Mayor Darrell Steinberg Committee for Sacramento's Future" are most frequently other committees, 

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg8.png)

Whereas the "Contributor Type" for "Steinberg for Sacramento Mayor" is most frequently "other."

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/steinberg7.png)

 You may find my public Github repository for the project [here](https://github.com/rileyschenck/sac-campaign-finance)
