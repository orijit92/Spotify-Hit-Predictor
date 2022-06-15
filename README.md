
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

![image](https://user-images.githubusercontent.com/85646063/173940555-784ee936-396c-4660-b377-ed014135690b.png)


**For Dataset containing songs of the earliest 3 decades (1960-1989):**

Reviewing the results of the logistic regression and p values, danceability, loudness, mode, tempo, speechiness, acousticness, time signature, instrumentalness were most significant predictors because their p-values were the smallest. Hit songs are more likely to have higher danceability, loudness, and are faster. Additionally, hit songs are more likely to be songs with 4 beats/measure and major key. Correspondingly, songs with higher speechiness, acousticness, instrumentalness, and songs with 5 beats per bar tend to be flops.
After running confusion matrices (appendices 2.1, 2.4, 2.6) for all groups described above, we see an accuracy of 73.31% for all six decades, 79.65% for 1990-2019, 71.62% for 1960-1989.

![image](https://user-images.githubusercontent.com/85646063/173940697-7d436b8c-ee6f-4140-a317-31ca1de1c2f6.png)



