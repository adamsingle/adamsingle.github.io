---
layout: post
title: Setting up Unity3D with GIT, SourceTree and BitBucket on a Mac
image: http://res.cloudinary.com/adamsingle/image/upload/v1443437753/open-source-version-control-system-git_wehtbc.jpg
date: '2013-05-20 11:00:57'
tags:
- blog
- resource
---


So, this week is a technical blog. I├óÔé¼Ôäóve been flat out with work and have had no time to further our games at all. Incidentally I├óÔé¼Ôäóll be announcing the name of the studio I├óÔé¼Ôäóll be forming soon. I├óÔé¼Ôäóll be writing up some stuff for it first though, so until then it├óÔé¼Ôäós still just vague ├óÔé¼╦£our games├óÔé¼Ôäó references.

So this week I have a bit of a tutorial of sorts. I├óÔé¼Ôäóve had to do set up version control for a project a couple of times now, and have had to re-teach myself the steps each time, so this time I wrote them down. Firstly I├óÔé¼Ôäóll talk a little about my reasoning for using what I use, then I├óÔé¼Ôäóll get to the meat of it all. I├óÔé¼Ôäóve included screen shots, so hopefully if you├óÔé¼Ôäóre following along there are no dramas.

Why GIT?

There are two reasons I use GIT.

1. I use a Mac, and the best version control client I├óÔé¼Ôäóve found is SourceTree. SourceTree doesn├óÔé¼Ôäót support SVN, but it does handle GIT very nicely.
2. GIT allows you to create branches. Branches create copies of a project at a given time. You can then work on that branch, implementing a feature you aren├óÔé¼Ôäót sure about, or you think will take a long time, without effecting the master branch. This can be good for two reasons.

- 1. If you are working with a team they can all have their own branches where they implement specific features that are assigned them, while this is happening the master branch can be kept as an ├óÔé¼┼ôalways playable├óÔé¼┬Ø build. Once the feature is complete and tested and 100% working, it can be merged back into the master branch.
- 2. Secondly, if you decide, after a lot of work and modification to the core of the code base, that the feature isn├óÔé¼Ôäót going to work, isn├óÔé¼Ôäót fun or is too heavy, you can simply kill the branch and go back to the game as it was before you started implementation. Nothing needs to be ripped out, no extranuous code will be left floating around in there. It├óÔé¼Ôäós all clean.

├é┬á

Why BitBucket?

Ok. So why BitBucket. There are a few other options, GitHub is extremely popular, and the first that jumps to mind. I have two reasons for choosing BitBucket.

1. You can have an unlimited number of repositories on the free account

2. You can set repositories to private

Why Sourcetree?

SourceTree is free and an excellent visual interface to use with GIT. It shows the branches, files that were commited, comments with commits, diff comparisons. It├óÔé¼Ôäós just much easier than the terminal. Incidentally, it├óÔé¼Ôäós also made by the same people that created BitBucket, so it plays nice.

So let├óÔé¼Ôäós get to it.

First you need to set up External Version Control in your Unity project.

- Edit->Project Settings->Editor
- Version Control Mode ├óÔé¼ÔÇ£ Meta Files
- [![Image 2 - Settings](http://res.cloudinary.com/adamsingle/image/upload/v1443437755/Image-2-Settings_rdkclf.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437755/Image-2-Settings_rdkclf.jpg)
- Asset Serialization ├óÔé¼ÔÇ£ Mixed. Unity made some big changes to allow you to force everything to be text files, essentially making everything serialise into something like XML. Unfortunately this doesn├óÔé¼Ôäót particularly help much. The main issue with version control in Unity is that the Scenes themselves are usually binaries. Binaries can├óÔé¼Ôäót be merged. This means if two developers are working on the scene at the same time someone will have to discard their changes to accept the others then make them again. Technically this is solved by making the scene a text file, except resolving conflicts when something as abstract as a scene has been stored as a massive massive XML doc is crazy. So leave it as mixed, leave the scenes as binaries so they are nice and small, and just pay attention to who├óÔé¼Ôäós working on which scene.
- Ignore Host URL

├é┬á

[Download Sourcetree and install it.](http://www.sourcetreeapp.com/download/ "SourceTree download link")

├é┬á

[Create an account with BitBucket](https://bitbucket.org/ "BitBucket") (I already had a GitHub account so I signed up using that account for ease of use)

├é┬á

Select ├óÔé¼┼ôCreate repository├óÔé¼┬Ø

[![Image 1 - BitBucket Dash](http://res.cloudinary.com/adamsingle/image/upload/v1443437810/Image-1-BitBucket-Dash_iz3lzn.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437810/Image-1-BitBucket-Dash_iz3lzn.jpg)

├é┬á

Give your repo a name

Give it a description

You can have unlimited private and public repos on BitBucket (my favourite thing about it)

Repo type is GIT

If you would like you can have Issue Tracking and a wiki. Both could be a good idea for managing the code base (issue tracking) and game design (wiki). The wiki could even be used in a similar way to a [design log as described by one of my favourite game designers Dan Cook](http://www.lostgarden.com/2011/05/game-design-logs.html "Design Logs")

Choose your language (even UnityScript is in this drop down!).

Select Starting from scratch.

Now we need to set up our project as a repository (repo).

The instructions for this are handily shown by BitBucket, but I├óÔé¼Ôäóll go through them here as well.

Open terminal.

Navigate to your project folder

$ cd /path/to/my/repo (for those that don├óÔé¼Ôäót know, the $ in these situations aren├óÔé¼Ôäót typed by you, they are a standard of some sort for showing what the user must type. It took me a little while to realise that when I began messing with the mac terminal).

$git init

$ git remote add origin ssh://git@bitbucket.org/<BitBucketUserName>/<RepoName>.git

again, this is very nicely mentioned in the BitBucket site, it will include the username and reponame for the project you├óÔé¼Ôäóre setting up so you could even copy and paste to be sure you├óÔé¼Ôäóve got it right.

Now we have everything that we need set up to use SourceTree instead of the terminal.

Set up repo in SourceTree

- Open SourceTree
- Select Add A Repo (That├óÔé¼Ôäós the Can looking thing with a green plus in the top left).
- Select Add Working Copy (the folder and can icon)
- Navigate the Working Copy Path to your Unity project folder. If everything is set up you should see ├óÔé¼┼ôThis is a GIT repository├óÔé¼┬Ø. If not, you may be selecting the wrong folder. You want the root folder of the project, it should contain the Assets and ProjectSettings folder amongst others. If not, find that one. If you are at that one then you set up a git repo in the wrong folder. Find the one you set it up in, delete the .git folder (it├óÔé¼Ôäós a hidden folder) from it, go to BitBucket and delete the repository and start again. It shouldn├óÔé¼Ôäót take long to catch back up.
- Give the repo a name (it├óÔé¼Ôäóll have the folder name by default which should be fine)
- Now the repo appears (or you may have to double click it on the list).
- This screen can look a little confusing at first, but it├óÔé¼Ôäós pretty straight forward. There are 4 ├óÔé¼┼ôpanels├óÔé¼┬Ø. On the left is a narrow one that is pretty simple at this stage because we have no branches. Here you can see any Remote repos (as this one is), any branches, tags, stashes or sub modules. These are all features of GIT that you may find useful one day. For now we are just going to use the origin (sometimes called master branch), so make sure it is highlighted.
- In the middle of the screen are two panels, one atop the other. The top is called Files staged in the index and should be empty. The bottom is called Files in the working tree and should be full.
- On the right is difference panel. This will show the changes made between a file existing on the remote repo and the one that is selected. This is especially handy for code files since it shows exactly what you├óÔé¼Ôäóve changed since last commit.
- The files in the working tree should only show files that need to be committed (that is new or deleted ones, or ones that have been modified since the last push to the remote repository). This can be changed with the drop down above the Files staged in the index panel which is currently set to Show Pending. We want to simplify the Files in the working tree panel. We don├óÔé¼Ôäót want to track everything in a Unity project. A lot of these things are automatically created, so if you are collaborating you want them to pull the project, open it in Unity and let their Unity set this stuff up. All you need is the Assets folder and the ProjectSettings folder. So, first of all, collapse any expanded folders (assuming you have it showing in tree mode, which I do for ease of reading, again, this is a drop down at the top, just to the left of the Show Pending drop down). Now select everything except the Assets and ProjectSettings folders. Right click and select Ignore.
- A pop up will give you some options. Select Ignore in this Repository.
- You├óÔé¼Ôäóll now see a very neat and tidy Files in the Working Tree panel. Notice the .gitignore file. That lists all those files and folders you just told it to ignore.
- [![Image 3](http://res.cloudinary.com/adamsingle/image/upload/v1443437755/Image-3_igre9y.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437755/Image-3_igre9y.jpg)
- Now we want to commit. Commit is the process of staging files, that is readying them to be pushed to the remote repository. To do that simply select all of them (including the .gitignore) and drag them up to the Files staged in the index panel.
- [![Image 4](http://res.cloudinary.com/adamsingle/image/upload/v1443437754/Image-4_vt19pp.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437754/Image-4_vt19pp.jpg)
- You├óÔé¼Ôäóll notice, if you expand the folders, that every file and folder has a .meta file and that every file (including .metas) have a green plus next to them. The plus, as you probably guessed, means they are new to the repo and are being added. It├óÔé¼Ôäós important that all meta files are included. If they go missing Unity won├óÔé¼Ôäót be able to set things up properly, this is especially important for prefabs since the meta files keep the prefab linked to any resources it uses.
- Now you just press commit at the top.
- [![Image 5](http://res.cloudinary.com/adamsingle/image/upload/v1443437754/Image-5_czezje.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437754/Image-5_czezje.jpg)
- A commit will ask for a comment. It is always a good idea to comment your commits well. When something breaks and you need to go back to the point you changed it, especially if it├óÔé¼Ôäós code and you just want to pluck the class back out of the repo from before you broke it, your comments can save a lot of time. For this one, something like ├óÔé¼┼ôInitial commit├óÔé¼┬Ø is fine.
- You├óÔé¼Ôäóll notice this also shows the files that are being commited, and any that are in the working copy that haven├óÔé¼Ôäót been committed, in case you missed some somehow. When you├óÔé¼Ôäóre ready, press Commit.
- [![Image 6](http://res.cloudinary.com/adamsingle/image/upload/v1443437753/Image-6_vv8ypw.jpg)](http://res.cloudinary.com/adamsingle/image/upload/v1443437753/Image-6_vv8ypw.jpg)
- That is now committed locally. All we need to do is Push it to the repository on BitBucket. It├óÔé¼Ôäós already linked to that repo, so simply press Push and away it goes. Push beings with it another dialog, here you can select which branch to push to. We only have the master, so select it and press OK.
- Let it do it├óÔé¼Ôäós thing, it can take a little while sometimes, depending on if you imported any packages into your project when starting up.
- Once that├óÔé¼Ôäós done you should see ├óÔé¼┼ômaster├óÔé¼┬Ø under ├óÔé¼┼ôbranches├óÔé¼┬Ø on the left. If you select it you get some rearanging of panels and some new panels.
- The difference panel is still there. The top panel is now a tree of commits. This will look more interesting and helpful after a few more. Selecting a commit will give you details in the other panels. It will show what files were effected, and the comment, date, author etc of the commit.
- That├óÔé¼Ôäós it. You are all done. Any time you work on your project, you can come back here, it will automaticaly update and show any files that have changed and need to be committed and you just follow the same process.

You can jump back to BitBucket.org if you like and see for yourself that it├óÔé¼Ôäós there. Select the name of the repo and it will show you Recent activity (which should have the push you just did. The SSH URL is on the right for future reference (for instance if someone else wants to pull your project and help you work on it). The number of branches, tags, forks and followers of the project. You can invite users from here. It├óÔé¼Ôäós all very simple and intuitive.

A couple of last minute tips.

Try to commite regularly and small changes at a time. Focus on getting a single thing working, then commit it. There is no harm in having many commits. If everything breaks it will make life much easier.

Make it a habit to save the scene and the project before each commit. Many times there won├óÔé¼Ôäót be any changes to them, but sometimes there are. Things like code are automatically updated just by being built. But any changes to prefabs are only saved when you save the project. If you change a prefab, double check that it shows up in the Files in the working tree panel (they have a .prefab extension). If not, you forgot to save the project first.

That├óÔé¼Ôäós about it. I made that long winded, I know, but I wanted it to be as clear as possible. You now have a nice pipeline for version control. Remember that you will still get conflicts if more than one person alters a scene at the same time as you can├óÔé¼Ôäót merge binary files. This means when you commit, one of you will have to discard your changes, accept the other├óÔé¼Ôäós version, pull it, redo your changes, then commit it, so communicate. I would recommend using something like a [Trello board](http://www.trello.com "Trello") to track scene usage if you have a few scenes in the game. If there├óÔé¼Ôäós only one, simple communication should suffice.

Now you can focus on exploring interesting mechanics and ideas and not worry so much about breaking anything. If anyone reading this doesn├óÔé¼Ôäót know much about GIT I recommend doing some reading, branching is a particularly useful tool for safely exploring mechanics and other ideas, especially if you are working with a team of people and you don├óÔé¼Ôäót want to break the current master branch copy.

Enjoy!


