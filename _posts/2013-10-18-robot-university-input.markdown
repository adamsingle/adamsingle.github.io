---
layout: post
title: Robot University - Input
date: '2013-10-18 11:00:58'
tags:
- blog
- robot-university
---


Robot University Update 4 ├óÔé¼ÔÇ£ Input

Good morning, day, evening or night friends. My last two posts have been discussing the rather unique issues for setting up a camera in Unity for a project spanning an entire wall of the Cube. The last post stated that the camera was solved. This isn├óÔé¼Ôäót quite the case. There is still another issue to solve, but let├óÔé¼Ôäós take a break from cameras. This week we got input working, so we├óÔé¼Ôäóll talk about that.

The touch panels at The Cube use the TUIO protocol. This is ├óÔé¼┼ôan open framework that defines a common protocol and API for tangible touch surfaces.├óÔé¼┬Ø ([http://www.tuio.org/](http://www.tuio.org/)). What this means for us is that Unity├óÔé¼Ôäós Input API isn├óÔé¼Ôäót going to help us here, it doesn├óÔé¼Ôäót, by default, receive TUIO touches.

If you are ever doing a project in Unity that uses TUIO, simply go here ([https://code.google.com/p/unity3d-tuio/](https://code.google.com/p/unity3d-tuio/)) it├óÔé¼Ôäós brilliant and super easy. Import the package, drag the TuioInput prefab into the scene and you├óÔé¼Ôäóre set up already basically. The only thing to double check is the TuioConfiguration.cs file. This is an entire class holding a public int (don├óÔé¼Ôäót ask me why). This is the port that the touch panel is broadcasting on. TUIO uses a pretty simple method of sending touch information across the network. So make sure you├óÔé¼Ôäóre listening on the correct port, it defaults to 3333 which seems to be a standard of sorts. That├óÔé¼Ôäós it, you are now receiving touch data.

The next part is the true beauty of this package. To get that touch data you just have to slightly modify your code. Instead of something like

if(Input.touchCount > 0)

{

}

you use

if(TuioInput.touchCount > 0)

{  
 }

That├óÔé¼Ôäós it! They use unity├óÔé¼Ôäós Touch struct internally, so TuioInput.touches contains UnityEngine.Touch objects. Which means any code you├óÔé¼Ôäóve written to handle touch input, or any 3<sup>rd</sup> party plugins you have that use touch, simply need to be given TuioInput├óÔé¼Ôäós touches instead of Input├óÔé¼Ôäós and everything will magically work for you. This is what I did in 2D Toolkit. I quickly jumped into tk2DUIManager and modified the CheckInputs() and CheckMultiTouchInputs() functions to include another condition for handling TuioInput.touches. The actual loop was identical code to what was already there, just copy, paste and replace any Input with TuioInput. Viola, I have 2D Toolkit UI recognising the touch panel touches.

Of course, this is the Cube. So nothing is quite that simple. Since each node (computer) controls or managers two touch screens (treated as two monitors would normally be treated by any computer) I had a slight issue. The touch panels couldn├óÔé¼Ôäót broadcast on their little local network with the same port number as each other. So they were actually set up on ports 3334 and 3335. My first thought was given that the tk2DUIManager is a singleton, I should be able to simple have two TuioInput prefabs in the scene, modify them to get their port numbers from somewhere else so that I could manually set them, and let them simply add their touches to the input manager. I├óÔé¼Ôäód then need to factor in the right panel being offset, since I needed to treat the two screens as a single space, so the touch data from the right panel would be 1080 short on the x and 1920 short on the y. But that didn├óÔé¼Ôäót seem like a big issue. Then there is the small issue of these touch panels sitting in portrait mode, which means their coordinate system is rotated 90 degrees├óÔé¼┬ª ok, that├óÔé¼Ôäós ok, slightly complicates the issue, but it├óÔé¼Ôäós not too hard to rotate an x,y position (and offsetting it in the case of the right screen) before storing it. It was at this point that I was introduced to Tim Gurnett, a programmer at the Cube that is no longer stationed up in the section I sit to work, so we hadn├óÔé¼Ôäót crossed paths before. Turns out Tim had already written a multiplexer for the touch panels that listened on both ports and mapped the two screens coords to one screen space for you, then creates new TUIO messages and sends them off on port 3333. Not just that, but it could handle the rotation for you too. Great, that saved me some time.

So on Tuesday the 15<sup>th</sup> of October we had another test on the downstairs wall, the actual full sized wall that this project will run on when it goes live. We do these every two weeks. In it we had some basic placeholder UI. Using the EventManager that I wrote to handle sending events across the network, this UI was recognising touch input and firing off animations on the Destructo bot. For a video of it in action take a look at the [official blog for Robot University.](http://www.robotuniproject.com/test-3-on-the-screens/ "Robot University")

Here are some pics I took myself though.

[![IMG_3890](http://res.cloudinary.com/adamsingle/image/upload/v1443437682/IMG_3890_mubbkn.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437682/IMG_3890_mubbkn.jpg) [![IMG_3891](http://res.cloudinary.com/adamsingle/image/upload/v1443437682/IMG_3891_psfr7w.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437682/IMG_3891_psfr7w.jpg) [![IMG_3893](http://res.cloudinary.com/adamsingle/image/upload/v1443437643/IMG_3893_y38egb.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437643/IMG_3893_y38egb.jpg) [![IMG_3894](http://res.cloudinary.com/adamsingle/image/upload/v1443437642/IMG_3894_wtaize.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437642/IMG_3894_wtaize.jpg) [![IMG_3895](http://res.cloudinary.com/adamsingle/image/upload/v1443437641/IMG_3895_urodzp.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437641/IMG_3895_urodzp.jpg)

All in all, I spent a decent amount of time trying to build this myself, and use some other code that had come my way, but the overall lesson is, if you want TUIO input in your game/project, use the cutdown TUIO framework from Mindstorm. It├óÔé¼Ôäós magic. You can also test TUIO at home using a smart phone. There are free apps for both android and iOS, android is called TUIODroid I believe, not sure what iOS is off the top of my head. Make sure your phone and computer are on the same wifi network, set up the app with your computers IP address and make sure the port is set in it├óÔé¼Ôäós settings to the same you have in code (default 3333 remember, I think the app defaults to that too), then that├óÔé¼Ôäós it. Run your project in editor and use the phone as a touch interface. If you drop in the TouchDisplay and TouchConfig prefabs as well you├óÔé¼Ôäóll get some nice little circles in the editor game window showing you your touches.

Thanks for reading,

Chat later,

Adsy


