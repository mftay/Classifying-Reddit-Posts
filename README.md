### Introduction & Objective

Imagine we have a group of people who only read fantasy books and are interested in news and group discussions about their favorite authors and upcoming book releases. Now imagine that they are repelled by science fiction books. They absolutely abhor that genre and never wish to see or read anything about it.



That is the basis of this project, in which we build a model capable of distinguishing whether a post from subreddit is from the fantasy or the science fiction books section. The idea is to filter, for those fantasy loving people, relevant posts directly into their inbox, like a curated news feed. To give an idea of the popularity of fantasy vs. science fiction, the fantasy subreddit has 760,000 subscribers, with around 62 posts a day. The science fiction subscribers number 60,000, with 13 posts a day.



### Methodology

The broad methodology is as follows: 

1) API calls to the subreddits to obtain about 1,200 posts each from Fantasy and Science Fiction. This will reduce to ~1,000 posts each after data cleaning. Each post had information such as user name, status of user, thumbnail photos etc.

2) **During EDA**:

a) All the 110 columns were examined and only 3 columns were kept: 'title' and 'selftext' columns which is the content of the post, and the 'subreddit' column to identify which post the subreddit is from. 

b) A number of duplicate posts were found, possibly due to 'stickied' posts which are semi-permanent posts left on the front page of the subreddit - they could be ongoing polls, or announcements. The 'stickied' column was used to identify and remove duplicates. 

c) The remaining columns was deemed not useful as they contained information like thumbnails/handle of the redditor which is not relevant to classifying whether a post belongs to Fantasy or Science Fiction.

d) Posts with empty 'selftext' columns were dropped as they do not contain enough text to perform the classifier on.

3) **Pre-processing**: The raw posts had html and punctuation. Pre-processing was done in the form of BeautifulSoup and regex to remove these words. URLs were also removed. Customised stopwords were also inserted, such as 'book', 'fantasy', 'printSF' as these would be a giveaway for the model.

4) **Modelling**: Test size of 20% was set, and the models had to beat a baseline model accuracy of 54%. We have a total of 1830 reviews.

a) Naive Bayes (Multinomial Bayes) and logistic regression was chosen. A pipeline was built of CountVectoriser, which converts the cleaned posts into vectors, and gridsearch performed over multiple values of maximum features, ngrams, and the max and min frequency a word had to appear in the documents.



### Results

The Naive Bayes (NB) model scored 86% on testing data, which is not bad. This is for CountVectoriser parameters of 1500 max features, a minimum and maximum document frequency of 1% and 70%.

The logistic regression model scored 85% on testing data. However it was overfitted as it scored 99% on training data. This is for CountVectoriser parameters of 2000 max features, a minimum and maximum document frequency of 1% and 70%, and on bigrams. 

To reduce overfitting, a wordcloud was built to visualise the most occurring  words of the reviews in the training dataset. The most common words were then iteratively added to the customised stopwords list, however, the overfitting did not reduce by much.

