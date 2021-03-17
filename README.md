# Spotify-Discover-Together
I and a few friends developed a web-application using clustering algorithms to create a brand new playlist of new songs for two individuals to listen to together. Check it out at dicover-together.com and this is the documentation of how we did it!

## Goal
Trustin and I analyzed and synthesize 2 users' Spotify recent taste profiles so that we can create a playlist filled with brand new song recommendations for both of them to discover together. The playlist we curate will automatically end up in both of their spotify libraries. A novel way for people to socialize because the playlist is unique to them and can create a special experience for both to find new music together that matches their music tastes.

## Data Collection
We used Spotipy, a lightweight Python library for the Spotify Web API and created a Spotify Developers app to gain authorization to Spotify profile data. Here is the official Spotipy documentation: https://spotipy.readthedocs.io/en/2.13.0/

Also forked authorization flow codes from Plegas Gerasimos's repo that contained the codes for granting authorization flows which can be found here: https://github.com/makispl/Spotify-Data-Analysis

Then we created a code that authorizes access to a user's Spotify profile to extract their top 50 medium term songs using Spotipy's current_user_top_tracks() function. medium_term was the most optimal time frame since short_term songs were too short (~1 week of songs) and long_term was too long (basically a user's all time song history). The user's top 50 medium term songs were then extracted into a csv file, and this was repeated for the second user.

## Clustering
Now that we had both users' top 50 songs, we can use those tracks for reference to recommend new songs that are curated for both people's recent music tastes. K-means clustering was the method we chose so that we could group each song from our combined songs into a cluster (sub-genre/type of song) regarding their sonic features that are officially provided by Spotify. These features include: danceability, energy, key (musical key), loudness, mode (minor or major), speechiness, acousticness, instrumentalness, liveness, valence (how positive/happy/cheerful it sounds), and tempo.	We chose to focus on danceability, energy, speechiness, acousticness, valence, and tempo after concluding the other features were noise to our analysis. We also chose to have k=20 even though the elbow method showed that optimal k=6 since we wanted to have more specified clusters so that we could base our recommended songs off less generalized clusters.


![1_MPoDs6szyqxykHcKAv9e5A](https://user-images.githubusercontent.com/68137802/111539026-a6b22100-872a-11eb-9295-365005c8e87b.png)

![1_MPoDs6szyqxykHcKAv9e5A](https://user-images.githubusercontent.com/68137802/111539039-ac0f6b80-872a-11eb-9007-1981ae54979b.png)
![1_uOOQ3YWo9o2IxH9SzjwojA](https://user-images.githubusercontent.com/68137802/111539050-af0a5c00-872a-11eb-845a-f31ea69a6594.png)


## Building the Playlist Creator
Once we got a list of songs for each cluster, we fed each list into Spotipy's recommendation() function which takes in 5 seed tracks to base its recommendation from. For the clusters that contained more than 5 songs, we randomly chose 5 from its list to represent the cluster. The clusters that contained less than 5 songs, we left it as is. We ran the recommendation function for each cluster, and each time it produced 1 recommended track for the clusters that contained less than 5 songs and 2 recommended tracks for the clusters that contained 5 songs. We wanted to put more weight onto the clusters with more songs since that meant we generally enjoyed songs that fit in those clusters moreso than the smaller clusters. After fetching all the recommended tracks, we put it into a list and called another Spotipy function that automatically creates and places a playlist with a given list of songs straight to the user's Spotify library.

## Conclusion
<img src="Photos/created_playlist.png" width="800" > 
Trustin and I found songs in the playlist that we both enjoyed and we thought matched both of our tastes in music. Trustin and I had commonalities in rap and electronic music and our algorithm was able to find the overlap in the subgenres within those that we enjoyed and found songs that were similar. Overall, our project was a success, and going forward we are looking to develop this idea into a functioning web app that anyone and their friend can use to find music together.
