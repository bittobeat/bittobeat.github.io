---
layout: post
title:  "Audio Syncing, Part 3 - Minor Issues (and Their Solutions)"
date:   2021-1-25 13:34:56 -0500
excerpt_separator: <!-- more -->
banner: /assets/bit-to-beat_hit-zone.gif
banner_alt: Gif of notes moving towards a hit zone
---
**Written by Michael Berger**

&nbsp;&nbsp;&nbsp;&nbsp;Bit To Beat is a rhythm rogue-lite, where player actions, enemy attacks, and
obstacle movements only happen to the beat of a song. Underneath the genre
conventions, art style, and even the music itself lies a simple concept; Game
actions that are tied to audio output, otherwise known as audio syncing. In
today’s blog post, we will be talking about the solutions to a couple of audio
syncing issues that we have run into in developing Bit To Beat.

<!-- more -->

### Minor Issues (and Their Solutions)

&nbsp;&nbsp;&nbsp;&nbsp;If you happen to be following in the footsteps of what
we are doing to enable audio syncing for Bit To Beat, you may run into the same
issues we have had. Here are several of the problems we encountered and what we
are doing to curtail them.

#### Uneven Appearance of Same-Beat Objects

&nbsp;&nbsp;&nbsp;&nbsp;If multiple game objects are performing the same state
transformation at the same time in a single audio clip (read; on the same beat
of a music clip), you may notice they will occasionally look obviously
different. The ultimate timings of objects will not be altered, but visually
one beat-based object will appear smaller than the other or will appear
further away than its same-beat counterpart. This is not a deal-breaker, but
it looks off and digging into the solution may leave you more frustrated than
it is worth.

&nbsp;&nbsp;&nbsp;&nbsp;The solution to this error is two-fold. Firstly, the
correct way to time game events to audio playback is to interpolate between the
two states. The errors you are receiving are most likely the result of you
trying to treat differences in the audio timing as a delta variable. This does
not work, mainly because the audio time is updating so fast and so frequently
that each object will have a different delta audio time value to work with.

[![An example of a Delta Time variable, taken from ParallelCube.com](/assets/delta-time-example_parallelcube.png)](https://www.parallelcube.com/2017/10/25/why-do-we-need-to-use-delta-time/)

&nbsp;&nbsp;&nbsp;&nbsp;Secondly, you can still get timing errors with
interpolation because the audio time variable itself is rapidly changing. How we
solved this second issue is that in our Song Manager script we simply poll the
AudioSettings.dspTime variable and then have all of our song-based objects
reference the public variable that stores that polled value.

#### Jam-Packed Update Methods

&nbsp;&nbsp;&nbsp;&nbsp;If you have some particularly busy game object update
methods, you may notice that their performance in relation to other
audio-dependent game objects is inconsistent. At the exact moment a beat is
played in the song, an object may begin transforming while another object starts
its transformation a frame or two later. This is much more of a serious issue
than the objects simply appearing to be mismatched; They begin performing
actions later than intended, and this can cause massive disconnects between the
music and game actions.

&nbsp;&nbsp;&nbsp;&nbsp;Our solution to this error was to use finite-state
machines. In essence, finite-state machines are metaphorical representations for
how an object behaves. An object can have several, “States,” (read; actions it
can perform) and each state can have various, “Inputs,” (read; other actions
which instigate the object to take an action). A good example of a finite-state
machine is a turnstile. It has two states (“Locked” and “Unlocked”) and two
inputs (“Push” and “Insert Coin”). When the turnstile is in its Locked state,
pushing on it will result in nothing happening. However, inserting a coin will
change the turnstile from Locked to Unlocked, whereupon pushing it will result
in the turnstile moving its metal arm.

[![The finite-state machine of a turnstile, taken from Wikipedia.com](/assets/wikipedia-turnstile-fsm.png)](https://en.wikipedia.org/wiki/Finite-state_machine)

&nbsp;&nbsp;&nbsp;&nbsp;To implement finite-state machines into our game, the
steps we needed to take were fairly simple. By using conditional statements in
our game code, we are able to forgo performing any unnecessary or complex
functions unless they were necessary for the current state the game object
happened to be in.

### Next Time

&nbsp;&nbsp;&nbsp;&nbsp;In the next blog post, we will be discussing one major
performance consideration we have implemented in our audio syncing system.
Additionally, we will recap the major lessons of this series and list the
resources that were consulted when we first began creating our audio syncing
system.
