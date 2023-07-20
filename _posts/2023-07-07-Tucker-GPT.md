---
title: "Building a Tucker Carlson bot using LangChain, Pinecone, Chat-GPT, and 813 Carlson transcripts"
date: 2023-07-07
published: true
tags: [dataviz]
excerpt: "As simple as querying a database of Tucker Carlson's monologues from his Fox News show with Chat-GPT"
toc: true
toc_sticky: true
read_time: false
---
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/scaredtucker.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/replacement.png)

Are you someone who misses their nightly dose of hyperbolic and conspiratorial raving from Tucker Carlson on Fox News? Well, what if I were to tell you that the excerpt above of Tucker ranting about his infamous "replacement theory" was actually not an excerpt at all, but Chat-GPT imitating Mr. Carlson based on his own monologues? That's right, with a little modern magic we can actually create a realistic-sounding Tucker Carlson bot who will rant and rave for as long as your heart desires about any and all of the hot-button topics of the day, just like the good 'ol times! 

I will take you step by step through the process of how you can query and ask questions of Carlson's transcripts, or any document for that matter. You can follow along and run all the code yourself using [the Jupyter Notebook version of this post.](https://github.com/rileyschenck/tuckercarlson/blob/main/tucker_generator.ipynb)

### First, a quick overview:
Using LangChain, Pinecone, and Chat-GPT to query any documents you have is actually remarkably easy. The web scraping I did to just get the transcripts was the most complicated bit of coding in this process BY FAR. The monologue transcripts from Tucker Carlson Tonight were scraped from Fox News' website and cover a period from November 27, 2018 to March 16, 2023. I show the code below for how I scraped the transcripts using Selenium and Beautiful Soup. Feel free to skip over that section if you want to get straight to the nitty gritty of using LangChain and Pinecone to query the transcripts with Chat-GPT. 

Steps:
1. Scrape the transcripts (skip if you like).
2. Create a single large document that contains all of the Carlson monologue transcripts that I scraped from the web.
3. Use Langchain's recursive character splitter to break the document into chunks.
4. Use OpenAIEmbeddings to turn each chunk of text into a vector.
5. Store the vectors on Pinecone and allow for similarity search between them.
6. Query the transcripts using Chat-GPT, providing the 20 most relevant chunks of text from the transcripts as context.
7. Tweak your prompts for Chat-GPT (prompt engineering)

### 1. Web scraping 831 Tucker Carlson monologues from Fox News' website
Edit: I originally scraped these transcripts for a class project on March 16, 2023 before Carlson's Fox News show was cancelled. This code no longer works since Fox News has removed the following page that included links to all of Carlson's available monologues and transcripts: [https://www.foxnews.com/category/shows/tucker-carlson-tonight/transcript](https://www.foxnews.com/category/shows/tucker-carlson-tonight/transcript)

Comments explain how I scraped the monologues from Fox's website:
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/webscrape1.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/webscrape2.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/webscrape3.png)

The resulting dataframe containing each Carlson monologue's URL, the date it was published (timestamp), it's title, and the text of the transcript:
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/dataframe.png)

### 2. Combine texts into a single document 
If we want to be able to allow Chat-GPT to query all the transcripts simultaneously we will need to combine them into a single, long document.
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/combine.png)

### 3. Use Langchain's recursive character splitter to break the document into chunks 
The splitter simply slices the entire document we've created of all the transcripts into 1,000 character chunks of text (about 150 words per chunk). We can make the chunks as large or small as we like, but we want to make sure the chunks are large enough to provide relevant context, while at the same time not being so large that we lose precision. For example, if we ask Chat-GPT what Carlson's monologues say about California, we want the similarity search to pull in the most relevant examples of Carlson ranting about California and provide them as context to Chat-GPT. If the chunks are too small we may miss lots of relevant context, because Carlson may only mention California when he first starts speaking about it, and if the chunk ends before he finishes we will lose any additional context that's not included in that chunk.

On the other hand, if the chunks are too large we may include irrelevant parts of text where Carlson isn't talking about California, and since we only get about 3,000 words of context to provide Chat-GPT, we definitely don't want to waste valuable context space on text that is irrelevant to our query. As you will see below, when the chunks are set to 1,000 characters, we get a maximum of 20 pieces of context to provide Chat-GPT, or in the context of our task, just 20 examples of Carlson talking about California, Democrats, Trump, or whatever we want our Carlson bot to talk about. 

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/recursive.png)

### 4. Use OpenAIEmbeddings to turn each chunk of text into a vector.
If you don't know what word embeddings are, basically it's code for language, where words and their relationships to each other are all mapped as numbers, so when you "embed" text as a vector, you are converting that text into a bunch of numbers that correspond to its semantic meaning.

With OpenAI's embeddings, our text is going to be represented by 1,536 numbers, regardless of whether it's two words long like the example below of "Hello world," or our 150 word chunks of text. As you can see in the code below, the length of the vector/embedding we created from "Hello world" is indeed 1,536 numbers long, and I've printed out the first 5 of those numbers as well.

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/embeddings.png)

### 5. Store the vectors on Pinecone as an index and allow for similarity search between them.
We are going to have a whooooole bunch of vectors (one 1,536 number long vector like the one above for each 150 word chunk of text) and we will need to store them somewhere, and Pinecone is great for that. There are many tutorials online on how to set up an account and get started with your first Pinecone project (it's literally just a few clicks).

To create our index of vectors in Pinecone, we use the Pinecone.from_documents method, pass in our docs (the 150 word transcript chunks), our embedding object we created that will turn those chunks into numerical vectors, and the name of our index we've set up in Pinecone.

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/pinecone2.png)

Then, we define a little function that uses Pinecone's similarity search method to find the top k vectors (the 'k' parameter, in our case the top 20 chunks of text) that have the closest cosine similarity (the angle between two vectors) to the vector created from the query that we will also be turned into a vector with the same OpenAI embedding method (the 'query' parameter). 

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/similarity.png)

### 6. Query the transcripts using Chat-GPT

And with that we are all set to query our chunks of Tucker Carlson transcripts, adjusting our prompts to get Chat-GPT to take the pieces of provided context and immitate their style and substance to make a Tucker Carlson bot.

A couple things of note: 

Sometimes the pieces of context provided to Chat GPT are clearly moments when Carlson is being sarcastic, which he oftentimes is, especially when creating bombastic straw men of what liberals supposedly say and think which explains some unexpected outputs, for example when asked to immitate the tone and language of the angriest pieces of context when talking about white men or Russia.

Also, as a chatbot, Chat-GPT saves context from your previous queries which may influence its response to your current query unless you create a new instance of the QAChain every time you want to ask a question without effectively simulating a 'fresh start' for each query by starting the chat over. To achieve this you can place the chain object inside the get_answer() function below by uncommenting the line that begins with "chain." 

 ![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/queryupdate.png)

 
### 7. Tweak your prompts for Chat-GPT (prompt engineering) to create an angry Tucker Carlson bot

By tweaking our prompt that we input as our query, we can effectively "train" a bot that imitates the most inflamatory messaging seen in Tucker Carlson's monologues for any topic that is discussed in the transcripts. Instead of having to hire an army of online trolls or train their own advanced large language models, bad actors that are trying to flood social media with biased posts hoping to inflame divisions or influence voter behavior will only have to provide relavant context to one of the multitude of freely available and ever-improving free open source models to instruct it how it should respond to any topic of conversation. I of course am not encouraging people to do this, but rather highlighting what I believe is a dangerous new front in the battle against malicious influence campaigns on social media.

#### (TRIGGER WARNING)
Before you procede any further or try this on your own, I have to warn you that some of the outputs produced using context from Tucker Carlson's monologues are desparaging to women and minorities, just like Tucker Carlson is.

I first tried to get Chat-GPT to adopt the voice of Carlson by asking it to immitate the style and substance of the pieces of context about a particular topic, in this case Democrats:
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/dems1.png)

While that does sound a bit like Carlson, I wanted to see if it could identify the vitriol often used in his messaging by asking it to immitate the style and substance of the "angriest" pieces of context: 
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/dems2.png)

You may be tempted to think that this is too much, Chat-GPT must be putting words in Carlson's mouth, surely he didn't actually say that Democrats are "the party of neurotic, personally unsatisfied White ladies," well think again because that is a direct quote from a piece of context found in [this December 21, 2021 monologue:](https://www.foxnews.com/opinion/tucker-carlson-biden-covid-policies)

>They're the party of neurotic, personally unsatisfied White ladies who live in the suburbs. You know, the pretty little signs you see in the lawns of affluent neighborhoods telling you how the people who live inside love BLM and support Tony Fauci? That's the real Democratic Party.

More examples of angry Tucker Carlson:

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/desantis.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/a4.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/a5.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/zelensky.png)

#### In the world of Carlson, everything is a conspiracy and nobody is to be trusted! Except the conspiracy theorists, of course...

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/jan6.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/ct1.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/ct2.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/ct3.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/ct4.png)

#### The output using the angriest pieces of context about "White men" shows how Carlson plays on feelings of anger, racism, and fear using sarcastic straw man arguments meant to inflame. Similar messaging is used when talking about Putin and Russia, employing sarcastic straw men to paint liberals as obessed and blinded by their hatred of Russia.  

![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/t7.png)
![Plot1]({{ site.url }}{{ site.baseurl }}/assets/images/russia.png)




