---
layout: post
title: Robot University - The Kinects
date: '2014-05-28 10:00:44'
tags:
- blog
- robot-university
---


My humble apologies for ignoring this site and blog for so long. Much has happened since my last post, so I have some catching up to do. Expect a blog every week for a little while while I do just that. Today is a post I wrote many months ago and never got around to uploading it. It├óÔé¼Ôäós my final update on Robot University, which is well and truly finished. Next week I might put together a simple Post Mortem of the project, from my point of view, and share some of the media that resulted from it. After that it├óÔé¼Ôäós all about the future. On to the kinects:

Robot University Update ├óÔé¼ÔÇ£ Kinect Integration.

I left this one off till quite late in development, because I wanted to be sure that I had it working as expected. Now that I have, I├óÔé¼Ôäód like to walk you through how we integrated 6 kinects into the Robot University installation.

Now we aren├óÔé¼Ôäót doing anything super amazing with them, just a simple little ├óÔé¼┼ôcall to action├óÔé¼┬Ø. For those wondering what that is, a call to action is something that grabs a ├óÔé¼╦£potential├óÔé¼Ôäó user├óÔé¼Ôäós attention, and turns them into an ├óÔé¼╦£actual├óÔé¼Ôäó user. In our case, our potential users were people walking past the installation. Now audio is a big call to action, and we have that, but there will be times when the audio is turned down, most notably exam time for QUT. So we wanted to use the Kinects to add another call. We decided on a swarm of little bots that could follow the user as they walked past the wall. Hopefully the movement would be something that they caught out of the corner of their eye, and draw them to the wall. Once the user gets too close, the swarm scatters off screen.

To add a little bit of fun to this, we decided the swarm would track the closest person to the wall. So if you were walking past, they would begin to follow you, initially converging on your position from multiple offscreen positions. But if a second person suddenly started walking past the wall, closer to it than yourself, the swarm would abandon you and follow them.

The Cube had always had Kinect installation on it├óÔé¼Ôäós ├óÔé¼┼ôtodo├óÔé¼┬Ø list, but it hadn├óÔé¼Ôäót been a high priority before now, so we had to wait to get them installed before we could really muck around with it, and I won├óÔé¼Ôäót lie, it took me a while to wrap my head around how to do what I wanted to do.

Let├óÔé¼Ôäós break it down. There are 6 nodes along the bottom of the wall, each controlling two touch panels. My initial discussions with the staff resulted in us all thinking we would only need 3 kinects to cover the distance along the wall with minimal overlap, though this would leave gaps in close. My main problem was I had no idea what sort of data I would be getting back from the Kinects, or how to handle someone being seen by more than one at a time.

So I needed to get one hooked up and debugging out data. I have a Kinect at home that came with my Xbox, so that was my starting point. Since I├óÔé¼Ôäóm using a Mac, I had looked at the open source Kinect SDK, OpenNI. There is a Unity plugin for this already. However, it occurred to me that since the systems at The Cube were PCs, it was likely that the staff would want to use the official Microsoft SDK and drivers. So I set about setting up Windows on my little Macbook Air. This was pure hell. It was easy enough to set up with Bootcamp, but there just isn├óÔé¼Ôäót enough internal memory to handle it. I have since bought Parallels, and am still having similar issues, but I finally realised I can have Windows installed on an external HDD (but not Parallels itself) so I├óÔé¼Ôäóm in the process now of getting back half my internal hard drive space. The point is, I eventually got a Windows environment set up with Unity installed. I actually really love Parallels and being able to switch between the two OSs at will is awesome.

For some reason, however, I was getting an error in Unity saying I had no Kinect plugged in. Getting the Kinect drivers and SDK integrated into the actual project was as simple as importing the plugin on the Asset Store, very easy. But this issue was a little tricker to solve. I eventually discovered, hidden somewhere in the back of MSDN documentation, that the Xbox Kinects cannot be recognised by the Microsoft Drivers when running in a Virtual Machine. Sigh. So I had to get my hands on a Windows Kinect. Luckily these were what The Cube was using, and I was able to borrow one.

So with a rig finally set up at home I started looking into the data it was giving back. I made the mistake of not looking up what the Kinect returns, and making an assumption. I assumed, without really thinking about it, that the Kinect would return a normalised value within it├óÔé¼Ôäós view, much like the Viewport value of a Unity Camera. Since I was testing this in a fairly small room, the numbers I were getting looked to support this to some extent (but with 0 being the middle instead of the left, so values from -0.5 to 0.5. Not exactly normalised, and this should have been my first warning). Unfortunately, once I got this same set up happening on the wall, I was getting values outside of these numbers. Which was throwing me. So after finally googling some docs, I discovered these values are actually meters from the middle of the sensor. To be fair, that does seem pretty strange. I never suspected I├óÔé¼Ôäód be getting physical distances. Guess I├óÔé¼Ôäóm too used to working in virtual environments.

OK, so I have another piece of the puzzle. I started to sketch up the problem. 6 kinects across a space. Each one giving data, in meters, of the closest person it could see. I could send this data to the server, but I had to figure out what to do with it. The first obvious problem is that someone standing in the middle of the sensor on node 5 would produce the same values (or close enough) as someone standing in the middle of node 1. So I needed a way to offset these values in world space so that the swarm new where the person was standing. This took me a while, and involved some doodling and free form note taking.

Eventually I figured it out. Since the data was in meters, I had to work in meters too. I knew the physical dimensions of the panels, and they are almost flush against each other, so a few mm wasn├óÔé¼Ôäót going to be worth worrying about. Each panel is 692mm. This means the first Kinect is 692mm from the left edge of the project space. The 2nd Kinect was 1384mm from there, the 3rd another 1384 etc etc. This brought the total distance across the wall to about 8.3m. From this I could create a little equation to calculate the distance from the left edge that a person is standing using the position the kinect gives, and the node that kinect is attached to:

Distance to first kinect in meters + ((node ├óÔé¼ÔÇ£ 1) * distance between kinects in meters) + kinect position.x

= 0.692 + ((node ├óÔé¼ÔÇ£ 1) * 1.384) + kinect position.x

Since I also knew the physical width of the whole space, I could convert this to a fraction of the width of the ├óÔé¼┼ôcollective screen├óÔé¼┬Ø that was the whole environment by dividing it by 8.3. Now I basically had a Viewport position of the person. I could use the camera viewportToWorldPoint function to convert that into world space and use that as the position the swarm needs to track.

The only tricky part left was handling the logic for which kinect to track. After some teething issues I got that working too. Essentially the server, which controls the swarm, holds a last known position for each node (setting any nodes last known position to a huge number when it receives data indicating that sensor doesn├óÔé¼Ôäót see anyone). It cycles through each node to check if the new position just received is closer than any last known positions. If it is, it├óÔé¼Ôäós the new position to track, otherwise it├óÔé¼Ôäós ignored. There├óÔé¼Ôäós a little more logic for handling when to call the swarm in or send it running, but basically that├óÔé¼Ôäós the functionality we were after working. You├óÔé¼Ôäóll be able to come and see it for yourself in a couple of months if you happen to be in Brisbane.

I hope you found that informative, or enjoyable. Both would be marvellous ├»┬┐┬¢├»┬┐┬¢

Thanks for reading,

Adsy.

Final Note:  
 Obviously, as I said, this was written some time ago, so you can see the installation right now down at the Cube. It├óÔé¼Ôäós been up for a few months now, so I├óÔé¼Ôäóm not sure how much longer it├óÔé¼Ôäóll be there. If you do get down there to see it, let me know what you thought of it.


