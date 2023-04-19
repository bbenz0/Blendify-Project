# Blendify
\
Note: This public repository ONLY includes the readme file for the Blendify Project. The files containing code are on a separate private repository due to potential risk of leaking credentials. Please contact me if you'd like to see the code :)
\
\
Blendify is a recommendation engine for Spotify created by myself and two friends to create a unique experience for people to discover brand new music with a friend of their choice.
\
\
[user1.ipynb](https://github.com/bbenz0/blendify/blob/main/user1.ipynb) - get user 1's top 50 songs
\
[user2.ipynb](https://github.com/bbenz0/blendify/blob/main/user2.ipynb) - get user 2's top 50 songs
\
[blendify_v2.ipynb](https://github.com/bbenz0/blendify/blob/main/blendify_v2.ipynb) - data analysis and working model 
\
[app.py](https://github.com/bbenz0/blendify/blob/main/app.py) - working web app

## Objective
We took inspiration from Spotify's 'Blend' feature that allows users to merge their unique music palettes and create a new playlist with their mixed tastes; however, this feature doesn't allow users to view any recommendations. We also found that 'Discover Weekly' did a great job of creating recommendations for individual users on a weekly basis. So, we decided to pull inspiration from both these features that allows two users to create a blended playlist of recommendations directly into a user's library.

## Data Collection
For our data collection process, we used Spotipy, a lightweight Python library created by Spotify, that allows us to connect and interact with Spotify's API data. 

More information about the library can be found here: (https://spotipy.readthedocs.io/en/2.22.1/)

We also took inspiration from two articles we found online that allowed us to create authorization flow codes and to analyze our data more efficiently:

https://github.com/makispl/Spotify-Data-Analysis
\
https://medium.com/@arjunravulareddy/generating-a-two-person-spotify-discover-playlist-using-data-science-38fddacf7ff6

After, we wrote in code that allowed us to authorize access to a single user's Spotify library to retrieve their top 50 short term (~ 4 weeks) songs using Spotipy's current_user_top_tracks() function. We decided that short_term was the best time range for our model because medium term (~6 months) and long term (~1 year) songs were too long. After extracting the user's top 50 short term songs, we converted the data into a csv file. We ran the process again for the second user.

## K-Means Clustering
From the two user's top 50 playlists, we used the tracks in each playlist to recommend new tracks curated specifically for both users' based on their recent music preferences. We decided that K-means clustering would be the most optimal method for this by grouping each song from our combined playlist into a cluster, which we defined as a sub-genre. The audio features that are available to us by Spotify allowed us to classify the songs based on features we found relevant. Essentially, we found that audio features such as danceability, energy, key, loudness, speechiness, acousticness, instrumentalness, liveness, valence, tempo, and duration_ms were all relevant metrics.

![pic 2 for project](https://user-images.githubusercontent.com/114318061/232260604-7ec89ab7-7fda-408b-855b-f688d6e8979d.png)

Since Spotipy's recommendations() function only allows 5 seed songs at a time, we decided to choose our k = 20 (20 clusters * 5 songs = 100 songs). The elbow method is a popular way for running K-means clustering with an average k = 6-8; however, our large k = 20 allows us to produce more clusters that represent specific subgenres. In simpler terms, a smaller k would result in fewer clusters and more songs, while a larger k helps us have more clusters and fewer songs.

![pic 3 for project](https://user-images.githubusercontent.com/114318061/232260624-62c5e1a4-0148-4d54-b953-83486b156157.png)
\
![pic 4 for project](https://user-images.githubusercontent.com/114318061/232260659-5e42730e-23c1-43f9-add6-6116d97ed1a1.png)
\
![pic 5 for project](https://user-images.githubusercontent.com/114318061/232260681-27485457-d8f4-4d2b-8898-ff721567b6bd.png)


## Building the Recommendation Model
Finally, with the list of songs from each cluster, we used Spotipy's recommendations() function to take in 5 seed tracks for each list to come up with new recommendations. Because of the limitations of Spotipy, we randomly selected 5 songs for clusters with more than 5 songs. The clusters with only 1 song usually indicates that a unique song (outlier) is being played by the user repeatedly such as a white noise track to fall asleep or a 8 hour loop of a lo-fi song for studying. The qualities of the 1 song clusters, therefore, aren't relevant to our recommendation model. So we decided to remove them whenever they come up.

Our goal is to select 1 song recommendation from clusters that have less than 4 songs, and 2 recommendations from clusters that contain 4-5 songs. This approach reflects our assumption that larger clusters imply a greater likelihood of enjoying similar songs. To account for this preference towards larger clusters, we adjust the weighting of our song recommender.

## Conclusion
Now, with all the recommended tracks, we put them into another list and used a Spotipy function that automatically adds the new playlist directly into the Spotify user's library.

We were integrate this recommendation model into a functioning website application here using PythonAnywhere: http://bendify.pythonanywhere.com/
\
Feel free to check it out and make your own blended playlists!

![pic 1 for project](https://user-images.githubusercontent.com/114318061/232260565-5e6950af-960b-42a9-a9a3-d2e6a5f2c115.png)



