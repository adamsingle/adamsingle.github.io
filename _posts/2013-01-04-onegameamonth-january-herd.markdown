---
layout: post
title: "#OneGameAMonth - January: Herd"
image: http://res.cloudinary.com/adamsingle/image/upload/v1443437892/1GAMlogo1_idv7cs.png
date: '2013-01-04 14:35:03'
tags:
- 1gam
- 1gam-2
- ai
- blog
- games
- unity3d
---


Let├óÔé¼Ôäós talk about my first game for the Game A Month challenge.

I├óÔé¼Ôäóm keeping this one RELATIVELY simple. The game idea itself is simple, and may even be boring to play. But the purpose of this game is not so much to make a fun game, as to test some processes. I have been doing a lot of reading lately, some on game design, some on AI, some on lean startup principles for entrepreneurs, and I want to try them out on a small game.

Lean startup prinicples tells me to develop an MVP or minimal viable product. That is, create the product, or in this case game, at it├óÔé¼Ôäós minimal playable state. But that means completely playable. There should be a menu, the game, end condition, end menu or replayablility functions. You should be able to give it to your customer and have them interact with it and give feedback. The idea is to do this as fast as humanly possible to test your hypothesis. You must get it in the hands of the people you are making it for.

I├óÔé¼Ôäóve also been reading some game design articles, almost all of which come from the amazing Dan Cook. If you├óÔé¼Ôäóre interested in game design, or indeed design in general, and haven├óÔé¼Ôäót read his blog, you should do so now. Stop reading mine, read his, mine will wait. One of the articles that caught my eye in particular, is the one on design logs. Design logs are a flexible, iterative alternative to the traditional Game Design Doccument (GDD). I love the idea, and intend to give it a go for this game and see how well it works in practice, or how well my lazy butt managers to use it as intended.

This game should be done relatively quickly, which will leave me with time to figure out how to publish it to all the places that One Game A Month has encouraged me to join. Namely IndieDB, Newgrounds and Kongregate. All of these require different set ups, and Kongregate at least has it├óÔé¼Ôäós own API that allows you to tie your game to Kongregates leaderboard system, amongst other things. So I definitely would like to use that.

Finally, the AI prinicples I├óÔé¼Ôäóve been reading won├óÔé¼Ôäót really be applied here, this is my chance to aim at producing a certain type of AI that I have started implementing before, but never finished because I had no real goal for it. This game will be using an AI technique commonly refered to as flocking. Flocking is a type of ├óÔé¼╦£steering behaviour├óÔé¼Ôäó and is a subset of the ├óÔé¼╦£emergent behaviour├óÔé¼Ôäó field of AI, a field I am particularly interested in.

Emergent behaviour has a few definitions depending on who you ask, but my personal definition is ├óÔé¼┼ôa system of simple rules that, when combined, produce unintentional results├óÔé¼┬Ø.

I use the word ├óÔé¼┼ôunintentional├óÔé¼┬Ø to refer to results, actions or behaviours that weren├óÔé¼Ôäót specifically programmed. The high level result should be intentional, but the specific details of how it is achieved come about ├óÔé¼┼ônaturally├óÔé¼┬Ø. Luckily, flocking is one of the simpler examples to help explain this.

So flocking is a steering behaviour that combines 3 ├óÔé¼┼ôrules├óÔé¼┬Ø to create movement for a group of agents (agent being an entity that is a part of an AI system) that is somewhat similar to the way a flock of birds move. By playing with the values of these rules the actual flocking behaviour can be tweaked however. So where a flock of birds move smoothly in a group, rarely changing their ├óÔé¼┼ôpostion├óÔé¼┬Ø within it, you could use flocking to create swarming (like a swarm of rats for instance) where the agents scramble over the top of each other, just by modifying the value of the rules (not changing the rules themselves).

Lets break that down a little further. The three rules used are refered to as Cohesion, Separation and Alignment. Each of these, on their own, are quite simple.

Cohesion is the act of staying close to other agents. This is done by averaging all their positions and moving towards that position.

Separation is the act of pushing away from agents that are too close. This is done based on a maximum distance or range from the agent. The closer another agent is, the stronger the ├óÔé¼┼ôseparation├óÔé¼┬Ø or ├óÔé¼┼ôrepulsion├óÔé¼┬Ø. This separation vector is calculated for each agent within the max range and summed together to get a resulting separation vector.

Align tries to match the orientation to a target, or on this case, a number of targets. This behaviour produces no velocity vector, just a rotation. Again this uses a maximum distance so the agent is only effected by other agents within a certain range.

So, when you combine all three of these you get a direction to face and move towards based on all other agents that are close. Since all of the are also using these same behaviours you end up with a group of agents that appear to be moving together like a flock of birds. Check out this simple demo. In this example the agents are also using a ├óÔé¼┼ôseek├óÔé¼┬Ø behaviour and moving towards the red dot. Without that they would move around randomly as each is pushed around by all the others. This one hasn├óÔé¼Ôäót been particularly smoothed out or tweaked in any way, so it├óÔé¼Ôäós quite jittery.

So that├óÔé¼Ôäós emergent behaviour, and specifically the behaviour I├óÔé¼Ôäóll be implementing in my first game. Where it get├óÔé¼Ôäós interesting is when you add a predator. By having an agent that all the flocking agents avoid, and making this avoidance behaviour ├óÔé¼┼ôstronger├óÔé¼┬Ø than the flocking one, you get some very interesting looking results. Take a look at this real life video of a flock of starlings avoiding a hawk.

And that├óÔé¼Ôäós the basis of my first game for 1GAM. You control a sheep dog attempting to herd sheep into a pen. I have some concerns over how well the ├óÔé¼┼ôflock├óÔé¼┬Ø will move in a way that suggests sheep, and I may change it to a school of fish or flock of birds. The reason I chose the sheep is the simple to understand end condition of getting them all in the pen. At this stage it doesn├óÔé¼Ôäót really matter. I├óÔé¼Ôäóll get a working prototype happening as soon as possible with primitives and skin it from there.

The first step, however, is the set up and documentation. This time I├óÔé¼Ôäóm doing everything right from the start. So I├óÔé¼Ôäóve set up accounts on IndieDB, Newgrounds and Kongregate. I├óÔé¼Ôäóve installed Git and set up an account on Github (all these games will be completely open, so feel free to jump over to my github account and take a look at the code if you├óÔé¼Ôäóre interested. There isn├óÔé¼Ôäót anything there right now, but there might be by the time you read this). I├óÔé¼Ôäóm installing github for mac right now and about to set up an empty unity project to get that ball rolling. After that I get started on my design log for the game. I have most of it in my head, but like I said, I want everything done right for these games, get this part of the process sorted and a part of my pipeline.

Stay tuned for a progress report.

Chat later,

Adsy


