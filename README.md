
**Problem :**

The datasets contain the measures of the song features for roughly 41,000 songs from 1960 to 2019. The target column indicates if a song is a Hit or Flop. Hit songs are the ones that have been featured on the Billboard at least once. There is not a clear answer on which song features are most important in making a song a hit. To solve the problem, me and my team used R to build a variety of prediction models to find out what the most important song features are in modern hit songs and learn which model works best for such a dataset.

**Goal :**

The goal is to predict whether a song would be a hit or a flop by examining audio features of songs from 1960 to 2019. For example, if a music studio were to start a new project, would features like loudness, acousticness impact the song's popularity more than others?

**Description of the Dataset :**

The Kaggle Spotify hit dataset contains 6 datasets partitioned by decade. 
 

Name	| Type	| Description
------|--------|--------------
Track	| character	| The name of the track.
Artist	| character	| The name of the Artist.
Uri	| character	| The Spotify url for the track.
Danceability	| double	| Describes how suitable a track is for dancing. 0 to 1 indicates  Low to High
Energy	| double	| Measure of intensity and activity. 0 to 1 indicates  Low to High
Key	| integer	| The key of the track. 0 to 11 indicates C note to B note
Loudness	| double	| Loudness of track in decibels
Mode	| integer	| Modality(Major or Minor) of a track
Speechiness	| double	| Presence of Spoken words in a track. 0 to 1 indicates Low to High
Acousticness	| double	| Measure of the acousticness of track. 0 to 1 indicates Low to High
Instrumentalness	| double	Likelihood of a track containing no vocals. 0 to 1 indicates Low to High
Liveness	| double	| Presence of an audience in the recording.0 to 1 indicates Low to High
Valence	| double	| Musical positiveness conveyed by a track. 0 to 1 indicates Low to High
Tempo	| double	| Tempo of a track in BPM(Beats per Minute)
Duration_ms	| integer	| Duration of the track in milliseconds
Time_signature	| integer	| Overall time signature of a track (1,2,3,4 or 5)
Chorus_hit	| double	| Timing of the start of chorus
Sections	| integer	| Number of sections of a particular track
Target	| integer	| Whether the track is a hit (1) or not (0)



We started with doing an exploratory analysis and analyzed how several song features changed over the decades.

**Exploratory Analysis :**

![image](https://user-images.githubusercontent.com/85646063/173898554-0c7f5de1-ec09-43ff-b700-1100c8317d54.png)


![image](https://user-images.githubusercontent.com/85646063/173898608-4aad3108-0c24-4687-834a-a8f57a534352.png)

Hit Song vs. Non-Hit Song Instrumentalness Comparison:

![image](https://user-images.githubusercontent.com/85646063/173898654-a6837c97-23a2-4f09-a2fe-b0e6010d4a19.png)

**Model I: Logistic regression**

Method:  Since mode, time signature, key, and decade are categorical variables, we factored these variables. We re-coded mode 0 as minor and 1 as major and left the base level as minor. The default base level for time signature was set as 4, since 4/4 time is one of the most common time signatures when it comes to music. For key, we converted the numerical values from the source data (0 to 11), to their corresponding keys where 0 = C. The default base level of 0 = C was kept.
We executed 3 regression runs. The first run contained all the data over the 6 decades. The second run comprised data over the last 3 decades i.e. 2010s, 2000s and 1990s. Our final run contained data over the earlier 3 decades i.e. 1960s, 1970s and 1980s. After running the model on the validation set, we used the validation set to plot the ROC curve, showing the tradeoff between sensitivity and specificity. Additionally, we used the coords() function to determine the value for the best threshold represented on the ROC curve as the point furthest away from the line of no discrimination (appendix 2.1). 

**For Dataset containing songs of all Decades (1960-2019):**

Reviewing the results of the logistic regression and p values, danceability, energy, loudness, mode, speechiness, acousticness, and instrumentalness were most significant predictors because their p-values were the smallest. Other significant variables include valence, tempo, and time signature. Hit songs are more likely to have higher danceability, valence, loudness, and are faster. Additionally, hit songs are more likely to be songs with 4 beats/measure, major key, and in keys C#, D#, F, F#, G#, A#, and B compared to songs in the key C. Correspondingly, songs with higher energy, speechiness, acousticness, and instrumentalness tend to be flops.

![Logistic Regression All decades](https://user-images.githubusercontent.com/85646063/173940453-6c0b2639-b287-46f9-84ed-baf90b4842cd.png)

**For Dataset containing songs of the past 3 decades (1990-2019) :**

Reviewing the results of the logistic regression , danceability, energy, loudness, mode, speechiness, acousticness, liveness, valence, time signature, and instrumentalness were most significant predictors. Hit songs are more likely to have higher danceability, loudness, and instrumentalness. Additionally, hit songs are more likely to be songs with 4 beats/measure and in major key. Correspondingly, songs with higher  energy, acousticness, liveness, valence, and time signature (3) tend to be flops.

![image](https://user-images.githubusercontent.com/85646063/173976060-256db103-9c4d-43be-ad6a-0b4fe3ff925d.png)



**For Dataset containing songs of the earliest 3 decades (1960-1989):**

Reviewing the results of the logistic regression and p values, danceability, loudness, mode, tempo, speechiness, acousticness, time signature, instrumentalness were most significant predictors because their p-values were the smallest. Hit songs are more likely to have higher danceability, loudness, and are faster. Additionally, hit songs are more likely to be songs with 4 beats/measure and major key. Correspondingly, songs with higher speechiness, acousticness, instrumentalness, and songs with 5 beats per bar tend to be flops.
After running confusion matrices (appendices 2.1, 2.4, 2.6) for all groups described above, we see an accuracy of 73.31% for all six decades, 79.65% for 1990-2019, 71.62% for 1960-1989.

![image](https://user-images.githubusercontent.com/85646063/173976107-1dccfdee-1262-4e12-a8a4-bfcbda709167.png)

**Decision Tree :**

![image](https://user-images.githubusercontent.com/85646063/178332977-457ac479-99cb-48e3-98ad-7d75004dab52.png)


We can wee that instrumentalness is the most important song feature deciding if a song would be a hit.

**Logistic Regression:**

**ROC Curve and Confusion Matrix (all decades)**


![image](https://user-images.githubusercontent.com/85646063/178333012-1b40b556-1323-4cab-9e1f-e7ffa8ef39d7.png)

**Roc Curve and Confusion Matrix (1990-2010s):**

![image](https://user-images.githubusercontent.com/85646063/178333032-645cb43e-0558-461d-a10c-a9171b19dc0c.png)

RANDOM FOREST

Since a decision tree can encounter over-fitting issues and it can also ignore explanatory predictors when there's a small sample size and a large p-value, I decided to create a random forest model and put it to the test.

Method:

For this model, I didn't had to partition the dataset into training and validation sets since the random forest pick a sample with replacement from the dataset as its training set for every individual tree. Those not considered in the training set for a specific tree are going to be used later on to test the prediction power. The records that were not used to build a tree are referred to as Out-Of-Bag (OOB).

At first, I ran the model with the default values (number of trees 500, variables to consider at each splitting node 4) for the whole database. I ended up getting an OOB error of 19.01%, which translates into 80.99% of accuracy. It came to my mind that maybe different tastes for music along the decades could be canceling the effects from the predictors to the target variable (the characteristics that made a hit in the 60s might be different from those in the 2010s).

Since we are trying to predict a hit or a flop as for today, and we also know that the more data the better (as long as it doesn't contain errors or misleading info like it could be from other decades taste for songs), I decided to try the latest data adding the following decade each time and looking for the accuracy behavior.

We got:

Considered data	| OOB Error
-------------|-------------
2010s |	15.49 %
2010s and 2000s |	15.04 %
2010s, 2000s and 1990s |	14.9%
2010s,2000s, 90s and 80s |	16.51 %

We can notice that when considering the 80s, a lot of noise and confusion was added to the model, losing prediction power. So, I consider utilizing only the data conformed from the 90s, 2000s, and 2010s.

As for the next step, I searched for a proper number of trees to consider. To do so, I plotted the Out-of-Bag errors by the number of trees considered in the model. As we can see in FIG1. 500 trees look to be a good number of trees since the errors are not decreasing anymore in a meaningful way, it looks like by 500 trees we are already observing convergence.

Then, we search for the optimal number of variables to consider at each split.

To do so, I computed the OOB errors when considering 1 variable all the way to 10 variables.

The results were as follows:

Number of variables considered per split	| OOB Error
1	| 0.1644744
2	| 0.1514896
3	| 0.1495784
4	| 0.1489601
5	| 0.1494098
6	| 0.1505902
7	| 0.1490163
8	| 0.1522203
9	| 0.1507026
10	| 0.1516582
So, as we can observe considering 4 variables as suggested by default (considering the square root of your predictors) for splitting at each node was in fact the optimal since it brings the lowest OOB error. But we wouldn't be able to know unless trying for different values.

Result:

In conclusion, in this prediction model, we generated a random forest that involves generating 500 decision trees which will evaluate any new song characteristics, and then the result will be the majority vote of these all 500 trees' decisions. It considers 4 random variables at each splitting node, and it presents an overall OOB error of 14.9% (85.1% accuracy).

We can observe the confusion matrix in FIG2 to notice that the overall OOB error comes from a 19.43% error from wrongly predicting flops and a 10.37% error from wrongly predicting hits.

As for the predictor's importance, I plotted their permutation importance in FIG 3 which clearly highlights instrumentalness as the most important factor. Just for interpretation purposes, the way these values are determined comes from permuting all the OOB values from a predictor (so you break the real relationship with the target) and then try them with each tree. If the error after permuting the predictor's values doesn't increase significantly, then it means that the variable wasn't very important. Then the difference between the error from the real data and the permuted data for a specific predictor is going to be averaged across all trees and normalized by the standard deviation of the differences. So, in the end, the variables with the higher difference between errors (Highest decrease of accuracy) are going to be the most important for our model.

![image](https://user-images.githubusercontent.com/85646063/178160363-2eb8a05c-b3dd-4326-a0a2-685785aa8e5e.png)



![image](https://user-images.githubusercontent.com/85646063/178160373-43a1ba01-23de-4fd1-8104-c9420f17c09b.png)







