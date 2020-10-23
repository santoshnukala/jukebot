---
layout: default
title: "Weekly Logs"
permalink: /weeklylogs/
---
[Back to Homepage](https://santoshnukala.github.io/jukebot/)
## Weekly Logs

Here is where you can find our weekly logs

### Week 7 Summary 

[Here is the link to our generated music on YouTube](https://youtu.be/3TLGdOGuqKg)
```markdown
Team name:
Jukebot
Team members:
Krithik Chitla, Sahith Desham, Santosh Nukala, Omkar Apte, Anand Hande
Date:
10-09-2020

Team roles for this week (write down name):
- Facilitator(s): Anand Hande
- Recorder(s): Anand Hande
- Deliverer(s): Santosh Nukala
- Planner(s): Omkar Apte

Describe briefly what the main goal of your team is (so the peer reviewer has some
context).
- The overarching goal of our project is to have a neural network that will output 
midi files corresponding to the retro video game music.  Specifically, over these 
two weeks, we focused on feeding our neural network different types of training data
in order to analyze how it responds to different midi song configurations.  Through 
this we are in the process of determining ideal tuning parameters and getting the 
information we can incorporate into our write-up.  

I. What was done this week regarding the project: If you want to include code include
this in the Appendix. Describe what the group did (including contributions of individual
team members) with regards to the group project this week. Give enough details so I
understand what you folks have been doing over the week. Include dates of your
meeting(s) and who met on these days.

- This week focused on stress-testing our LSTM network by running it with various parameters
and training data. We explored running it with sequence lengths (the number of previous notes
that the network holds to make inferences) of 50, 100, and 200. We also ran the network for
400 epochs, creating midi outputs every 50 epochs. The training was handled mostly by Anand
and Santosh due to connectivity issues, but Sahith and Krithik helped when they could. Omkar
used his music theory knowledge to design the network tests and evaluate the outputs. The 
network was trained on two separate data sets in a controlled manner. All of the training 
data was selected from the videogame I Am Setsuna, which was chosen for the music style 
and ease of analysis. All of the song files could be easily converted to MIDI for analysis
since they were composed for a single piano. 
The first group of songs was denoted the “simple” group. The songs in this training set were
chosen for the following criteria: they were composed in 4/4 time (which is the overwhelmingly
simplest and common beat structure in Western music) and did not change tempo (pace of the song)
or key signatures (allowed notes) during the song. From a theoretical perspective, these songs 
are simple due to a constant pace, simple beat structure, and simple note selection. The 
second group of training data contained slightly more “complex” songs. These songs were 
allowed different time signatures (beat structure) that are more complex, but we restricted 
odd time signatures due to sampling issues in our network, which we discussed in previous logs.
The outputs that we got during our initial runs of 200 epochs and fixed sequence length of 
100 were promising. 50 epochs turned out to be insufficient because the networks played a 
few different notes in key, and then got stuck on the same note for the rest of the sample. 
After 100 epochs, the network had figured out a sense of key (mostly picked up C major) and was
able to combine melody lines with chords. However, it got stuck on the same note again after
about 30 seconds. It also struggled with rhythm because it would double and triple play the
same note along its melody lines, which indicates that the model cannot capture transitions 
effectively.
At 150 epochs, the output became more interesting. The model still didn't quite capture note 
transitions, but it got a little better at rhythms and layering melody and harmony together.
It even figured out how to change key signatures (allowed note groupings), but this put the 
inference stage in an improvisation scenario, leading to sour notes being introduced in the
output. However, this sounds more “human” because it sounds more like a person experimenting 
with the notes during composition. The range of pitches was also expanded to include notes 
from the high and low ends of the piano. 
The final 200 epoch output was extremely promising. The song had key changes, moving melody 
lines, and matching harmonies. There were attempts that sounded like “improv” which led to sour
notes and dissonance, but not nearly as bad as before. At times it was clear the network got 
stuck in the inference stage because the melody got stuck on the same note, but unlike previous
runs, it was able to “unstick” itself. All in all, this output produced 4 or 5 complete musical
ideas in 2 minutes, which is amazing considering that the only training data we gave it was 6
simple songs. By complete musical idea, we mean that there was a development of melody with clear
phrasing, supported by an underlying harmony.
 

II. What were obstacles faced if any in working on the project? This could be technical
(like not being able to implement or understand particular techniques) or time issues
(midterms for other courses etc).

- While we learn a lot about the neural network every single time, we still sometimes feel
that we do not have a 100% thorough explanation for why the neural network output is the way
it is in every case.  Our understanding keeps expanding every time we try new parameters
so we need to keep on doing this in order to both learn more and have more to talk about
in our write-up.  Another aspect of understanding the behavior is having some understanding
of music theory to analyze the neural network’s output.  Some members of our group have more
music theory knowledge than others so this is forcing different group members to do different
tasks.  Our last obstacle directly relating to the project is that since we are dealing with 
music files, it is hard to have a firm, objective definition of which songs are “good” and which
are “bad”.  We are in the realm of unsupervised learning, so we are trying to fall back on 
music theory fundamentals to give ourselves a somewhat objective comparison. 
In terms of technical issues, we have faced many during the past 2 weeks.  One of our biggest 
issues we are facing is that some of our group members do not have constant access to the internet.
2 of our group members, Sahith and Krithik, are in India and have sporadic internet access due 
to flooding.  Although we are trying to keep them updated, it has made it difficult for them to
work on the project since such a large part of our work is waiting for a neural network to train
for 10-12 hours.  If their internet goes out training, then they lose all progress (since we are 
using Google Colab to train the neural network).  In addition to this, Omkar has been without power
and internet since 10/20 so he has to go to a café in order to use the internet.  These factors 
combined with the fact that it takes 10-12 hours for our neural network to run have slowed down our
work.  As of right now Santosh and Anand are the only ones with constant internet access so we plan
to have them train the neural network on various inputs.  Those who do not have continuous internet
access will most likely be given other tasks such as working on the write-up.  
  


III. What is the plan for the next week including what each team member is planning
to work on in the next week. Describe goals and potential timelines (“ I plan to
finish understanding x to see if it can be implemented for our project by
Wednesday etc”. )

- Our group has had success meeting before or after class every Monday, Wednesday, Friday.
We plan to meet on 10/24 to reassess the results of the music training each of us did.  
Both Santosh and Anand are going to report their results of training with over 200 epochs 
(goal is to run up to 400 overnight) with varying sequence lengths.  We have tried training 
the network while varying epochs and sequence length separately, so we are curious to see the
output if we vary them together.  After discussing our findings, we will update our group 
members who cannot make the meeting due to internet issues.  Anand wants to look into 
unsupervised methods of clustering that are suited for midi files so that we can have a more
objective way to train different groups.  He plans to look into it over the weekend and report
what he finds on Monday.  We need to assess the results of the training before assessing the 
plan for subsequent meetings, but we are confident it will involve discussing what files we 
should give the network on next to gain the most insight.  Furthermore, the internet struggles
our group is experiencing inhibits us from making solid goals a week in advance.  Over the 
weekend each group member will update the group on their situation so we can make more 
concrete plans for these subsequent weeks.     
```

### Week 5 Summary 

```markdown
Team name:
Jukebot
Team members:
Krithik Chitla, Sahith Desham, Santosh Nukala, Omkar Apte, Anand Hande
Date:
10-09-2020

Team roles for this week (write down name):
- Facilitator(s): Anand Hande
- Recorder(s): Anand Hande
- Deliverer(s): Santosh Nukala
- Planner(s): Sahith Desham
- Team Contact: Omkar Apte


Describe briefly what the main goal of your team is (so the peer reviewer has some
context).
- The overarching goal of our project is to have a neural network that will 
output midi files corresponding to the retro Mario music midi files that are inputted.  
Specifically, over these two weeks, we focused on how to analyze midi files and get them 
into a suitable format for the neural network.  As of Friday, October 9th, we have 
implemented an LSTM (Long Short-Term Memory) neural network and have tested it on ideal,
one instrument piano data.  We have attached the midi file outputted by our neural network 
which you can download and play.  

I. What was done this week regarding the project: If you want to include code include
this in the Appendix. Describe what the group did (including contributions of individual
team members) with regards to the group project this week. Give enough details so I
understand what you folks have been doing over the week. Include dates of your
meeting(s) and who met on these days.

- Before these last two weeks, we intended to use a midi-to-CSV file converter to work 
with the songs as CSV files since we thought this would be easier.  On Wednesday, September
30th, all of us met for about 90 minutes to discuss what we found after trying to convert 
midi files to CSV.  We all had trouble converting the files, and we also thought it would 
be bad if our neural network outputted CSV files which we were unable to convert back into 
the midi format.  We then decided to work on analyzing midi files in their original format.  
On Saturday, October 3rd, we met up again to discuss our progress on analyzing midi files.  
Sahith found a tutorial in which someone was parsing midi files to be inputted into a neural 
network so we decided it would be best if we could each implement this and understand the tutorial 
fully.  This way we could have a good framework for our implementation.  On Friday, October 9th, 
we met up and confirmed that we have a working LSTM neural network which produced a midi file 
given ideal training data (one instrument piano songs). Our goal is to test our neural network
on more complicated music (specifically various combinations of Mario music) to see how it 
performs and where the shortcomings are. 

II. What were obstacles faced if any in working on the project? This could be technical
(like not being able to implement or understand particular techniques) or time issues
(midterms for other courses etc).

- As we progress we are realizing the complexities and nuances of this project and it 
takes a lot of time to understand each step of the process.  For example, the aforementioned
tutorial we implemented used an LSTM neural network.  We have not covered neural networks yet
so there is a lot of individual research that needs to be done, and we hope that we can get a
better understanding by discussing the tutorial as a group on October 10th.  Lots of our group
members had midterms early in the week of Monday, October 5th so it was difficult for us to find
a time to meet and discuss what we have implemented so far.  After our meeting on Friday, October
9th we realized it takes a long time for the LSTM neural network to be trained (running it on a
full dataset would take 12 hours).  We will most likely have to stop training early when we work
on it during the day to find the optimal combination of songs and have it run overnight.   


III. What is the plan for the next week including what each team member is planning
to work on in the next week. Describe goals and potential timelines (“ I plan to
finish understanding x to see if it can be implemented for our project by
Wednesday etc”. )

- By the next weekly log, we want to improve our neural network to work for complex Mario
songs with multiple instruments (or use techniques to simplify the songs so they work with
our neural network).   For this, we need to improve our understanding of neural networks
and understand each result for each case. To achieve this, we are planning on meeting twice
every week. We plan on meeting Sunday, October 11th.  Prior to this meeting, Krithik and
Santosh will have tried different combinations of Mario songs with our LSTM neural network 
to see which songs give ideal outputs and how we need to improve our neural network.  
Anand is going to study LSTM neural networks to better understand potential shortcomings 
from training the neural network with more complex songs.  Omkar’s internet is out due to a 
Spectrum outage and Sahith is busy preparing for a job interview, so we plan to update 
everyone by the next meeting on Sunday, October 11th.  

```
[Here is the link to our test midi file](https://drive.google.com/file/d/1S6EOSqMO5RBsU8qi0gCpO3vosVbunZE6/view?usp=sharing). 
The midi file needs to be downloaded to be played. In the future, we plan to upload our files to YouTube.


### Week 3 Summary 

```markdown
Team name:
Jukebot
Team members:
Krithik Chitla, Sahith Desham, Santosh Nukala, Omkar Apte, Anand Hande
Date:
09-25-2020

Team roles for this week (write down name):
- Facilitator(s): Omkar Apte
- Recorder(s): Anand Hande
- Deliverer(s): Santosh Nukala
- Planner(s): Omkar Apte
- Team Contact: Anand Hande


Describe briefly what the main goal of your team is (so the peer reviewer has some
context).
- Currently the main goal of our team is to analyze some of the datasets we found for
video game songs. Specifically ,we need to analyze some retro mario songs (these
are in midi but we are going to try converting them to CSV) and start cleaning the
data. This way we can get a feel for how we need to manipulate the data so that it is
in the optimal format for the neural network.

I. What was done this week regarding the project: If you want to include code include
this in the Appendix. Describe what the group did (including contributions of individual
team members) with regards to the group project this week. Give enough details so I
understand what you folks have been doing over the week. Include dates of your
meeting(s) and who met on these days.

- The whole team met on 9/16 and 9/23. We made a rough weekly plan for the semester. The 
first goal was to have the data downloaded and processed into a format that we can use 
(ended up choosing CSV) by 9/25. The next step is to understand autoencoders so we can implement one. 
To this extent we found several resources on the internet and a working code example that is 
on Github. In the following couple weeks we plan to have the autoencoder in a state where it can 
start training. What to do after this point obviously depends on the results of the training, so 
we stopped there. We created a Jupyter notebook on Google colab, where we will be running our code, 
so that we can leverage Tensorflow and CUDA using Google’s hardware. Additionally we set up a way for 
us to log code changes and staging so that we could check it before deploying into a master notebook.

II. What were obstacles faced if any in working on the project? This could be technical
(like not being able to implement or understand particular techniques) or time issues
(midterms for other courses etc).

- One of the obstacles we need to tackle is how to factor in different instrument songs in our midi 
files.  If one song has a certain set of instruments and another song lacks some of those, this may 
muddle the song outputted by the neural network.  This is why we will be focusing on analyzing multiple
songs so that we can perhaps group them by instrument.  We also thought of restricting it to older mario 
songs because these use the same 8 bits for each song.  


III. What is the plan for the next week including what each team member is planning
to work on in the next week. Describe goals and potential timelines (“ I plan to
finish understanding x to see if it can be implemented for our project by
Wednesday etc”. )

- Since we only have several weeks left to finish this project, we want to start meeting 
twice each week.  We plan on one longer meeting each Monday before class and one shorter
meeting on Friday before class.  Each team member is going to pick 2-3 songs in midi, 
convert them to CSV, and start analyzing them over the weekend.  On Monday we plan to 
meet back up and discuss what we found in terms of observations and challenges.  We are 
toying with the idea of pairing people up based on how difficult they found the data 
analysis from the weekend.  If so, the pairs would be flexible and open to change based 
on the project’s current status.  

```

### Week 1 Summary 

```markdown
Team name:
Jukebot
Team members:
Krithik Chitla, Sahith Desham, Santosh Nukala, Omkar Apte,
Rajeev Rajendran, Anand Hande
Date:
09-04-2020

Team roles for this week (write down name):
- This week all of us were brainstorming and discussing early possible directions for our project.
Omkar facilitated our meeting, Sahith and Rajeev were recording, Krithik and Santosh were the
planners, and all members were engaged and contributing.


Describe briefly what the main goal of your team is (so the peer reviewer has some
context).
- Our goal is to generate music (likely video game music) using neural networks.


I. What was done this week regarding the project: If you want to include code include
this in the Appendix. Describe what the group did (including contributions of individual
team members) with regards to the group project this week. Give enough details so I
understand what you folks have been doing over the week. Include dates of your
meeting(s) and who met on these days.

- Our full group met on Tuesday, September 1st. We had a long list of possible ideas which we
discussed in depth, and we finalized the decision to move forward with the idea of generating
music using neural nets. Other ideas included star identification, election result prediction using
polling data, medical image analysis, and using a genetic algorithm to tackle a schrodinger
equation. The prospect of generating music, likely videogame music, was ultimately the most
engaging idea to the full group. We’re planning to use MIDI files as training data and we found many possible sources,
including the following:

  1. https://www.vgmusic.com/music/console/
  2. http://www.mirsoft.info/gamemids.php
  3. https://github.com/mdeff/fma
  4. http://abc.sourceforge.net/NMD/
  5. http://www.piano-midi.de/
  6. http://musedata.stanford.edu/
  7. https://composing.ai/dataset

- We have also started to gather information on other approaches to similar problems. We found
the Google Magenta project generating Bach-like music using neural networks to be inspiring, as
well as work from OpenAI and Facebook AI Research._


II. What were obstacles faced if any in working on the project? This could be technical
(like not being able to implement or understand particular techniques) or time issues
(midterms for other courses etc).

- At this early stage we haven’t hit any major obstacles. However, when choosing an idea
initially, we were sometimes limited by our knowledge of our toolkit. As it’s so early in the term, and
we don’t have a full sense of everything we’re going to learn, we had to make some assumptions
about what plans would be both sufficiently challenging while still being feasible for our skill level.
Ultimately, we’ve chosen a topic that we feel should offer us some latitude to adapt as we learn
more techniques across the course.


III. What is the plan for the next week including what each team member is planning
to work on in the next week. Describe goals and potential timelines (“ I plan to
finish understanding x to see if it can be implemented for our project by
Wednesday etc”. )

- Prior to our next update we’re going to map out a plan for how to proceed through the term
with internal deadlines. Additionally, we’d like to wrangle a complete dataset of midi files that we
can use for training our music generation model. We’re also going to work on understanding the
implementation of the neural networks that we’ll need to use to tackle this work.
```

