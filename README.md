# ProxLib: A Book Recommendation System
During the Covid-19 pandemic, businesses of all sizes found themselves needing to pivot how they operated on a daily basis. This was especially true for those that did not have an e-commerce presence. 

This book recommendation system is a proof of concept to encourage the new businesses that have launched online to incorporate a rating and recommender system.


## TABLE OF CONTENTS

The home repository contains the project environment and information about this project.

### Notebooks

[Exploratory Data Analysis](notebooks/exploratory) 

[Final Report Notebook](notebooks/final)

### Reports
[Executive Summary](reports/Executive Summary)

[Visualizations](reports/Visualizations)

### Data

[Dataset](data)

### SRC

[Custom Functions](src)

### ReadMe

[Read Me](README.md)


This ReadMe is divided into sections addressing each step of the Cross-Industry Standard Process for Data Mining([CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)) approach for this project.

## Business Understanding 

How can reviews improve sales? How are our customers making choices on what items to buy? Choice is the act of picking or deciding between two or more possibilities. But how about when there are sometimes hundreds of thousands of possibilities? From Amazon to Netflix to Yelp, we have seen businesses capitalize and address this choice overload by providing recommendations to their customers.

<img align= "right" src="https://github.com/angelicacodes/book_recommendation/blob/main/reports/Visualizations/susan-yin-2JIvboGLeho-unsplash.jpg" width="400" height="200">

Just like in this row upon row of books—how do we best chose one if we haven’t read and possibly don’t know anything about the book? We might ask a friend for a recommendation. Similarly, businesses have sought to address this choice overload by providing recommendations to their clients/buyers on what products they might enjoy.

The Covid-19 pandemic has shown a pivot by businesses of all sizes across many industries.Many did not have a website before, but now do. Businesses have pivoted to include e-commerce. According to the [Harvard Business Review(HBR)](https://hbr.org/2020/07/how-businesses-have-successfully-pivoted-during-the-pandemic), one of the successful conditions of a business pivot--"pivoting is a lateral move that creates enough value for the customer and the firm to share." What are the consequences for operating in "business as usual" terms in a new normal?  HBR tells us that, "it does weed out business models that fail to pivot toward the new reality." Not only during pandemics, but business models are having to adapt and change to different realities as they grow, as time changes.

In his book, [Recommendation Engines](https://mitpress.mit.edu/books/recommendation-engines), [Michael Schrage](http://ide.mit.edu/about-us/people/michael-schrage), visiting fellow at the MIT Sloan School of Management’s Initiative on the Digital Economy writes, “Recommendation inspires innovation: that serendipitous suggestion—that surprise—not only changes how you see the world, it transforms how you see—and understand—yourself. Successful recommenders promote discovery of the world and one’s self,recommenders aren’t just about what we might want to buy; they’re about who we might want to become.” 

Citing Schrage's book, [Strategy+Business magazine](https://www.strategy-business.com/article/What-people-like-you-like?gko=d2e94) points that about **30% of e-commerce revenues around the world come from recommendations.**  


## Data Understanding

To build the recommendation system I have scraped the data from [Goodreads](https://www.goodreads.com/)--the largest community for reviewing and recommending books. This data will be used to train and test the model.

There the a total of 54,387 ratings
The ratings come from 28,959 reviewers
There are a total of 484 books being reviewed

Ratings consist of 1-5 stars with 1 being the lowest and 5 being the highest positive rating. 

### Obtaining the data

See data/scrape/final/scraping.ipynb for the functions and code used to scrape the data.

I opted for scraping the data with Selenium because the datasets available with similar data were oudated and did not have features such as text reviews, page length, or genre-- features that I wanted to have available for a phase 2 of this project which includes content based filtering. 

I was not able to use the Goodreads API because they did not provide one that enabled me to get the data that I needed--books connected to individual ratings, the ratings in the APIs availalble were aggregates.

### Data Properties

As mentioned above, the dataset set includes several features including book title, author name, page length, ratings, users, and text ratings.  For the purposes of this phase 1 of the project, the focus will be on:

- **book
- **user
- **rating

Book and user id's were strings that will be converted into numerical form, rating is already in integer type.

Having an accurate recommendation system will be important because want customers to trust the recommendation system so that they continue to use it in the future. 

##### Collaborative Filtering

I am using the model-based collaborative filtering approach which looks at the ratings that are available to predict those that are missing.

At a very basic level, what the model does is fill in the missing ratings with estimations of predicted ratings. It does this by looking at ratings that are filled in.

It goes through each of the users and sees which users are similar to you based on ratings you have in common. 

![matrix](https://github.com/angelicacodes/book_recommendation/blob/main/reports/Visualizations/books.gif)

The model learns from those ratings that are already there.



##### Distribution of Data

Often recommendation systems have what it called a long tail distribution— this means that some items have more ratings because they are more common or popular— for household goods this could be toilet paper and soap, and for movies it could be Avengers:Endgame. These are at the top of the tail because they outweigh a lot of other items in their respective categories.

This dataset does not have the long tail problem. There are so many options for books, and a wide array of tastes that it is unlikely that books would fall into this problem as we see here. There are some that have more ratings than others, but not so substantially of a few high over many low.

<img src="https://github.com/angelicacodes/book_recommendation/blob/main/reports/Visualizations/book_ratings.png" width="600" height="300">

In this histogram we can see that there are more ratings distributed among more books.


## Data Preparation

Because my scraper took reviews based on books found on a specific genre's page, there were duplicates between the book ratings because they may have been classified under multiple genres--such as one book falling into fiction, historical fiction, and history genres.

During the data cleaning process I:
- combined the multiple genre book and reviews table to make one combined data frame (see notebooks/exploratory/merge_tables.ipynb for merging multiple tables into one dataset)
- took out the duplicate data left by the scraping
- removed extraneous text
- removed text in mixed data type columns, such as the word "pages" from the pages columns so that column could be an integer data type


## Modeling


To build the model I am using [PySpark's Alternating Least Squares(ALS)](https://spark.apache.org/docs/2.2.0/ml-collaborative-filtering.html) which works well with sparce matrices. The ratings dataframe is  99.60% empty.

After building a first simple model(FSM) I itterate on that model adding and adjusting hyperparameters. However rather than continuing to iterate on this model by tesing with the hyperparameters by hand, I use spark's parameter grid builder to test out different parameters and cross validation to test the best model. The results from this give me a best model estimator with the following parameters:

The best model Rank is:  30
The best model MaxIter is: 20
The best RegParam: 0.1

After building a new model with these parameters, I receive the best RMSE to date.

Best RMSE: 1.22


## Evaluation

<img src="https://github.com/angelicacodes/book_recommendation/blob/main/reports/Visualizations/Screen%20Shot%202020-11-25%20at%2011.42.46%20AM.png" width="400" height="100">


To evaluate how well this model is predicting, as mentioned before, I’ve chosen to use the Root Mean Square Error(RMSE) metric.  This metric essentially let’s us know how far an observed value is from the model’s prediction.

For example, the best model RMSE I have so far is of 1.2. This means that the predicted value could have a standard deviation of + or - 1.2 the actual value. 

What does this mean for businesses that have and are pivoting to e-commerce during the Covid-19 pandemic? A model prediction rating of 3, with this error metric, the actual could be a 4.  Or rating is a 3 but actual is a 2. The first instance is not a problem-- we would have a customer more satisfied with the recommended book than predicted. However, in the second scenario, we would have an unhappy customer because they are less satisfied with the book than predicted. The drop from a predicted 5 to an actual 4 is not bad because it takes the customer from a  predicted 'Amazing' to a "really liked it"--the customer is still happy with purchase. However a jump from a 3 star rating to a 2 star rating is a difference of of a customer "liking" a book to "it's OK."

For this reason, I want to continuing to find ways (such as hyperparameter tuning, more robust grid search, and adding more data) to decrease the RMSE. Though this model works for proof of concept,  a lower RMSE is advisable before it is put into production.


## Potential Next Steps

<img align= "right" src="https://github.com/angelicacodes/book_recommendation/blob/28bd65dbd927984b7a69eb39f38729856d312e68/reports/Visualizations/laura-kapfer-hmCMUZKLxa4-unsplash.jpg" width="150" height="300">

Further phases could include:
- Scraping more data to add to the dataset
- Perform Natural Language Processing(NLP) on text reviews
- Add content-based filtering to address the cold start problem of recommendation systems
- Deployment of model to interactive form 






Image credits in order of appearance:
- Photo by [Susan Yin](https://unsplash.com/@syinq?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash
- Photo by [Laura Kapfer](https://unsplash.com/@kapfii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash





**Capstone project for Flatiron School's Data Science program**
