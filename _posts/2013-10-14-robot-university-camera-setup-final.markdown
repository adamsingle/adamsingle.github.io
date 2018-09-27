---
layout: post
title: Robot University - Camera set up [Final]
date: '2013-10-14 12:00:25'
tags:
- blog
- robot-university
---


[info][Back to Robot University Main Page](http://adamsingle.com/robot-university/ "Robot University")[/info]

Good morning, day, evening or night. This is quite overdue, but as with any indie dev, I├óÔé¼Ôäóve been pretty busy. More than a few projects on the table at the moment, and Robot University has been throwing some challenges at us. I know, we knew that when we started. But it├óÔé¼Ôäós one of those projects where you can never identify them before they smack you in the face.

The plan was to get the camera system sorted out in the first week or two, then nail down touch input, tie it into 2D Toolkit UI and I can sit back and wait for content to come my way. Sure, there├óÔé¼Ôäós more to it than that. I have a dialog tree to implement. And I haven├óÔé¼Ôäót used 2D Toolkit before, so all the UI, and it├óÔé¼Ôäós various transitions and functions, will be new to me and take time to figure out and bug test, but essentially the core should be a few weeks work. Well, we├óÔé¼Ôäóre nearly half way, and I think I just got the camera system sorted out. Touch input is still toying with me. So I├óÔé¼Ôäóm going to have plenty of late nights in the months to come.

Let├óÔé¼Ôäós talk cameras. ├é┬áLast update I said I had it in the bag. I didn├óÔé¼Ôäót quite. There was one thing I had failed to take into account, and I can thank my dear friend Hans Van Vliet for helping me find it. Hopefully everyone remembers the setup for the Cube walls. If not, duck back to the last post for a recap. The camera solution involved using one camera looking at everything, scissor off the part you wanted to show depending on which node in the wall you were, then fill that to screen. The problem was the projector space on top had a different resolution, or more importantly, aspect ratio, than the panels down below. So putting either into the others aspect wasn├óÔé¼Ôäót going to solve anything. What we had last time was the projector space being squashed down to fit. Everything lined up horizontally with the touch panels, but was all out vertically in the projector space. The solution is actually rather simple and obvious, in hindsight. Some of you reading may already be scoffing at my missing it. If we are looking at this scene in Unity using one camera, we need to look at this wall, these collective screens, as one ├óÔé¼┼ômonitor├óÔé¼┬Ø. And a monitor only has one aspect ratio. You can├óÔé¼Ôäót have a different one across the top half. So I needed to calculate the aspect ratio and resolution of the ├óÔé¼┼ôwall screen├óÔé¼┬Ø. This basically means converting one set of pixel space into the other.

Here goes:

The projector space is 5470 x 1170. Each touch panel is 1080 x 1920. There are 12 panels side by side. So the ├óÔé¼┼ôtouch panel space├óÔé¼┬Ø is (1080 * 12) x 1920 or 12960 x 1920. Try and picture this using this image (again):

├é┬á

<div class="wp-caption aligncenter" id="attachment_2001" style="width: 891px">[![The Cube - Zone 3. Physics Playground.](http://res.cloudinary.com/adamsingle/image/upload/v1443437684/Zone3_shserx.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437684/Zone3_shserx.jpg)The Cube ├óÔé¼ÔÇ£ Zone 3. Physics Playground.

</div>├é┬á

The top ├óÔé¼┼ôhalf├óÔé¼┬Ø (it├óÔé¼Ôäós clearly not half) is the projector. That├óÔé¼Ôäós 5470 pixels wide. The panels are basically the same physical width, but have almost 13000 pixels in that space.

So, we want to know how many ├óÔé¼┼ôprojector space pixels├óÔé¼┬Ø fit into a ├óÔé¼┼ôtouch panel pixel├óÔé¼┬Ø.

Or

5470 / 12960 = 0.4221.

So we can now convert the pixel height of the touch panels to the same ├óÔé¼┼ôpixel space├óÔé¼┬Ø by multiplying 1920 by 0.4221. Which is 810.37. Obviously we need to drop the .37, can├óÔé¼Ôäót have a fraction of a pixel, but close enough is good enough.

So what we├óÔé¼Ôäóve done here is say, let├óÔé¼Ôäós pretend the touch panels have the same pixels across as the projector space, like a normal monitor would. If that were the case, the number of these sized pixels in the vertical height of the panels would be 810. Add that to the vertical height of the projector space and we get the height of our ├óÔé¼┼ôwall screen├óÔé¼┬Ø, which is 1980.

So the wall, as a single monitor, has the pixel dimensions 5470 x 1980. We can now create that custom resolution in Unity and work within it. I can now set the cameras aspect ratio in script to be 5470 / 1980 and I know that what the artists see in editor is what will be on the wall.

So that├óÔé¼Ôäós enough for this week probably. I├óÔé¼Ôäóll briefly mention that the solution for filling the two panels that I mentioned last week also didn├óÔé¼Ôäót work. Well, it did, but setting all the touch panels to a custom resolution that none of the other projects on the wall used had what should have been obvious repercussions. The solution actually ended up being to set, in editor, the application to be a sizable window and to use a script to launch the the exe and set the window size to be the width and height we needed. For those who can├óÔé¼Ôäót remember, each node controls two touch panels, so their ├óÔé¼┼ôscreen├óÔé¼┬Ø is actually (1080 * 2) x 1920 or 2160 wide. But the set up in windows is two screens with a stretched desktop. As most of you probably know, maximising a window on one of two monitors will not make it fill both monitors. Hence our solution to set up a custom resolution of 2160 x 1920. The sizable window solution worked well.

Next post I├óÔé¼Ôäóll talk about touch input (hopefully working fully by then) and after that I├óÔé¼Ôäóll talk about the networking, which worked almost without a hitch. Much to my surprise as I├óÔé¼Ôäód estimated it to be the most likely to take ages to figure out and get working. Typical. Don├óÔé¼Ôäót know why us programmers bother even trying to guess what├óÔé¼Ôäóll happen during a project.

Thanks for reading,

Chat later,

Adsy.


