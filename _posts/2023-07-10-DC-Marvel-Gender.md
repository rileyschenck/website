---
title: "Visualizing Gender Representation in DC and Marvel Comics"
date: 2023-07-10
published: true
tags: [ marvel, dc, gender, altair]
excerpt: "Throughout comic book history there are fewer 'good' female characters compared to men and even fewer 'bad' female characters."
altair-loader:
  altair-chart-1: "charts/DC-Marvel-stacked-Altair.json"
  altair-chart-2: "charts/DC-Marvel-Altair.json"
  
toc: true
toc_sticky: true
---
<div id="altair-chart-1"></div>
## Lots of men and traditional genders

As you can see in the interactive stacked bar chart above, only a few DC or Marvel characters fail to conform to 
traditional gender roles (grouped into the Non-Traditional or Not Identified Characters bars), and there have been more than 
double the number of appearances by male characters compared to female characters in both comic series according to data compiled by 
site [FiveThirtyEight from Marvel Wikia and DC Wikia](https://github.com/fivethirtyeight/data/tree/master/comic-characters). 

If you hover your mouse over any part of the stacked bar chart you can see information on each character grouped by their "alignment" (whether they are good, bad, or neutral). The characters with the most appearances appear at the bottom of each stacked bar chart, and the characters with the fewest appearances are at the top of each category in the stacked bar chart.

## Exploring DC and Marvel Characters' history, filtering by gender and alignment (good vs bad)

Let's eliminate the neutral characters and focus on our heroes and villians.

The following interactive altair chart allows you to view all the DC and Marvel characters released over time, where the x-axis
shows the release date of each character, and the y-axis shows the total number of appearances by the character in their 
respective comic. 

You can filter for male and female characters by clicking on one of the red or green squares inside the legend on the upper right
hand corner of the chart. 

Hovering over a specific point shows you that character's name, total number of appearances, and alignment.

Scrolling up with your mouse inside one of the two charts allows you to zoom in on the chart.

<div id="altair-chart-2"></div>
 
## Takeaways

- There are far more male characters than female characters, especially among the characters that have appeared most in each comic.

- The most popular good characters appear many more times than the most popular bad characters.

- If you zoom in on the charts filtering for bad men vs bad women, it's apparent that the gender disparity is especially pronounced 
among the villians, with male villians being far more likely.




