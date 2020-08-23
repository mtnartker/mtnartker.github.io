---
layout: post
title: "Initial Introduction to SpotifyR with KMeans"
date: 2020-08-15
---

Some intro

When looking at Spotify’s API and the package SpotifyR, it is immediately easy to see its how applicable/interesting its data can be. The API and package offer a number of interesting applications, including song-specific metadata, playlist recommendations, and a slew of “biographical” information on the song/album. Additionally, users can investigate their own listening habits, finding their top 50 artists and top 50 songs over a span of four weeks, six months, or since the creation of their Spotify account. 

After playing around with the data for a little bit, I wanted to show my friends my listening habits in an aesthetic fashion. Simple tables are fine, but it was not a pretty picture to look at or easily shared to social media. Luckily, SpotifyR includes a link to each artists’/songs’ cover art. The images are artist/album-specific, allowing for identification of each artist in my top artists and the album affiliation of each song in my top songs. Each image itself is interesting, of course, but a better, more concise picture of my listening habits would include _all_ of my top 50 artists/songs. This motivation led me to create a collage of the cover art of each of my top 49 (perfect 7x7 square!) in a square collage. The result: a beautiful summary of my favorite artists in an easy-to-understand, easy-to-share image. 

<img src="https://user-images.githubusercontent.com/37127146/90769358-1871db80-e2be-11ea-8746-4d29f5a79eff.png" alt="Artist Cover Art Collage" width="1000"/>

This image is a summary of my top 49 artists in April 2020. As you can see, The Weeknd was my top artist throughout that time, followed by Tame Impala, Lil Uzi Vert, and Caribou, each of whom had released an album in the previous two months. 

After finishing this visualization project, I wanted to do something a little more analytical. I had recently started a playlist titled “[elán vital](https://open.spotify.com/playlist/6scEj1HgGsb4Mt7MU9z3gb)”, which I was attempting to give a certain “mood” while incorporating a number of different genres. This playlist was taking me out of my comfort zone; it included songs from Michael Jackson’s jazzy “Baby Be Mine” to rapper JID’s mellow trap R&B song “Hereditary”. Because of the multiple genres in the playlist, I wanted to see if songs from similar genres would be clustered together, or if some other factor would influence their grouping. To accomplish this, I used Spotify’s audio features, which attempts to describe songs using a number of different data points. 

[Spotify’s audio features](https://developer.spotify.com/documentation/web-api/reference/tracks/get-several-audio-features/) are divided into six categories: danceability, energy, speechiness, acousticness, instrumentalness, liveness, and valence (it is important to note that lyrics and/or thematic elements of songs are not included in this analysis -- yet). To find these clusters, I used an unsupervised machine learning techniques called KMeans clustering. KMeans clustering is among the most used clustering techniques in the analytics world, searching for patterns and trends in the data and grouping like data together. The technique will be implemented from the `cluster` package in R. 

To begin, I prepared my playlist’s data with `spotifyR::get_playlist_audio_features()` and created a dataframe containing only the song’s audio features. Next, I randomly selected the number of centers, or number of clusters, I wanted the algorithm to search for in my data. In this case, I selected three clusters. 

Next, I wanted to visualize my clusters using principal component analysis, a dimensionality reduction technique. Principal component analysis, or PCA, is (describe PCA). By performing PCA on our data, we now have seven variables, with the top two variables accounting for over 50% of the data. The resulting plot of the data, as labeled by the song titles, can be seen along with the eigenvectors of the original variables. Unfortunately, we can not easily see the clusters in this plot, an issue that the `factoextra` package from R easily solves. 

<img src="https://user-images.githubusercontent.com/37127146/90986905-791a4600-e554-11ea-97a2-52190467e14c.png" alt="PCA and KMeans Plots" width="1000"/>

As you can see, the two plots are essentially the same; the axes and relative location of each song, with the plot on the right easily showing the clusters. 

While that looks pretty good, let’s see if we can do any better. If you recall, I randomly chose the number of clusters for our initial investigation; let’s try to fine-tune that and rely on math to give us the optimal number of clusters. 

To start, it is important to know that there are three main methods of fine-tuning KMeans models: the elbow method, the silhouette method, and the gap statistic. The elbow method finds the number of clusters that has the minimum total sum of squares within each cluster. The silhouette attempts to determine how well each object lays within a cluster, finding the average residual distance from the center for each cluster. The gap statistic method compares intracluster variation to null distribution clustering, searching for the maximum “improvement” the additional cluster provided the model. Each plot can be seen below. 

<img src="https://user-images.githubusercontent.com/37127146/90986944-e3cb8180-e554-11ea-80b3-bfee0db96e3c.png" alt="Tuning Plots" width="1000"/>

According to our plot, the elbow method continuously decreases, meaning it suggests more clusters than less. The silhouette method has its maximum value at five clusters. The gap statistic method, on the other hand, suggests that just one cluster is optimal, suggesting that the distribution is no better than a null distribution. Because the silhouette method suggests five clusters and the elbow method supports it as a good selection, we will continue with five centers in our method. 

After rerunning the model, we get a new plot with new clusters: 

<img src="https://user-images.githubusercontent.com/37127146/90986969-1bd2c480-e555-11ea-929a-c7bb0477c6d2.png" alt="KMeans Plots" width="1000"/>

In cluster one in the top right, we have songs from iconic artists such as Michael Jackson and The Isley Brothers to more current, lowkey artists such as Yves Tumour and KAYTRANADA. To the second cluster, it is a number of recent songs with a notable instrumental uptick in its song, giving this cluster the highest mean acousticness and mean instrumentalness of our five clusters. The third cluster contains the most danceable songs of any of the five clusters, including songs from Bobby Caldwell to Clairo. In cluster four, the four songs are by artists more known for their production and instrumentals than their lyrics, containing the highest mean valence and mean energy of any cluster. The final cluster has the highest mean speechiness along with the lowest mean valence of the five groups, containing odes to failed relationships and the beginnings of new ones. 

As you can see in the above plot, two of our clusters overlap. While this seems incorrect, it is important to remember that the current plot is only using two of our seven variables created during the dimensionality reduction from PCA. These two variables account for 54% of the data, but 46% of the data is unplotted. The KMeans analysis of elán vital’s resulted in within cluster sum of squares of 0.43 for cluster one, 0.56 for cluster two, 0.66 for cluster three, 0.36 for cluster four, and 0.44 for cluster five with 69.4% of the variation in the data explained by the clusters.

Overall, these strong measures show that even though elán vital has a consistent “mood” throughout the playlist, there are still specific groupings within it. Finish conclusion
