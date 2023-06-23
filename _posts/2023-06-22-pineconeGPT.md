---
title: "Analyzing the messaging found in Tucker Carlson's monologues using Chat-GPT"
date: 2023-03-03
published: true
tags: [dataviz]
excerpt: "Querying a database of 918 monologues using Pinecone and Chat-GPT"
toc: true
toc_sticky: true
read_time: false
---
## Querying a database of 918 Tucker Carlson monologues using Pinecone and Chat-GPT
The monologues were scraped from Fox News' website and cover a period from November 27, 2018 to March 16, 2023. They were sliced into 1,000 character chunks using LangChain's RecursiveCharacterTextSplitter, which were then turned into embeddings using OpenAIEmbeddings, and then stored as vectors in Pinecone to be queried using simility search.

### Using GPT to query Carlson's monologues about about common Pew Polling issues:

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t1.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t2.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t3.png)

### (TRIGGER WARNING) Summarizing some of the monologues' most controversial views  
Can help us understand the messaging on these issues that a segment of the Republican base unfortunately agrees with. 

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t4.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t5.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t6.png)

### (TRIGGER WARNING) Using the angriest pieces of context from Tucker Carlson's monologues to highlight the anger and vitriol used in his messaging

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/angry1.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/angry2.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/angry3.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/angry4.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/angry5.png)

### In the world of Carlson, everything is a conspiracy and nobody is to be trusted! Except the conspiracy theorists, of course...

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/jan6.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/c1.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/c2.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/c3.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/ctheorists.png)

### Angriest pieces of context about "White men" output shows how Carlson plays on feelings of anger, racism, and fear using sarcastic straw man arguments meant to inflame while talking about issues sorrounding race.

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t7.png)



