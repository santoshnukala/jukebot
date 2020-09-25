---
layout: default
title: "Weekly Logs"
permalink: /weeklylogs/
---
[Back to Homepage](https://santoshnukala.github.io/jukebot/)
## Weekly Logs

Here is where you can find our weekly logs

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

