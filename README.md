# Spotify-Charts-2023-EDA

We were tasked to commit an exploratory data analysis on a dataset containing information about popular tracks on Most Streamed Spotify Songs 2023 [(spotify-2023.csv)](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)

## Dependecies
For this task, the following 
- Jupyter Notebook
- Pandas Library
- Numpy Library
- Mathplot Library
- Seaborn Library

## Guide Questions

These served as a guidlines and the questions posed for the code written, the results, and the visual presentations as well as its interpretations are to answer.

### A. Overview of Dataset
How many rows and columns does the dataset contain?
What are the data types of each column? Are there any missing values?

### B. Basic Descriptive Statistics
What are the mean, median, and standard deviation of the streams column?
What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

### C. Top Performers
Which track has the highest number of streams? Display the top 5 most streamed tracks.
Who are the top 5 most frequent artists based on the number of tracks in the dataset?

### D. Temporal Trends
Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

### E. Genre and Music Characteristics
Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

### F. Platform Popularity
How do the numbers of tracks in spotify_playlists, ~~spotify_charts, and apple_playlists~~,deezer_playlists, and apple_playlists compare? Which platform seems to favor the most popular tracks?

### G. Advanced Analysis
Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.


# ---------------------------------------------------------------

### Section A - Dataset Overview
First, importing the required libraries is imperative to manipulate, explore, and clean the data, then importing the dataset using pd.read_csv function. the syntax was a little bit different as the traditional, as the .csv file was not utf-8 compatible, so the latin-1 syntax had to be added. Then, using the len function, with axis=0, it is easy to determine the number of rows and axis=1 for the number of columns, presenting **953 rows and 24 columns**. To show the data types of each column, the function dtype was used and the results are as follows;

- track_name -              object
- artist(s)_name -         object
- artist_count -            int64
- released_year -           int64
- released_month -          int64
- released_day -            int64
- in_spotify_playlists -    int64
- in_spotify_charts -       int64
- streams -                object
- in_apple_playlists -      int64
- in_apple_charts -         int64
- in_deezer_playlists -    object
- in_deezer_charts -        int64
- in_shazam_charts -       object
- bpm -                     int64
- key -                    object
- mode -                   object
- danceability_% -          int64
- valence_% -               int64
- energy_% -                int64
- acousticness_% -          int64
- instrumentalness_% -      int64
- liveness_% -              int64
- speechiness_% -           int64

The object pertains to strings and int64 to int values. Some columns must be changed from objects to numerics for mathematical functions such as Central Tendency Functions and Correlation functions. This change however was done after identifying the NaN values and changing them. **There are only two columns with NaN values, the in_shazam_charts with 50 empty entries and key columns with 95 empty entries**, thanks to the help of the combination of .ismull function and .sum function. And with the use of .fillna, the null values were change to either 0s for the in_shazam_charts column, or unknown for the key column. And then after a few manipulation and replacement of commas into nothing, as well as handling a very specific case, using pd.to_numeric, the data types of certain columns like the streams, in_deezer_playlist and in_shazam_charts were replaced to int64


### Section B - Basic Descriptive Statistics
Thanks to the stream column's data type altered, the Central Tendency functions can now be used, presenting a **mean of 513597931.3137461 streams**, a **median of 290228626 streams**, and the **mode was 156338624 streams**. Outliers can easily be determined using .describe, by looking at the max values, however, two separate dataframes had to be made using .groupby in conjunction with .size, making one for how many songs from each year did it make to the top streamed data set, and how many songs had how many artists. It is also especially emphasized with visual presentation, a bar graph, which shows that the outliers was probably the year 2022, which makes sense, the songs last year were the most popular songs of the current year. The graph also shows that generally, the closer the year is to the current one, the more songs from that year makes it to the data set. 

![image](https://github.com/user-attachments/assets/1df587ac-2048-4562-8a17-ad5c58160ed6)


As for the artist count, it seems that the less artist makes a song, the less popular it is, which makes sense, considering collabs with multiple artists and/or bands are rare and songs from one artist or band are more often released. 

![image](https://github.com/user-attachments/assets/943383b5-7e41-45e3-81cb-4a165d268fe5)


### Section C - Top Performers
By using the .copy, .sort_values, and .head functions, it is easy to identify which song has the highest streams, which are the following

![image](https://github.com/user-attachments/assets/fadf7d67-0144-49fc-9962-40e768efdd27)

Blinding Lights seems to be the most streamed song of the year. However, none of the top 5 songs were from the artist with the most songs on the board.

Using .groupby, it is easy to merge how many songs released by the artist were able to make it to top streams of 2023, presenting a result of;

![image](https://github.com/user-attachments/assets/477063a3-9cf2-4266-a83f-88d434ca9e44)

Taylor Swift has the most song entries that made it to the 2023 most streamed songs: talk about unironic. The Weeknd followed her, though he did get his song as the most streamed of the year and also got the most streamed artist (see Section G).


### Section D - Temporal Trends
Just using the same dataframe as from Secton B, we can map a line plot of how many songs were released for each year, and with the same interpretation, the close it is to the current year, and the songs from the previous year seem to have the most popularity, the more songs make it to the top stream of the Spotify platform, as presented by the graph.

![image](https://github.com/user-attachments/assets/9e9ed50a-e470-4238-98ad-0ea81d134aed)

And using .groupby and .size once again, we can count how many songs that were released each month were able to make it to the top streamed and present it with a bar graph;

![image](https://github.com/user-attachments/assets/57b8bb74-b3d7-4c56-9537-c3944d63ab4f)

January and May seem competing in how many songs released from their month made it to the top charts. January makes sense even for the songs released that year, it had enough time to be streamed all throughout the year, it could be why songs from ber months only had average, although songs from May seems to have a bit of a mystery.


### Section E - Genre and Music Characteristic
This is where .corr function shined, which was in default, uses Pearson correlation, which interprets as if the values are closer to eiher 1 or -1, they are correlated, positively or negatively, respectively. It can also be visualized using a scatter plot. The .corr function showed a low correlation to all the attributes to the number of streams, the bpm only having -0.002 or a negative 0.2% correlation, and the energy% only having -0.02 or a negative 2% correlation. However, danceability% seems to have a negative 10% correlation, which is still pretty bad. These are further emphasized by the graphs

![image](https://github.com/user-attachments/assets/8551f773-b1f8-4558-9948-d568627ec8ea)

![image](https://github.com/user-attachments/assets/3876edfa-4a57-4309-a1c6-946cc4e28a73)

![image](https://github.com/user-attachments/assets/b07da41b-b932-49ab-be6f-df50a8f4683e)

As presented, either the stream values are either too low, and/or all the values are scattered all over the place, which presents no correlation at all.

The same thing for attribute to attribute correlation, using the .corr, it presents that danceability and energy had an almost positive 20% correlation, which is the most correlation computed there is among the computed values, but is still pretty low, further proved by the graph visualized

![image](https://github.com/user-attachments/assets/e09f758f-941e-4d8d-bc50-bfaa420c07ca)

There is a faint growth, although it is scattered thoroughly, it could be seen as following a general direction. An then, as for the valence and acousticness, there is only an 8% correlation, which is pretty low, but higher than the 2% and 0.2% from the streams to attributes correlation. This was the graph as visualized of the valence and acousticness;

![image](https://github.com/user-attachments/assets/9c4e0598-32f6-43c0-ae82-382903d0a762)

Most are clumped at the bottom, meaning there are just not enough songs with higher acousticness that made it to the data set to accurately assess or even make sense of their correlation.


### Section F - Platform Popularity

![image](https://github.com/user-attachments/assets/df3c3b47-acf4-4642-8043-137061aa8ca0)

![image](https://github.com/user-attachments/assets/f8c62293-211f-4554-98b6-10a86a4a0a81)

![image](https://github.com/user-attachments/assets/8d33f8f8-9528-49a7-b5e7-5d74429254a0)

![image](https://github.com/user-attachments/assets/c66f7e37-bfe8-4b8d-a80f-2fb4781d90d6)

![image](https://github.com/user-attachments/assets/9e051e7f-4624-41e6-84ae-be2253dbc664)

![image](https://github.com/user-attachments/assets/a3f8f389-3ebb-4769-a941-b0ddee99d1d5)

![image](https://github.com/user-attachments/assets/23ae4de5-60cd-42d5-bf00-1082e4a32293)

Based on these scatterplots, the value of stream and in_spotify_playlist had most clumped up in the lower left corner, meaning least streamed songs also were least entered in the Spotify playlists and increases as the number of streams also increases, at least with a general look, the same thing appears for in_apple_playlist. However, there are not enough high values in the in_spotify_charts, in_apple_charts, in_deezer_playlists, in_deezer_charts, and in_shazam_charts, presented with many plots on the 0 value of the x-axis, to make sense of the data.


### Section G - Advanced Analysis
C# was the key with the most song entries in the data set, and it could be a direct correlation to why it also has the most streamed songs, as presented in the graph below. However, while G was the second in the category of having the most songs, it isn't the one with the second most streams. That honor belongs to songs with F#, which makes the C# case a possible outlier or a coincidence, or simply because C# is a pretty common key.

![image](https://github.com/user-attachments/assets/a311722e-3205-4b08-98b2-9baf5892bce0)

![image](https://github.com/user-attachments/assets/005c8051-9e19-4a0e-a5ad-2240e2dcc872)

As for the modes, the count was almost the same for minor and major, and the one who got the higher number of songs, in this case, the major mode songs, had the higher number of streams. Considering there are only two of the modes, it isn't enough to draw a conclusion regarding the number of streams and the number of songs from the mode that got in the top streamed data set.

![image](https://github.com/user-attachments/assets/a605b4c1-ce71-4e47-be2e-53e94453e8c9)

![image](https://github.com/user-attachments/assets/02a4b0c3-4db9-4f44-ad2d-f5104d27ecea)

Finally, using .copy, .groupby, and .sort_value, it is easy to see which artist had the most streams of the year, and using .sort_value, we can check the ranking based on the different other platforms

The Weeknd appeared in the top ten songs of all the charts and playlists except the Deezer charts. Taylor Swift also appeared in the top ten except Deezer playlists. Ed Sheeran didn't appear in both Spotify charts and Shazam charts. harry Styles also did not appear in two charts, Deezer and Shazam. Bad Bunny and Eminem didn't appear in three categories. Olivia Rodrigo only appeared thrice among the 7 categories, the same as the Arctic Monkeys, while Coldplay only appeared once, and Bruno Mars did not appear anywhere except the top streams. 

