# ProxLib: A Book Recommendation System
 PROJECT OVERVIEW HERE

## TABLE OF CONTENTS

The home repository contains the project environment and information about this project.

### Notebooks

[Exploratory Data Analysis](notebooks/exploratory) 

[Final Report Notebook](notebooks/finanl)

### Reports
[Executive Summary](reports/presentation)

[Visualizations](reports/visualizations)

### Data

[Dataset](data)

### SRC

[Custom Functions](src)

### ReadMe

[Read Me](README.md)


This ReadMe is divided into sections addressing each step of the Cross-Industry Standard Process for Data Mining([CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)) approach for this project.

## Business Understanding 
--Add stakeholders

What is choice? How to we choose?

Choice is the act of picking or deciding between two or more possibilities. 

How about when there are sometimes hundreds of thousands of possibilities? How can we possibly choose? Or better put, how can we know that we are making the right choice?

<img align= "right" src="https://github.com/angelicacodes/book_recommendation/blob/main/reports/Visualizations/susan-yin-2JIvboGLeho-unsplash.jpg" width="400" height="200">

Just like in this row upon row of books—how do we best chose one if we haven’t read and possibly don’t know anything about the book? We might ask a friend for a recommendation. Similarly, businesses have sought to address this choice overload by providing recommendations to their clients/buyers on what products they might enjoy. 

In his book, [Recommendation Engines](https://mitpress.mit.edu/books/recommendation-engines), [Michael Schrage](http://ide.mit.edu/about-us/people/michael-schrage), visiting fellow at the MIT Sloan School of Management’s Initiative on the Digital Economy writes, “Recommendation inspires innovation: that serendipitous suggestion—that surprise—not only changes how you see the world, it transforms how you see—and understand—yourself. Successful recommenders promote discovery of the world and one’s self,recommenders aren’t just about what we might want to buy; they’re about who we might want to become.” 

Citing Schrage's book, [Strategy+Business magazine](https://www.strategy-business.com/article/What-people-like-you-like?gko=d2e94) points that about **30% of e-commerce revenues around the world come from recommendations.**  

##### Content based vs. Collaborative

Today, I am going to share with you a book recommendation system that uses collaborative filtering to predict what a customer will rate a certain book—which will in turn give a book recommendation the customer is likely to enjoy. 

According to the Atlantic “the publishing industry survives on super fans—-on bookworms who read far more than most Americans, and who then tell their friends what to read as well.”

Codex “estimates that 11 % of book buyers make about 46% of recommendations” 



## Data Understanding

To build the recommendation system I have scrapped the data from [Goodreads](https://www.goodreads.com/)--the largest community for reviewing and recommending books. This data will be used to train and test the model.

There the a total of 54,387 ratings
The ratings come from 27,642 reviewers
There are a total of 484 books being reviewed

Ratings consist of 1-5 stars with 1 being the lowest and 5 being the highest positive rating. 

##### COLLABORATIVE FILTERING

I am using the model-based collaborative filtering approach which looks at the ratings that are available to predict those that are missing.

To build the model I am using [PySpark's Alternating Least Squares(ALS)](https://spark.apache.org/docs/2.2.0/ml-collaborative-filtering.html) which works well with sparce matrixes. The ratings dataframe is  99.60% empty.


At a very basic level, what the model does is fill in the missing ratings with estimations of predicted ratings. It does this by looking at ratings that are filled in.

For example, if the model does a good job, it will likely recommend that I read Nancy Drew because it is likely that I will give it a high rating because other users who overlapped in ratings with me and had similar high ratings to my same books, rated Nancy Drew high.  


##### LONG TAIL

Often recommendation systems have what it called a long tail distribution— this means that some items have more ratings because they are more common or popular— for household goods this could be toilet paper and soap, and for movies it could be Avengers:edgame. These are at the top of the tail because they outweigh a lot of other items in their respective categories.

NOT LONG TAIL PROBLEM:
This dataset does not have the long tail problem.  There are so many options for books, and a wide array of tastes that it is unlikely that books would fall into this problem as we see here. There are some that have more ratings than others, but not so substantially of a few high over many low.

##### Metrics available:



## Data Preparation


Because my scrapper took reviews based on books found on a specific genre's page, there were duplicates between the book ratings because they may have been classified under multiple genres--such as one book falling into fiction, historical fiction, and history genres.

During the data cleaning process I:
- combined the multiple genre book and reviews table to make one combined data frame
- took out the duplicate data left by the scrapping
- removed extraneous text
- removed text in mixed data type columns, such as the word "pages" from the pages columns so that column could be an integer data type
- 



## Modeling





## Evaluation

##### Cross-Validation


## Potential Next Steps

<img align= "right" src="https://github.com/angelicacodes/book_recommendation/blob/28bd65dbd927984b7a69eb39f38729856d312e68/reports/Visualizations/laura-kapfer-hmCMUZKLxa4-unsplash.jpg" width="150" height="300">

Further phases could include:
- Adding more data
- Natural Language Processing on reviews
- Content-based filtering 
- Deployment of model to interactive form 


## Additional sources

Image credits in order of appearance:
- Photo by [Susan Yin](https://unsplash.com/@syinq?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash
- Photo by [Laura Kapfer](https://unsplash.com/@kapfii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash





**Capstone project for Flatiron School's Data Science program**
