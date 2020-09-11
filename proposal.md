---
layout: default
title: "Project Proposal"
permalink: /proposal/
---
[Back to Homepage](https://santoshnukala.github.io/jukebot/)
## Project Proposal STOR 565 - Jukebot
*Member List: Sahith Reddy, Anand Hande, Omkar Apte, Rajeev Rajendran, Santosh Nukala, Krithik Chitla*

```markdown
Team name:
Jukebot
Team members:
Krithik Chitla, Sahith Desham, Santosh Nukala, Omkar Apte,
Rajeev Rajendran, Anand Hande
Date:
09-11-2020

Motivation and Goals:
- Our goal is to utilize some of the techniques described below to generate midi files, 
specifically for video game music. As a group we were interested in creating new songs, 
and video game music seemed free enough in terms of patterns and sound range. 
We also thought it may be difficult to have a computer produce music containing human voices. 
Upon research, we found that there has been success in recreating Baroque music due to the 
heavy pattern dependencies which is the same for video game music. In terms of goals, we are aiming 
for something that could be considered “music” if an arbitrary person were to listen to it.  
This could have cool applications such as creating custom music as a player plays a video game or 
simply treating it as general-purpose music.

Techniques:
- The method we use to generate new music is subject to change, but after some 
preliminary research two methods look promising. One method is using autoencoders, which 
are Artificial Neural Networks (ANN) that use unsupervised learning to produce an output that
is as close as possible to the input data. The aim is for the autoencoder to learn the representation
of the data and eliminate signal noise if present. This is useful for dimensionality reduction,
which we don’t need in our case because we are going to sample the MIDI in pre-recorded intervals. 
The optimal choice would be to divide the measure into 96 segments used for sampling because all of
the common time signatures divide 96 (2,3,4,6,8,12,16). We will exclude the use of music in odd-time
for the purposes of this project because it is much rarer and musically more complex to deal with, 
which will make the autoencoder learn much slower and probably end up making a worse generative model.

- The other technique is Principle Component Analysis which will likely be used in conjunction with
the autoencoder to make a better generative model. PCA is based on the principal axis theorem from
classical mechanics, and can loosely be thought of as fitting a p-dimensional ellipse to the data.
Each of the principal components is chosen such that the direction is perpendicular to all the others
and maximizes the variance of the projected data. This helps with dimensionality reduction as we can
eliminate features of the music that are emergent properties and thus are not important to the generative
algorithm.

Data Source:
- The site from which we are extracting our data from is www.vgmusic.com, also called The Videogame Music
Archive or VGMA. We selected the VGMA as our datasource as it is perhaps the largest archive of MIDI sequences
of video game music with over 30,000 MIDI sequences hosted on the site. Having our data in the MIDI format
is critical as the music we are using is best stored in such a format. The MIDI sequences on the VGMA are
typically General MIDI and GM2 standard. We are planning on sampling music from a particular game across 
different platforms using this site which will be remarkably easy due to it’s range of selection
```
