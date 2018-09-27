---
layout: post
title: Robot University - Camera Set Up
date: '2013-09-09 16:00:31'
tags:
- blog
- robot-university
---


[info][Back to Robot University Main Page](http://adamsingle.com/robot-university/ "Robot University")[/info]

So last update on Robot University I promised a post describing the camera rig set up once I had it working. That was a few weeks ago. I only just got it working. Almost. It has turned out to be a greater challenge than I anticipated.

Here├óÔé¼Ôäós a recap of the situation. The wall at The Cube that will be displaying Robot University is made up of 12 touch screen panels, set up as linked pairs controlled by a single PC called a node. The panels are in portrait orientation, side by side along the bottom of the wall. The top of the wall is three projectors being handled by another node. This node handles the image compositing, so thankfully I don├óÔé¼Ôäót need to worry about combining and overlapping three projectors, I can simply treat this is a good old fashioned single machine with some sort of crazy monitor running a 5360 x 1114 resolution.

<div class="wp-caption aligncenter" id="attachment_2001" style="width: 574px">[![The Cube - Zone 3. Physics Playground.](http://res.cloudinary.com/adamsingle/image/upload/v1443437684/Zone3_shserx.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437684/Zone3_shserx.jpg)The Cube ├óÔé¼ÔÇ£ Zone 3. Physics Playground.

</div>Now my initial thought was that each node would have a camera in the scene that it would set up based on it├óÔé¼Ôäós node number (or I build all the cameras in the scene and each node deletes the ones it doesn├óÔé¼Ôäót need based on it├óÔé¼Ôäós node number) so I immediately started experimenting with that. Since this is a single scene that basically needs to look like you├óÔé¼Ôäóre looking straight into it no matter where on the wall you are standing, my first thought was cameras side by side and just figure out how to stitch them together. But that doesn├óÔé¼Ôäót work because of the shape of a perspective frustum and issues with camera angle. Let me show you.

[![BadStereo](http://res.cloudinary.com/adamsingle/image/upload/v1443437683/BadStereo_plp3uh.png)](http://res.cloudinary.com/adamsingle/image/upload/v1443437683/BadStereo_plp3uh.png)

Now as you can see, this image is actually referring to how our eyes process an image, but it effectively displays how two perspective cameras side by side aren├óÔé¼Ôäót going to see an object form the same angle. In the case of our eyes, the brain compensates, but programatically these obviously aren├óÔé¼Ôäót going to stitch together. Especially not when there are 6 of them in parallel.

So my next thought was a fan of cameras. This would line up edges of the frustums so there are no gaps between them, essentially creating one really wide frustum. This works, as you can see in the image, but you├óÔé¼Ôäóll notice it creates a view just like a panoramic photo on your phone (since it├óÔé¼Ôäós done in exactly the same way). This is because you are looking at the scene from a singe point of view, so as you point the camera to the far left and right you are much further from it then you are form the middle, so you get a bowed look to it. You could compensate for this physically and wrap the scene in a crescent shape around the camera position. But then the whole team has to keep factoring that in and thinking in that space. It would be better to find a solution that allowed us to build the scene as you would expect it to exist, in a straight line.

So it took me a lot of playing around, but eventually I managed to sit down and put the problem into words. What, exactly, was I trying to achieve here? I want to look at this scene through a single camera, but only display parts of that view to each node. So instead of multiple cameras being stitched together to show the scene, one camera being cut up into the node spaces. I├óÔé¼Ôäóm sure such a thing would be possible, but I├óÔé¼Ôäód never heard of it being done, nor could I think of a game play reason to do it, so I wasn├óÔé¼Ôäót expecting to find a solution out there for me, I was expecting to have to spend a lot of time down the matrix math rabbit hole. I was wrong. It├óÔé¼Ôäós apparently a common technique (though I still can├óÔé¼Ôäót think of a use for it outside of this rather unique project). It├óÔé¼Ôäós called a Scissor. Unity already has built in functionality for controlling the View rect. That is you can easily render the cameras entire frustum into just a sub section of the screen space. This I can think of a use for. Mini maps are the first that come to mind, rear view mirrors are another I thought of. I understand they are normally done with shaders, but you could potentially treat it like a reverse camera in a car and render it onto just a section of the screen space. This wouldn├óÔé¼Ôäót work if you wanted it to look pretty and have it on the surface of a model of the mirror in the car. But in the early racing games that had this feature it was just a little rectangle at the top of screen. Anyway, this is easy to do. But I didn├óÔé¼Ôäót want that. I needed to only render a subsection of the cameras view to all of it├óÔé¼Ôäós screen space. Essentially the reverse. Traditionally a Scissor cut on the camera is used to only render that subsection of the view into that subsection of the screen. So it was similar to the View rect in unity, except it didn├óÔé¼Ôäót render the whole camera view, only what fits into the new view rect. I was lucky enough to find someone sharing a little script that did the scissor for me. I just had to modify it slightly so that it would render the cut to the whole screen. A single line of code. I have├óÔé¼Ôäót had time to completely deconstruct this code and figure out how it├óÔé¼Ôäós doing it, but there is a warning that one of the values isn├óÔé¼Ôäót being used. So I will eventually take a look at that. But it works, so I├óÔé¼Ôäóll share it here.

*using UnityEngine;*

***using System.Collections;*

*├é┬á*

***public class Scissor : MonoBehaviour*

*{*

*public Rect scissorRect = new Rect (0,0,1,1);*

*public Rect viewRect = new Rect(0,0,1,1);*

*├é┬á*

*public static void SetScissorRect( Camera cam, Rect r, Rect view )*

*{*

*if ( r.x < 0 )*

*{*

*r.width += r.x;*

*r.x = 0;*

*}*

*├é┬á*

*if ( r.y < 0 )*

*{*

*r.height += r.y;*

*r.y = 0;*

*}*

*├é┬á*

*r.width = Mathf.Min( 1 ├óÔé¼ÔÇ£ r.x, r.width );*

*r.height = Mathf.Min( 1 ├óÔé¼ÔÇ£ r.y, r.height );*

*├é┬á*

*cam.rect = new Rect (0,0,1,1);*

*cam.ResetProjectionMatrix ();*

*Matrix4x4 m = cam.projectionMatrix;*

*cam.rect = r;*

*Matrix4x4 m1 = Matrix4x4.TRS( new Vector3( r.x, r.y, 0 ), Quaternion.identity, new Vector3( r.width, r.height, 1 ) );*

***Matrix4x4 m2 = Matrix4x4.TRS (new Vector3 ( ( 1/r.width ├óÔé¼ÔÇ£ 1), ( 1/r.height ├óÔé¼ÔÇ£ 1 ), 0), Quaternion.identity, new Vector3 (1/r.width, 1/r.height, 1));*

*Matrix4x4 m3 = Matrix4x4.TRS( new Vector3( -r.x * 2 / r.width, -r.y * 2 / r.height, 0 ), Quaternion.identity, Vector3.one );*

*cam.projectionMatrix = m3 * m2 * m;*

*cam.rect = view;*

*}*

*├é┬á*

*void OnPreRender ()*

*{*

*SetScissorRect( camera, scissorRect, viewRect );*

*}*

*}*

The only thing I added here is the final line in the SetScissorRect() function. This renders the scissor cut to a different view rect (in my case I├óÔé¼Ôäóm always using 0,0,1,1 which is the full screen.

##### 

So, now I could split my camera view the way I needed it. I just needed to make it a little data driven to calculate the size of the scissor cuts. Since the test wall we use has only 4 touch screens and a smaller projector, I wanted code that would work on both. So I set it up to read a little text config file that told the program which node it was and how many nodes there are total. If the node number was the last one (so if it was the same as the number of nodes) it would make the assumption it was the projector node. Otherwise it would use the numbers to calculate how wide it├óÔé¼Ôäós cut was and where along the bottom it was. This worked a treat. After some trial and error I eventually realised (should not have taken me so long) that the aspects of the cameras had to all be the same. So whatever the aspects were on the bottom, that├óÔé¼Ôäós the aspect the top needed to be to line up.

Now we are at the last little problem to solve. Each touch screen has a frame on it. That actual rendering screen doesn├óÔé¼Ôäót go all the way to the edge. But the view into the world assumes otherwise. This isn├óÔé¼Ôäót a big deal on it├óÔé¼Ôäós own, the human eye can ignore that without even being aware of it. But the projector space has none of that. So anything that needs to line up where two touch panels meet and the top (like the very large robot in the centre), won├óÔé¼Ôäót line up properly because the pixels are next to each other physically on the projector space, but separated by the frame of the screens on the bottom. So I need to calculate the world space taken up by these frames and trim those off the scissor rects. This will mean some of the world will be hidden behind the frames of the screens, but that fits the world we are creating since these are supposed to be as windows into the scene. Plus it├óÔé¼Ôäós necessary to things lined up above and below.

The final step for the camera set up to be called working was getting the networking set up so something could pass from one screen to another. Since technically they are all showing different, but identical, scenes nothing could move across the environment. The main one for that will again be the large robot in the middle. Any animations he has need to be in sync across all nodes or it just won├óÔé¼Ôäót work. Luckily Unity├óÔé¼Ôäós networking is pretty easy to set up for something as simple as this. I wrote a little script that has a list of objects for each node. Anything in the scene that needs to sync across the network will be assigned to a node (to spread the load) and that node will instantiate them at load up. All the other nodes then get one created on their end automatically by the network library built into unity. Another little script checks to see if the object is owned by this node or not and deactivates any logic scripts if it isn├óÔé¼Ôäót so the only instance that├óÔé¼Ôäós doing anything is the one on the node that owns it. That worked very nicely and much quicker than I had anticipated (networking is not my strongest coder discipline), which is good since the camera set up took longer than I had hoped.

All in all, a lot of the core functionality for the set up is in. I have touch working to an extent, though the TUIO protocol isn├óÔé¼Ôäót super clean or easy to understand when you├óÔé¼Ôäóre used to unity├óÔé¼Ôäós built in Input handling touch on mobile. I├óÔé¼Ôäóll do another post on that shortly. The only final note I├óÔé¼Ôäóll make is about the data loading. As I said, I was using a text file to load in essentially 2 numbers that were used to define everything. This wasn├óÔé¼Ôäót the most convenient way to do it since each file had to have the same name (since the code loaded the file based on a path and file name) so putting the exe on each machine also meant manually dropping a text file on each and making sure you got the right one. So a bit of digging showed you could use command line arguments. .net gives very simple access to these. So we could run the exe from the command line with some extra arguments for the numbers. We were already running it from command line to use the -popupwindow argument that creates a window without a frame (this was needed to get fullscreen across two touch screens, since full screen defaulted to one of the two as windows treated it like an extended desktop over two monitors). So by creating shortcuts to a single network location, and modifying the path of each shortcut to include the extra arguments, we could set up each machine to run perfectly each time. All I have to do is replace the files on the network with a new version and when I run the shortcut on each machine it├óÔé¼Ôäós running the new code with it├óÔé¼Ôäós own arguments already set up. Made my testing process much quicker. If anyone is interested in seeing how to do set up command line arguments I├óÔé¼Ôäóm happy to do a very quick little post on it, it├óÔé¼Ôäós not difficult.

That├óÔé¼Ôäós all for now. Next post I├óÔé¼Ôäóll talk a bit about touch input. I want to wait till I have some placeholder UI in so I can talk about interacting with it, since it├óÔé¼Ôäós not as straight forward as I would have liked.

Hope you found that interesting,  
 Chat later,  
 Adsy.


