# Subreddit Classification

## Introduction
Both the News and Worldnews subreddits curate news articles for their consumers. While one might assume that news would focus on US news with Worldnews focusing on International matters. However, in reality both of these subreddits cover both US and International news.

This presents an interesting problem in a classification model. Are certain topics more frequent in one subreddit over another? What about political leanings? Do these communities rely on different news sources for their posts? Do posts of one community generally have higher scores than the other? Constructed models seek to answer these questions.

| Variable 	| Description 	| Example 	|
|----------	|-------------	|---------	|
| subreddit   	| String. Title of the subreddit.      	| Ex: news, worldnews   	|
| title   	| String. Post title       	| Ex: Trade Talks with China    	|
| score    	| Integer. Post upvotes/downvotes difference      	| Ex: 15   	|
| num_comments    	| Integer. Number of comments for the post.      	| Ex: 25    	|
| domain    	| String. Website destination for the post.      	| Ex: cnn.com   	|
| target    	| Integer. Subreddit dummified as a 1 or 0.      	| Ex: 1    	|
| stem_title    	| String. Stemmed titles of the posts.      	| Ex: Tra Tal w Chi   	|
| lem_title    	| String. Lemmatized titles of the posts.      	| Ex: Trad Talk wi Chin    	|

## Data Science Problem
What subreddit metrics and models are best at separating posts of one subreddit from another similar subreddit?

Classification models will be constructed to test the ability of natural language processing and post titles to predict whether a post is likely to be from the News or Worldnews subreddit. Success in this project will be measured by a model's accuracy in categorizing posts to their respective subreddits.

## Importance
Classifying posts in different subreddits may be an interesting thought experiment, but what practical applications does it have? In an increasingly interconnected society with a 24/7 news cycle allows for more media than we possibly have time to consume. It can be incredibly easy these days to rely on one or two news outlets that already cater to our existing opinions without looking for different interpretations of the facts.

A well-rounded news diet is essential for understanding all sides of an argument. This includes both understanding **multiple sides** of an argument and understanding a **diverse range of topics**. So often, we are confronted by calls of "Fake News" that highlight how differently our media outlets portray the same stories. In practice, it can often be hard to find unbiased news so we are forced to confront many outlets. This way we at least we consume enough of the facts to render our own interpretation of the full picture.

**Key Points:**
1. Understanding multiple sides of an argument
2. Consuming news media encompassing many topics

These two key points are possible through model building and interpretation. In a perfectly balanced model, there is an equal chance of categorizing a post from either subreddit. As the subreddits diverge in topic of conversation, they become easier to categorize. With this model, we can expect models with high accuracy to be examining subreddits that are less alike. Additionally, breaking down each post's title allows us to examine topic frequency and perhaps hint at leanings of the subreddit. For example, a subreddit talking about protests may talk more about police action or protester disruptions, depending on the subreddit's opinion of the events.

Further analyzing how we consume media is essential for creating a civil public discourse. So often we see online arguments devolving into name-calling and separation. This is due to a reactionary focus on right vs. wrong instead of a proactive conversation about shared values and the effectiveness of public policy strategies. Personal news media analysis can move us in the right direction.

## Process
The 5000 most recent posts as of October 13th, 2019 were collected from the News and Worldnews Subreddit communities of the Reddit website by looping through API requests, returning JSON dictionaries. Each dictionary was subsetted for its data and appended to a data-frame for each subreddit.

Each subreddit's dataframe was analyzed for missing or improper values, during which posts of non-english languages were removed. Relevant columns were selected from the total returned columns. The final data for the News and Worldnews subreddits included the subreddit, titles of recent posts, post scores, number of comments for each post, and the domain of the website referenced by the post. Additionally, word frequencies were calculated for the lemmatized post-titles of each subreddit to obtain a general sense of their popular topics.

## Model Exploration
Several classification model types were fit to test varying aspects of binary classification and model features. A total of seven models were fit:

#### Models
1. Logistic Regression Count Vectorizer
2. Logistic Regression Term Frequency: Inverse Document Frequency
3. Bernoulli Naive Bayes Count Vectorizer
4. Multinomial Naive Bayes Term Frequency: Inverse Document Frequency
5. Column Transformer LR Term Frequency: Inverse Document Frequency
6. Bernoulli Naive Bayes Lemmatized Titles
7. Bernoulli Naive Bayes Stemmed Titles

#### Bernoulli Naive Bayes: Stemmed Titles
Each model yielded similar results with consistent degrees of overfitness. The model that resulted in the best accuracy was the Bernoulli Naive Bayes model with Stemmed titles with a total 687 misclassifications of 2458 total test cases or a successful classification rate of 72.05% which is substantially over our base model of 50%.

#### CT-LogReg: TF-IDF
Another interesting model was the Column Transformed Logistic Regression with the Term Frequency: Inverse Document Frequency Vectorizer. This model included not only post titles, but also scores for each post, and the number of comments. While this reduced the overall overfitness of the model, it also brought down the accuracy, with 756 misclassifications out of 2458 total test cases or a successful classification rate of 69.24% which is worse than our previous model.

## Future Development
Seeing such a perfect project may leave you thinking, "But how can we possibly improve this miraculous and beautiful model?". Well fear not valiant reader, for there is always room for improvement!

#### Accuracy
While accuracy may hint at how different two subreddits are, a poor accuracy score may just be a result of a poor model. Future development of this project would not only seek to differentiate subreddits by isolating more features, but also quantify the degree of similarity between subreddits.

#### Generalizability
The data collected for this model included the 5000 most recent posts from these subreddits. This will hinder generalizability in the long-run as the news is constantly changing. The news of any given week will vary a great deal over the course of a year, especially in subreddits with such a broad scope. Future models should collect data for much longer periods of time to discover underlying trends in posts. For example, does Worldnews post more consistently about international trade than News?

#### Utility
The use of the model can be expanded from its current utility classifying subreddits and look at the political leaning of a subreddit and recommend other subreddits or news sources to round out one's media consumption. This would require further data collection, a model for revealing political leaning, and expand to news sources outside the reddit community.
