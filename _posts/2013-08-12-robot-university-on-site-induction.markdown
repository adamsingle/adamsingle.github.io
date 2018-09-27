---
layout: post
title: Robot University - On Site Induction
image: http://res.cloudinary.com/adamsingle/image/upload/v1443437683/InductionBanner_almezr.jpg
date: '2013-08-12 15:00:57'
tags:
- blog
- robot-university
---


[info][Back to Robot University Main Page](http://adamsingle.com/robot-university/ "Robot University")[/info]

This week the creative seas of our little collective met and lashed the mighty coastline that is The Cube. Probing for weaknesses, seeking pathways to new ideas.

[![IMG_3390](http://res.cloudinary.com/adamsingle/image/upload/h_334,w_640/v1443437687/IMG_3390_ddt3k4.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437687/IMG_3390_ddt3k4.jpg)

This was the first time we had all been together in one place, and for many of us, the first time we had met each other in person. This in itself is a wonderful step forward. Being able to hold an impromptu discussion with someone, on anything at all, is incredibly important when it comes to building your impression, trust and confidence in a person that you are going to be working with. I├óÔé¼Ôäóm delighted at the professionalism, creativity and motivation that Christy, Jacek, Simon and Paul showed. I am very much looking forward to this project.

<div class="wp-caption aligncenter" id="attachment_1990" style="width: 1034px">[![TeamatCube](http://res.cloudinary.com/adamsingle/image/upload/h_480,w_640/v1443437685/TeamatCube_x0bklg.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437685/TeamatCube_x0bklg.jpg)Left to Right: Paul Stapelberg, Jacek Tuschewski, Christy Dena, Simon Boxer and me

</div>That was the first exciting part. The second was the space we├óÔé¼Ôäóll be working in. If you live in Brisbane, come here often, or even infrequently, in fact, if you ever visit in the future, if only for a day, you must see [The Cube](http://www.thecube.qut.edu.au/ "The Cube"). There are photos on the site, and I├óÔé¼Ôäóll be adding many more in this post, but until you stand in front of it, it doesn├óÔé¼Ôäót quite sink in. And when you are looking at it with the eyes of a developer, it gets your heart rate right up. So much potential.

We spent the two days of our induction meeting the amazing staff at The Cube and touring the space we├óÔé¼Ôäóll be developing in and for. Everyone was incredibly helpful and friendly, and clearly have a passion for seeing awesome content on their amazing facility. I was blown away with the whole campus. It├óÔé¼Ôäós been a while since I was on one, and I was struck with an overwhelming desire to return to studies. Especially during our tour of the robotics labs (check out the gallery just below for a taste of that). Although speaking to the guys there I realised my passion isn├óÔé¼Ôäót so much with robotics as the AI behind them. That├óÔé¼Ôäós still what I really want to do one day. If I ever find time I├óÔé¼Ôäóll actually get stuck into the many free AI university level courses available online.

[nggallery id=9]

<iframe allowfullscreen="" frameborder="0" height="480" src="https://www.youtube.com/embed/eH2f9HGARKY?feature=oembed" width="640"></iframe>

I spent a lot of my time before the schedule kicked in to observe people using the installations that are currently up on the walls. There is a huge interactive reef on Zone 1 and 2. Zone 1 and 2 is the big one. It├óÔé¼Ôäós combined into one zone really, and has a different set up to zones 3 and 4 (which are the zones we are building for. See the specs further down). This drew some attention, predominantly for it├óÔé¼Ôäós sheer scale in my opinion.

[nggallery id=19]

├é┬á

The one that really caught peoples attention was the physics playground. This is a great interactive installation that allows you to change the gravity to match the different planets in our solar system and play with a mass of wooden blocks. There are also plenty of other interactive elements, but it├óÔé¼Ôäós the blocks that people are really drawn to. This was also set up on zone 3, which is the one we are most likely deploying on.

<div class="wp-caption aligncenter" id="attachment_2001" style="width: 891px">[![The Cube - Zone 3. Physics Playground.](http://res.cloudinary.com/adamsingle/image/upload/v1443437684/Zone3_shserx.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437684/Zone3_shserx.jpg)The Cube ├óÔé¼ÔÇ£ Zone 3. Physics Playground.

</div>Check out the video of a group of Japanese school kids. They spent ages here.

<iframe allowfullscreen="" frameborder="0" height="360" src="https://www.youtube.com/embed/X5antXrqh-I?feature=oembed" width="640"></iframe>

├é┬á

<iframe allowfullscreen="" frameborder="0" height="360" src="https://www.youtube.com/embed/VGlxgKjg1ho?feature=oembed" width="640"></iframe>

├é┬á

For those technically interested, here is a breakdown of the space.

The wall consists of 12 55-inch Multitaction MT553s. They are an optical (infra-red) based unit, able to accept an unlimited number of touch events and will be using the TUIO touch protocol. Each panel is installed in the portrait orientation and has a resolution of 1080 x 1920. Panels are paired, each pair being controlled by a single ├óÔé¼┼ônode├óÔé¼┬Ø computer running 2 Intel Xeon E5-2643 3.33Ghz CPUs, 32GB quad-channel, 1600Mhz of RAM, Creative X-FI Extreme PCIe sound adapter and an NVIDIA GTX690 graphics card.

Above the Touch Panels is a large projection zone. This uses 3 Panasonic DZ6700 projectors connected to another ├óÔé¼┼ônode├óÔé¼┬Ø PC which uses edge blending to composite the output to a single image with a resolution of 5360 x 1114.

As a predominantly mobile developer, you can perhaps imagine how my head spins a little at the thought. When I first started out I was trying to fit a game into 30MB to run on a 320├âÔÇö480 screen with 256MB RAM. In some ways, this removes a lot of the challenges I usually deal with. Optimisation, although an integral part of game development no matter what (and completely drummed into me from mobile dev), is somewhat moot on this set up. Theoretically there isn├óÔé¼Ôäót much this thing won├óÔé¼Ôäót be able to handle. And considering our scope restrictions, we shouldn├óÔé¼Ôäót come close to maxing it out. But, having said that, there is a new challenge. Although the wall appears (or in our case, will appear) to be one large interactive scene, it will be running simultaneously on 7 different machines (12 touch screens gives 6 machines controlling 2 each, and one more to control the three projectors overhead). This is certainly something I├óÔé¼Ôäóve never come across before.

So, my first task is clear. I need to get a test rig happening up there. We have a test environment available to us that uses 4 touch screens (set up the same way as the walls) and a projected (though smaller) panel above. So this is a 3 system set up, but once I have it working on that, extending to the 7 system set up won├óÔé¼Ôäót be any more difficult. I plan to treat this like any multiplayer game and use Unity├óÔé¼Ôäós built in networking to set up an environment where each player├óÔé¼Ôäós camera is fixed in the world, defining their view. When assembled in the correct configuration you get a seamless view of the world as if through one giant camera. I actually find this an interesting starting point for an actual multiplayer game, I may toy with the idea a little more after I this project, but imagine playing a game where you can├óÔé¼Ôäót move your camera, and neither can anyone else, but everyone is looking at a different part of the world. Or even a board game that requires the players to put their iPads together. Or maybe they only do that at certain times to solve things together. Could be some interesting ideas in there.

When I get the rig working in Unity I├óÔé¼Ôäóll do up a video showing how I├óÔé¼Ôäóve set it up, and then another once I have it up on the test set up, so keep an eye out for that (there may even be some Kinect detection happening in that one).

As you can tell, very excited about this project. I hope to keep you all up to date as I go along. I├óÔé¼Ôäóll either link to the blog posts of other team members from the main [Robot University page](http://adamsingle.com/robot-university/ "Robot University")├é┬áor write updates that include some of the work they are doing. And be sure to check out The Cube if you get the opportunity. Get a feel for what├óÔé¼Ôäós there now, because we plan on outdoing them ├»┬┐┬¢├»┬┐┬¢

Chat later,

Adsy

├é┬á

├é┬á

├é┬á

├é┬á


