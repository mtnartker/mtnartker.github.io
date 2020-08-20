---
layout: post
title: "My First Work with Spotify's API and SpotifyR"
date: 2020-08-15
---

Some intro

When looking at Spotify’s API and the package SpotifyR, it is immediately easy to see its how applicable/interesting its data can be. The API and package offer a number of interesting applications, including song-specific metadata, playlist recommendations, and a slew of “biographical” information on the song/album. Additionally, users can investigate their own listening habits, finding their top 50 artists and top 50 songs over a span of four weeks, six months, or since the creation of their Spotify account. 

After playing around with the data for a little bit, I wanted to show my friends my listening habits in an aesthetic fashion. Simple tables are fine, but it was not a pretty picture to look at or easily shared to social media. Luckily, SpotifyR includes a link to each artists’/songs’ cover art. The images are artist/album-specific, allowing for identification of each artist in my top artists and the album affiliation of each song in my top songs. Each image itself is interesting, of course, but a better, more concise picture of my listening habits would include _all_ of my top 50 artists/songs. This motivation led me to create a collage of the cover art of each of my top 49 (perfect 7x7 square!) in a square collage. The result: a beautiful summary of my favorite artists in an easy-to-understand, easy-to-share image. 

<img src="https://user-images.githubusercontent.com/37127146/90769358-1871db80-e2be-11ea-8746-4d29f5a79eff.png" alt="Artist Cover Art Collage" width="500"/>

This image is a summary of my top 49 artists in April 2020. As you can see, The Weeknd was my top artist throughout that time, followed by Tame Impala, Lil Uzi Vert, and Caribou, each of whom had released an album in the previous two months. 

After finishing this visualization project, I wanted to do something a little more analytical. I had recently started a playlist titled “elan vital”, which I was attempting to give a certain “mood” while incorporating a number of different genres. This playlist was taking me out of my comfort zone; it included songs from Michael Jackson’s jazzy “Baby Be Mine” to rapper JID’s mellow trap R&B song “Hereditary”. To determine how related each song in the playlist was to each other, I needed to find the overall “mood” of each song, which could be pseudo-accomplished using metadata. 

Spotify’s song metadata, or XXX, is divided into seven categories: danceability, energy, loudness, speechiness, acousticness, instrumentalness, liveness, and valence. Using this metadata, I could determine just how “related” the songs in my playlist are to each other to measure how well I was doing in curating this playlist. (Later, add other playlists to “check” how I’m doing using a rap playlist (don’t miss), a summer playlist (summer on northwood or honda ‘08), and a party playlist (we bumpin’ or what)). To find this relation, I used hierarchial clustering, an unsupervised machine learning algorithm that searches for patterns in the data and groups specific patterns together, from the `cluster` package on the metadata of each song in the playlist. I also added the tempo variable into my model, taking the pace of the song into account. The result: a beautiful tree with XXX (include model performance here).


Image


If you have never seen a tree diagram like this before, it is relatively easy to interpret. The closer two songs are to each other, the more related/similar they are. Continuing, if the root node of their split is nearer the bottom the tree, the more related they are. For example, let’s take two closely-related song (“” and “”), two moderately-related songs (“” and “”), and two relatively-unrelated songs (“” and “”). “” and “” are next to each other on the tree with their root node close to the bottom, making them closely related. This relation makes sense; ___ go on about their actual data____. (possibly go on about their actual era and artists). These two songs have the strongest relation of any in the playlist. 

Next, look at “” and “”. 

Finally, check out “” and “”. 

