# Spotify-Charts-2023-EDA

We were tasked to commit an exploratory data analysis on a dataset containing information about popular tracks on Most Streamed Spotify Songs 2023 [(spotify-2023.csv)](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)

## Dependecies
For this task, the following 
- Jupyter Notebook
- Pandas Library
- Numpy Library
- Mathplot Library
- Seaborn Library

## Guidelines and Questions

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
How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

### G. Advanced Analysis
Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.


# ---------------------------------------------------------------

### Section A
First, importing the required libraries is imperative to manipulate, explore, and clean the data, and then import the dataset using pd.read_csv function. the syntax was a little bit different as the traditional, as the .csv file was not utf-8 compatible, so the latin-1 syntax had to be added. Then, using the len function, with axis=0, it is easy to determine the number of rows and axis=1 for the number of columns as well, presenting 953 rows and 24 columns. To show the data types of each column, the function dtype was used and the results are as follows

- track_name              object
- artist(s)_name          object
- artist_count             int64
- released_year            int64
- released_month           int64
- released_day             int64
- in_spotify_playlists     int64
- in_spotify_charts        int64
- streams                 object
- in_apple_playlists       int64
- in_apple_charts          int64
- in_deezer_playlists     object
- in_deezer_charts         int64
- in_shazam_charts        object
- bpm                      int64
- key                     object
- mode                    object
- danceability_%           int64
- valence_%                int64
- energy_%                 int64
- acousticness_%           int64
- instrumentalness_%       int64
- liveness_%               int64
- speechiness_%            int64

The object pertains to strings and int64 to int values. Some columns must be changed from objects to numerics for mathematical functions such as Central Tendency Functions and Correlation functions. This change however was done after identifying the NaN values and changing them. There are only two columns with NaN values, the in_shazam_charts and key columns, thanks to the help of the combination of .ismull function and .sum function. And with the use of .fillna, the null values were change to either 0s for the in_shazam_charts column, or unknown for the key column. And then after a few manipulation and replacement of commas into nothing, as well as handling a very specific case, using pd.to_numeric, the data types of certain columns like the streams, in_deezer_playlist and in_shazam_charts were replaced to int64
