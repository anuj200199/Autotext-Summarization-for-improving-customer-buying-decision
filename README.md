# Autotext-Summarization-for-improving-customer-buying-decision
The project summarizes the reviews of a particular product on a website capturing the sentiment helping customer decide whether to buy the product or not.

The input to the project will be a text file containing all reviews of a particular product and the output will be a summary helping the potential buyers identify the strengths and weakness of the product.

A small excerpt from the file conatining reviews:
![A small excerpt from the file conatining reviews](https://user-images.githubusercontent.com/51110977/68645253-7c03f700-053d-11ea-8287-1ce39dbcdb6c.PNG)
 
 Summarised output identifying the overall product sentiment:
![Summarised output identifying the overall product sentiment](https://user-images.githubusercontent.com/51110977/68645334-b1a8e000-053d-11ea-82be-5da07733f18d.PNG)

<h3>DATASET USED</h3>

In our case, we are using the Amazon Reviews Dataset(reviews_data.csv) containing 5000 reviews for a total of 23 products. The figure below shows the distribution of reviews of 5 products.

![Reviews per product](https://user-images.githubusercontent.com/51110977/68646073-0fd6c280-0540-11ea-8b24-47b0213673f4.png)\

We have to group and concatenate the reviews by product and store in a csv format. The grouped reviews are stored under the <b><i>product_grouped.csv</i></b> file.

We will summarise the reviews of the first product - <b>All new fire HD tablet. 8HD display, WiFi, 16GB - includes special offer , Blue</b> containing 51 reviews and store in the file big.txt.

Small Excerpt of big.txt
![A small excerpt from the file conatining reviews](https://user-images.githubusercontent.com/51110977/68645253-7c03f700-053d-11ea-8287-1ce39dbcdb6c.PNG)
 
<h3>TEXT PROCESSING</h3>

<b>1. Sentence Tokenization</b>
For the generated text , we split the text into sentences. Sentence splitting is done to store each of the review and indexing them so that after further processing and textrank, we can extract the original representation which is syntactically meaningful stored in <b></i>split_sentences.csv</i></b>

<b>2. Decontraction</b>
Reviews are usually written using short forms of the words or combining two words. For example, some consumers may write "The product hasn't been upto my expectation" and others may write "The product has not been upto my expectation". In order to avoid the same sentences being identified as different sentences, all the contracted words are converted into their expanded forms. Some of the examples are

![list of decontracted words](https://user-images.githubusercontent.com/51110977/68647984-4236ee80-0545-11ea-9e23-a58a95f68726.PNG)

<b>3. Removing Unwanted words</b>
The inclusion of punctuation marks, unwanted breaks, brackets and multiple spaces is common among a large set of reviews. These extra special characters can be due to human mistake or serve as a purpose to bolster the idea. They do not contribute anything to summary formation and therefore, are removed.

<b>4. Spelling Corrector - Peter Norvig's Logic</b>
Humans seem to make spelling mistakes often while writing anything, whether it be reviews of a large piece of text. The paper makes use of the Peter-Norvig's spelling corrector logic. 

The first step is to edit the word by  removing a letter, replacing a letter, swapping letters or inserting a letter. We will create a list of all the edits. The probability of each word in the edited list is calculated as relative frequency to frequency of total words in a large document. From the probability values, we produce the candidates that are likely to replace the word in order of priority.

	1. The original word, if is known.<br>
	2. The list of words at distance one away.<br>
	3. The list of words at distance two away.<br>
	4. The original word, even though it is not known.<br>

After steps 2,3 and 4 the processed sentences are stored in <b><i>processed_sentences.csv</i></b>
<br>
For text processing, run <b><i>preprocess.ipynb</i></b>
<br><br>
<h3>Synsets Generation</h3>
To gain better results while generating the summary, we also have to consider the synonyms of words while calculating sentence similarity. These synonyms are called as synsets.

![synsets](https://user-images.githubusercontent.com/51110977/68648206-d3a66080-0545-11ea-9578-5cd871bb7eeb.PNG)

<h3>SENTENCE SIMILARITY</h3>
For calculating between two sentences s1 and s2, first generate individual list of all words in both sentences. Find the synsets of all words and add it to the two different lists synset1 and synset2. To calculate the similarity, the algorithm is mentioned below

![similarity](https://user-images.githubusercontent.com/51110977/68648286-04869580-0546-11ea-9bdd-37fc2191937c.PNG)

The similarity between all the sentences are stored in the <b><i>similarity_matrix.csv</i></b> as the output of <i></b>similarity.ipynb</i></b>

<h3>SELECTING HIGHER RANKED SENTENCES</h3>
<b>Graph Based Algorithm</b><br>
Graph is a data structure consisting of a set of vertices and edges representing the relationship between the vertices. In our case, the vertices represent the sentences and the edges, the similarity between them. The next step is to apply TextRank, a version of PageRank to generate the summary.
<br>
<br>
<b>TextRank</b><br>
In textrank, the vertices are the tokenized sentences. Weights are assigned to the edges connecting the node using similarity value. With arbitrary initial values, the iterative formula is run for each node until it converges below a specified threshold value.See <b><i>textrank.ipynb</i></b>

<h3>SUMMARY GENERATION</h3>
The output of textrank algorithm is the ranking of the sentences. Depending upon our requirements we will select the first n sentences.
Run <b><i>textrank.ipynb</i></b>

Output after selecting first 5 senetences<br><br>
![Summarised output identifying the overall product sentiment](https://user-images.githubusercontent.com/51110977/68645334-b1a8e000-053d-11ea-82be-5da07733f18d.PNG)

