---
layout: post
title:  "Audio Sync, Part 4 - Major Performance Considerations + Resources"
date:   2021-5-13 10:44:15 -0000
excerpt_separator: <!-- more -->
banner: /assets/bit-to-beat_new-hit-zone.gif
---
**Written by Michael Berger**

&nbsp;&nbsp;&nbsp;&nbsp;Bit To Beat is a rhythm-RPG hybrid where player actions, enemy attacks, and obstacle movements only happen to the beat of a song. Underneath the genre conventions, art style, and even the music itself lies a simple concept; Game actions that are tied to audio output, otherwise known as audio syncing. In today’s blog post, we will be talking about a major consideration we have made when implementing the audio sync system in our game and listing the resources that we referenced to make our system possible.

<!-- more -->

### Utilizing Callback Functions

&nbsp;&nbsp;&nbsp;&nbsp;If you follow the advice provided in these blog posts down to the exact phrasing, you may be creating many game objects that are constantly checking the audio timing of the song to determine whether or not they can perform some action. This approach completely works as intended, but it does result in a game where every game object is checking the same value over and over again within the same second. For small game scenes with few objects that react to the music (ex; the Bit To Beat alpha versions), this is an acceptable approach. However, for the performance of game scenes that could have dozens of objects which must react to the beat / notes in the song, this can get heavy to calculate. How would one go about removing all these song position checks while still having the same result?

&nbsp;&nbsp;&nbsp;&nbsp;The answer is with callback functions. In programming terms, callback functions are references to pieces of code that can be run at an unspecified time in the future. These types of functions are most common in programming for websites, where user actions are not guaranteed to happen instantly and the connection between the web server and the web client (a browser) could drop out at any moment. However, callback functions are not exclusive to that domain, and the functionality they provide can be used to synchronize actions to a song or beat.

&nbsp;&nbsp;&nbsp;&nbsp;In our game object that handles starting and stopping a song (`SongManager`), we created a `UnityEvent` class instance. This is a special class that acts as a list of callback functions. At the beginning of every scene, all of the game objects that want to perform actions on a beat pass in a method that follows a special definition into the `UnityEvent` instance. Then, when the song for the scene begins playing, the `SongManager` class carefully tracks the beat and notes of the song. When a beat or a note occurs, the `Invoke()` method of the `UnityEvent` instance is called, causing all of the passed-in methods to be executed all at once. This gives off the strong illusion of multiple game objects in the scene following the music when, in fact, only one game object (`SongManager`) is keeping track of the audio.

### Resources

&nbsp;&nbsp;&nbsp;&nbsp;This set of posts has two major resources, both of which deal with coding rhythm games in the Unity game engine. The first is Yu Chao’s, “Music Syncing in Rhythm Games,” which covers how to keep track of songs being played in Unity scenes and what to do to make game objects naturally move in time to those songs. The second is Graham Tattersall’s, “Coding to the Beat - Under the Hood of a Rhythm Game in Unity,” which covers many of the surrounding classes that encapsulate the music-tracking functionality and how to overcome some of the smaller problems that arise with creating rhythm game systems.