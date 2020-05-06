# Project Gemstone

### Business Understanding

With Videogames becoming increasingly popular among the adult population, the potential value in sales is unprecedented. This was true even before the lockdown in 2020 from the Coronavirus. However, games only have a limited number of hours before the content is consumed by the users. There are exceptions to this such as competitive multiplayer games, but even those games lose their luster for veteran gamers after a long enough duration. This pattern of content consumption drives the consumer to search for more and better games to fill their desire for new content.
    
Currently, the most successful driver of gaming sales has been targeted advertisement. Steam is known to host "Steam Sales" throughout the year, with some games seeing price reductions between 50 and 85%. The success of these sales is so well known that they've become the subject matter of many online memes within the gaming community.
    
The question remains: how can the development companies recoup on the lost revuenue from the discounted sales events? I believe the most effective method would be by narrowing in on the gaming preferences of the users themselves. By establishing an effective content-based recommendation system, the gaming industry would be capable of connecting users to games similar to those they've enjoyed in the past. If successful, the industry would benefit from the increased revenue of non-discounted sales, and the gaming community would benefit by being able to effectively navigate the ocean of games available to them. Consumer and Producer both benefit.
    
To that end, this project endeavors to create a content-based system that provides recommendations based on the users' descriptions of their ideal video game experience. The ultimate goal is to find the hidden gems undiscovered by the community and boost sales and enjoyment in the process.

For a quick overview of the sources used for this project, please see the following link:  
https://cseweb.ucsd.edu/~jmcauley/datasets.html#steam_data
    
### Data Understanding
The data used for this study can be downloaded from https://cseweb.ucsd.edu/~jmcauley/datasets.html#steam_data
    
Please be sure to follow the instructions @ the top of the webpage for unpacking the JSON files, and please reference the following studies when using the aforementioned data:
    
    Self-attentive sequential recommendation  
    Wang-Cheng Kang, Julian McAuley  
    ICDM, 2018  

    Item recommendation on monotonic behavior chains  
    Mengting Wan, Julian McAuley  
    RecSys, 2018  

    Generating and personalizing bundle recommendations on Steam  
    Apurva Pathak, Kshitiz Gupta, Julian McAuley  
    SIGIR, 2017  

The files of importance are "Version 2: Review Data" and "Version 2: Item Metadata". These 2 files can be merged along 'reviews:product_id' and 'games:id' categories. As the Games information is only relevant once the recommendation is complete, the primary focus of our data preparation and cleaning is the Reviews file.

Please see the README.md in the /data directory for a thorough walkthrough of the downloading process, and conversion to the CSV format for my coding purposes.
    
I passed the information into a Pandas dataframe, which resulted in over 7,000,000 rows and 15 columns. This dataframe represents 7,000,000 reviews spread acress roughly 35,000 unique games. The goal of the project is to implement a content-based system, and therefore the most important columns are the 'text' and 'product_id'.
    
### Data Preperation

Please see the README.md in the /data/ directory for a thorough walkthrough of the downloading process, and conversion to the CSV format. The remainder of the data preparation is summarized here. 

The exact coding process for cleaning the data can be found in the first few cells of the final_notebook under the report folder. Here, I will provide a brief overview of the methodology. First, Seeking to lower the amount of data for processing, I removed all unnecessary columns such as "found_funny", for example. Second, I removed all rows with null values in any of the columns. Third, I removed all the reviews that were based on less that 1 hour of gameplay (not sufficient game time for accurate review). Fourth, I calculated the total reviews for each game and subsequently removed all games with less than 500 total reviews (not sufficient data to describe the game).
    
Once the data was parameterized within reason, as determined by the problem domain, I then needed to clean the text before vectorizing it. First, I removed any punctuation or numbers within the text reviews. Second, I used NLTK's built in tokenize function to tokenize the text in each row. Third, I used NLTK's english stop_words list to remove any unnecessary words for classification. For example, stop_words will normally include words such as "I", "the", "and", "it", because these words are fairly universal among all subjects and therefore provide no benefit when performing classification. This final column was named 'clean_data' to preserve the original form.
    
### Modeling

The modeling in this particular methodology is untraditional. I did not approach this problem from the perspective of predicting a given value. The purpose of this project is to recommend similar games and thus the methodology when making recommendations is purely providing the most similar vectors. 

Having already cleaned the text data, I needed to process it through Doc2Vec for vectorization. First, I transformed the text into TaggedDocument format, assigning the clean_text to "words" and the product_id to "tags". The values of the tagged documents were then used to build the vocabulary for the model.


### Evaluation

Since the modeling method is untraditional, I created a custom metric by which to measure the performance of each model. I generated 5 unique descriptions for a user's "ideal game", each of which is assigned 5 game recommendations. I manually checked the descriptions and gameplay videos of the recommended games in order to determine their relevance to the description. If the recommended game was relevant (i.e. having similar elements to the description itself), then the recommendation was given a score of 1. On the otherhand, if the recommended game was irrelevant then it was given a score of 0. Each model's performance is evaluated on the percentage of recommended games which were relevant to the different descriptions: (total relavant recommendations) / 25

The goal is to maximize this Relavance Metric in the hopes that each game recommended is relavant to the description given.

### Future Improvements

