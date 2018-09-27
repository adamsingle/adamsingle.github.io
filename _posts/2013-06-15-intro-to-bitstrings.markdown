---
layout: post
title: Introduction to Binary and Bitstrings with C#
image: http://res.cloudinary.com/adamsingle/image/upload/v1443437752/Binary-Code_llevdp.jpg
date: '2013-06-15 21:27:17'
tags:
- blog
- resource
---


Below is a reprint of an old blog post I wrote in November of 2011. It├óÔé¼Ôäós a little Unity oriented and makes mention of OpenFeint, which I was using a lot of at the time. OpenFeint was a cross platform leader board and achievement platform that no longer exists. It├óÔé¼Ôäós a long post. Probably too long really, but I like to think it├óÔé¼Ôäós a great resource for new coders as much as those that have been doing it a little while, but haven├óÔé¼Ôäót come across bitstrings before. Enjoy, and let me know what you think.

├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇ£

So today I├óÔé¼Ôäóll be talking about making a mass of bools more efficient, both in time to code (smaller functions) and memory.

I├óÔé¼Ôäóm going to do that by utilising bit manipulation. Now I├óÔé¼Ôäóm not an expert coder, I├óÔé¼Ôäóm learning as I go. I do have an accelerated degree in Interactive Entertainment with a major in Games Programming, so I├óÔé¼Ôäóm not completely uneducated in this area, but until you try to do something in code that you├óÔé¼Ôäóve never done before, you don├óÔé¼Ôäót really know if you know it yet. So I├óÔé¼Ôäóm going to talk about what I have figured out and learnt in the last 2 days.

First let├óÔé¼Ôäós assume you are new to coding, or self taught and haven├óÔé¼Ôäót come across bit manipulation yet. Bit manipulation refers to using (and changing or manipulating) the memory at a single bit level. As you no doubt know, a bit can be either one of two states, on or off, 1 or 0. I├óÔé¼Ôäóm sure you can already see the connection between a bit and a bool. Before we get too much into that, let├óÔé¼Ôäós talk about how they can be manipulated. Basically all your logic operators (AND, OR, XOR, NOT) can be applied to bits. Syntactically they are similar.  
 A logic AND is &&.

E.g. If(one && two)

Which will only return true if both one AND two are true.

A logic OR is ||

E.g. If(one || two)

Which returns true if either one OR two is true.

For those of you wondering an XOR or ├óÔé¼┼ôexclusive or├óÔé¼┬Ø is ^^ I believe. It rarely comes up in regular coding in my experience, though I do use it in bit wise operations as we├óÔé¼Ôäóll see later. Exclusive or is true only if one is true and one is false, but it does it doesn├óÔé¼Ôäót matter which.

E.g. If(one ^^ two)

Which returns true if one is true and two is false OR one is false and two is true. But not if both are true.

This is a good place to mention if I have anything wrong please feel free to email. E or post a comment. Like I said I├óÔé¼Ôäóm not an expert, I├óÔé¼Ôäóm just trying to impart some knowledge as I discover it. However I haven├óÔé¼Ôäót ever used logical XOR in my code so I├óÔé¼Ôäóm only guessing it is represented by ^^.

So that├óÔé¼Ôäós the logic operators, which you probably already knew. You may have wondered, like I did when i started coding, why they are double symbols. My guess here is the bitwise operators came first and to separate the logic ones from the bitwise ones they doubled the symbols. You├óÔé¼Ôäóve probably guessed then that the bitwise operators are the same, but single character.

Understanding what they do is simple. The whole 1and 0 thing can be confusing, but if you starting out think of them as true and false instead and it├óÔé¼Ôäóll make sense.  
 Now we├óÔé¼Ôäóll take a quick look at something you might manipulate. To keep it simple lets look at a byte.  
 Assume we have two bytes.  
 one is 11001010  
 Two is 11100001

I know that can look pretty abstract but bear with me. If we want to AND these bytes we simple and each position with its corresponding position in the other byte. Simple to see if we put one above the other. Remember, think of them as true and false.

11001010 &  
 11100001  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ-  
 11000000

So only the bit positions that both have ones (so both are ├óÔé¼┼ôtrue├óÔé¼┬Ø) return one (├óÔé¼┼ôtrue├óÔé¼┬Ø) exactly like a logic AND

Ok, let├óÔé¼Ôäós OR them.

11001010 |  
 11100001  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇ£  
 11101011.

So any that had either one or the other ├óÔé¼┼ôon├óÔé¼┬Ø (├óÔé¼┼ôtrue├óÔé¼┬Ø) resulted in an ├óÔé¼┼ôon├óÔé¼┬Ø bit in the answer.

Finally let├óÔé¼Ôäós quickly look at an XOR.

11001010 ^  
 11100001  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇ£  
 00101011

Here you can see that the positions that both were on are now off.

Ok. Cool. What the heck is the point though?

The point is we can use these operators to turn off and on specific positions. Imagine we have an empty byte. We want to turn on the 3rd position specifically. Easy, we just OR it.

00000000 |  
 00000100  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ-  
 00000100

Now obviously we could have just said it equaled the second byte. But what if the first one wasn├óÔé¼Ôäót empty. What if we had something like the following:

11010010

now turning on the third bit without destroying the rest would seem a little tricky. But if we OR it with our byte above we get

11010010  
 00000100  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ  
 11010110

which is exactly the same but with the third bit on. Plus, if it had already been on we would not have done any damage.

What about turning one off you ask. Easy peasy. You OR it with the NOT of the one you want off├óÔé¼┬ª see├óÔé¼┬ªsimple.

So lets say we want to turn that pesky third bit back off. Here├óÔé¼Ôäós how to do it.

We have:  
 11010110 and we want to AND it with the NOT of the bit we want off, which would be  
 11111011

In this way, anything that was on stays on, anything that was off stays off and our special bit is turned off.

And just quickly before moving on, to tell if a specific bit is on or not you and it with the but you want to check. So let├óÔé¼Ôäós see if that third bit is on.

11010010 &  
 00000100  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ  
 00000000

what about the 8th├é┬ábit?

11010010 &  
 10000000  
 ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ  
 10000000

so if the result is all 0├óÔé¼Ôäós your bit is off. Anything else and it├óÔé¼Ôäós on.

├óÔé¼┼ôThat├óÔé¼Ôäós all great├óÔé¼┬Ø you say, but I still don├óÔé¼Ôäót see the point. Well I├óÔé¼Ôäóm getting to it. Some of you may have already seen it. These bytes can be represented as numbers. For those of you who understand your binary it├óÔé¼Ôäós simply a base 2 number system with the same rules as the base 10 one we use everyday.

So our base 10 (or decimal system) works on columns of 10 to the power of (which I will represent with ^ so don├óÔé¼Ôäót get that confused with the bitwise XOR)

We saw this at school. We have the units column, the tens column, the hundreds column, the thousands column and you just add them all up.

So 245 is:

(2 * 10^2) + (4 * 10^1) + (5 * 10^0). Note: Anything to the power of 0 is 1.

= 200 + 40 + 5  
 =245

Binary works the same, except it is base 2 so each column is 2 to a power.  
 101 is:

(1 * 2^2) + (0 * 2^1) + (1 * 2^0)  
 = 1*4 + 0 * 2 + 1 * 1  
 = 5  
 so 101 in binary is 5.

So what does that mean for us? It means these things can be represented as numbers that can be saved to PlayerPrefs (look out, I├óÔé¼Ôäóm starting to get to a relevant point here).

Practical Usages

Now we can├óÔé¼Ôäót save a byte to PlayerPrefs. But we can save an int. That├óÔé¼Ôäós handy, but we haven├óÔé¼Ôäót seen a use for this yet. You├óÔé¼Ôäóll remember at the beginning I mentioned bools. If your games are anything like mine, they can have a tendency to have a mass of bools. Since I├óÔé¼Ôäóm making iOS and Android games and using OpenFeint I├óÔé¼Ôäóm tracking a heap of achievements at the least. Each one is a bool. It occurred to me that this is very inefficient in two ways.

Firstly a the size of a bool is at least a byte. I did a quick google on the size of a bool and there is some contention. Most say a byte, but there seem to be some possible situations where they can be 2 bytes, or even 4. That├óÔé¼Ôäós crazy, a bool is a binary state, it├óÔé¼Ôäós true or false, on or off, 1 or 0. Even if it├óÔé¼Ôäós only one byte that├óÔé¼Ôäós 8 times bigger than it really needs to be.

Secondly my functions are ridiculous. If I have 20 something achievements I have a function for unlocking each one. I could optimise that by having an enum perhaps and passing that in. Then I├óÔé¼Ôäód have one function that would have a switch statement in it with a case for each one. And I├óÔé¼Ôäód still have 20 something bools being written to PlayerPrefs.

So I have way too much code to write and manage and I├óÔé¼Ôäóm saving 20 bytes (assuming 20 bools) at least. That├óÔé¼Ôäós 160 bits to represent the same data that could be represented using 20 bits.

So let├óÔé¼Ôäós do something about it.

The Octobool

Octobool is a term that a good mate of mine introduced to me. It was the way he had been taught to handle this type of situation and it basically uses a single byte to hold the data of 8 bools. Each bit is a bool. The problem for us there is we can├óÔé¼Ôäót write a byte to PlayerPrefs. Which brings me to├óÔé¼┬ª

BitStrings

BitStrings are what I was taught. It uses an int to store the bools. Obviously if you have 8 or less bools to store a byte would be better, but you├óÔé¼Ôäód still have to store it as an int in the PlayerPrefs. With a BitString you can use an int that suits your needs. There are standard ints, which are normally 32 bits, shorts (16 bits) and longs (64 bits). There are also signed and unsigned. Let me just say here it├óÔé¼Ôäós best to use unsigned. An int is usually signed by default, so I would recommend you use an unsigned int (for those new to coding an unsigned int is one that simply has no sign, meaning it can├óÔé¼Ôäót be negative. It also means that the bit that would have to be reserved to represent the sign is freed up). The problem with signed ints is you can├óÔé¼Ôäót use the sign but as a bool because the end result (the number) would then vary depending on if that bool is true or false. To access unsigned ints you simply put a u in front of the type declaration. For short and long you just use the types short and long. Simple.  
 So:  
 int standardInt; //32bit value between -2147483648 and 2147483647 (-2^31 and 2^31 -1)  
 uint unsignedStandardInt; //32bit value between 0 and 4294967295 (2^32 -1)  
 short shortInt; //16bit  
 ushort unsignedShort; //16bit positive values  
 long longInt; //64bit  
 ulong unsignedLongInt; //64bit positive values

(As a note I took a quick look at MSDN and I├óÔé¼Ôäóm not sure but I think a long can only be signed, they don├óÔé¼Ôäót mention an unsigned version, however in MonoDevelop ulong is a recognised type so I├óÔé¼Ôäóm a bit up in the air there. I think it must exist if the type is recognised by the IDE, so if you have more than 32 bools that you would like to store together, I├óÔé¼Ôäóm pretty sure you can do it).

BitArrays

BitArrays are a structure that exists in c# that I at first thought was a BitString, which was going to save me having to write a class myself. However I had quite a bit of trouble with it. Mainly getting it├óÔé¼Ôäós contents into an int for saving. It seemed to have a CopyTo() function but that could only save to ints that were in an array, so I had to make an int array with one int in it to copy the BitArray into and manipulating the contents of the BitArray wasn├óÔé¼Ôäót as straight forward as I would have liked. I posted the question on the Unity forum and was pointed in the right direction (thanks Ntero!). There is a structure in c# to do what we want. It├óÔé¼Ôäós called a FlaggedEnum.

FlaggedEnum

It├óÔé¼Ôäós a little difficult to wrap your head around at first, I think basically because it├óÔé¼Ôäós very simple. I will quickly explain an enum for those who aren├óÔé¼Ôäót sure what they are. An enum is basically wrapping an integer in a word to make it easier to read your code. So maybe you have your game states stored as an enum. There is a menu state, a paused state, a play state, maybe and endscreen state or something like that. You then create an enum that has words to represent each of these, but underneath they are really just integers. You could do the same thing with #DEFINE or even just an int with some comments. Something like:

int gameState; //1 = menu, 2 = play, 3 = paused, 4 = end screen.

And you just have to remember those. Your game loop has a switch in it that checks these and you have your logic for each state. You can do the same thing with an enum but it makes you game loop look a little clearer.

enum GameState  
 {  
 GS_MENU,  
 GS_PLAY,  
 GS_PAUSED,  
 GS_END_SCREEN  
 };

GameState m_state;

couple of things to note there.  
 First that is my personal convention for defining an enum, most of the coders I have seen do the same. Capitals for the values, use underscores, and usually have some sort of code at the start. In this case there is unlikely to be another enum with similar names, but it├óÔé¼Ôäós safer this way. So GS is for GameState. Notice there are commas after each entry except the last (you can have one, but you don├óÔé¼Ôäót need it). Also note the semi-colon after the closing curly bracket. Don├óÔé¼Ôäót forget to actually create an instance of this to use in your game as well.  
 So I have no numeric values in here. The compiler will therefore assume it starts at 0 and increments. So GS_PLAY is actually equal to 1. i.e. if you wrote:

Debug.Log(((int)GS_PLAY).ToString());

it would debug out 1

what I could do is start anywhere:

enum GameState  
 {  
 GS_MENU = 10,  
 GS_PLAY,  
 GS_PAUSED,  
 GS_END_SCREEN  
 };

in which case GS_PLAY now equals 11 and so on.

Take it a step further and I can do this:

enum GameState  
 {  
 GS_MENU = 10,  
 GS_PLAY = 20,  
 GS_PAUSED = 100,  
 GS_END_SCREEN = 55  
 };

There is absolutely no reason to do this (and many reasons not to), but it works. It├óÔé¼Ôäós great that we can do this, because we need it for the next step.

So that├óÔé¼Ôäós enums. FlaggedEnums have a flags attribute set on them. This allows you to do bitwise operations on them. To do that all you need to do is add the following line:  
 [FlagsAttribute]

Lets create an enum that has a little more relevance. If you would like to access this from any of your scripts just make a class for your enums. It would look something like this.

using UnityEngine;  
 using System;  
 using System.Collections;

public class FlaggedEnums : MonoBehaviour  
 {  
 [FlagsAttribute]  
 public enum AchievementFlags  
 {  
 AF_ACHIEVEMENT_1,  
 AF_ACHIEVEMENT_2,  
 AF_ACHIEVEMENT_3,  
 AF_ACHIEVEMENT_4,  
 AF_ACHIEVEMENT_5,  
 AF_ACHIEVEMENT_6,  
 AF_ACHIEVEMENT_7,  
 AF_ACHIEVEMENT_8,  
 AF_ACHIEVEMENT_9,  
 AF_ACHIEVEMENT_10,  
 AF_ACHIEVEMENT_11,  
 AF_ACHIEVEMENT_12,  
 AF_ACHIEVEMENT_13,  
 AF_ACHIEVEMENT_14,  
 AF_ACHIEVEMENT_15,  
 AF_ACHIEVEMENT_16,  
 AF_ACHIEVEMENT_17,  
 AF_ACHIEVEMENT_18,  
 AF_ACHIEVEMENT_19,  
 AF_ACHIEVEMENT_20,  
 }  
 }

Which is great. But if we are going to perform bitwise operations on this and expect it work we need each of these to represent a bit in the number, so we need to modify to this:

using UnityEngine;  
 using System;  
 using System.Collections;

public class FlaggedEnums : MonoBehaviour  
 {  
 [FlagsAttribute]  
 public enum AchievementFlags  
 {  
 AF_ACHIEVEMENT_1 = 1, //0000 0000 0000 0000 0000 0000 0000 0001  
 AF_ACHIEVEMENT_2 = 2, //0000 0000 0000 0000 0000 0000 0000 0010  
 AF_ACHIEVEMENT_3 = 4, //0000 0000 0000 0000 0000 0000 0000 0100  
 AF_ACHIEVEMENT_4 = 8, //0000 0000 0000 0000 0000 0000 0000 1000  
 AF_ACHIEVEMENT_5 = 16, //0000 0000 0000 0000 0000 0000 0001 0000  
 AF_ACHIEVEMENT_6 = 32, //0000 0000 0000 0000 0000 0000 0010 0000  
 AF_ACHIEVEMENT_7 = 64, //0000 0000 0000 0000 0000 0000 0100 0000  
 AF_ACHIEVEMENT_8 = 128, //0000 0000 0000 0000 0000 0000 1000 0000  
 AF_ACHIEVEMENT_9 = 256, //0000 0000 0000 0000 0000 0001 0000 0000  
 AF_ACHIEVEMENT_10 = 512, //0000 0000 0000 0000 0000 0010 0000 0000  
 AF_ACHIEVEMENT_11 = 1024, //0000 0000 0000 0000 0000 0100 0000 0000  
 AF_ACHIEVEMENT_12 = 2048, //0000 0000 0000 0000 0000 1000 0000 0000  
 AF_ACHIEVEMENT_13 = 4096, //0000 0000 0000 0000 0001 0000 0000 0000  
 AF_ACHIEVEMENT_14 = 8192, //0000 0000 0000 0000 0010 0000 0000 0000  
 AF_ACHIEVEMENT_15 = 16384, //0000 0000 0000 0000 0100 0000 0000 0000  
 AF_ACHIEVEMENT_16 = 32768, //0000 0000 0000 0000 1000 0000 0000 0000  
 AF_ACHIEVEMENT_17 = 65536, //0000 0000 0000 0001 0000 0000 0000 0000  
 AF_ACHIEVEMENT_18 = 131072, //0000 0000 0000 0010 0000 0000 0000 0000  
 AF_ACHIEVEMENT_19 = 262144, //0000 0000 0000 0100 0000 0000 0000 0000  
 AF_ACHIEVEMENT_20 = 524287, //0000 0000 0000 1000 0000 0000 0000 0000  
 }  
 }

As you can see, there are still some spaces in that 32bits that we could use if we need to. Base 2 is pretty simple to calculate, just keep doubling the previous value to get the next one.  
 I tend to also set two more values, in this case:

AF_ALL_OFF = 0;  
 AF_ALL_ON = 1048573 // this is 20 bits on. I could have used 4294967295, which would have been all 32 bits on, this would have allowed me to add some more achievements in without needing to push the ALL_ON flag further out. Either way will work, you only ever test the values you├óÔé¼Ôäóve set anyway.

Now how do we use this?

Exactly like we did at the beginning with the bitwise operators.

We have an instance of this enum which probably holds a value that doesn├óÔé¼Ôäót even exist on that list above because it├óÔé¼Ôäós a combination of them. But that├óÔé¼Ôäós fine. Lets say if we have a blank, empty copy. We want to unlock an achievement. Remember our old way. We├óÔé¼Ôäód have maybe 20 functions, one for each achievement. Something like:

manager.UnlockAchievementOne();

which would call:

public void UnlockAchievementOne()  
 {  
 if(!achievementOneUnlocked)  
 {  
 achievementOneUnlocked = true;  
 openFeint.UnlockAchievement(achievementOneID); //this is still going to be an issue for us and we would use key/value pair to handle this, but I├óÔé¼Ôäóll do another post on that later.  
 }  
 }  
 Or we could have something like:

manager.UnlockAchievement(AchievementEnum.ACHIEVEMENT_ONE);

which would call:

public void UnlockAchievement(AchievementEnum achievement)  
 {  
 switch(achievement)  
 {  
 case AchievementEnum.ACHIEVEMENT_ONE:  
 {  
 if(!achievementOneUnlocked)  
 {  
 achievementOneUnlocked = true;  
 openFeint.UnlockAchievement(achievementOneID);  
 }  
 break;  
 }  
 etc├óÔé¼┬ª..  
 }  
 }

Or this way we can do the following

manager.UnlockAchievement(AchievementFlags.AF_ACHIEVEMENT_1);

which would call:

public void UnlockAchievement(AchievementFlags unlockedAchievement)  
 {  
 if((mAchievements & unlockedAchievement) ==├é┬á0) //if this achievement is not already unlocked.  
 {  
 mAchievements = mAchievements | unlockAchievement; //turn on the flag in our state of ├é┬á ├é┬á achievements.  
 //this is where we would use a key/value pair to get the ID of the achievement  
 }  
 }

and that would be the whole function, no switch, just a few lines.

Now how do we save that?

Easy.

uint stateOfAchievements = (uint)mAchievements;  
 PlayerPrefs.SetInt(├óÔé¼┼ôstate of achievements├óÔé¼┬Ø, stateOfAchievements);

and to load is the opposite.

uint stateOfAchievements = PlayerPrefs.GetInt(├óÔé¼┼ôstate of achievements);  
 mAchievements = (AchievementFlags)stateOfAchievements;

And that is that. I think.

I hope that was informative and helpful. I really hope it was clear. I have an 11 month old who doesn├óÔé¼Ôäót sleep well so I├óÔé¼Ôäóm pretty tired and bit manipulation can do your head in at the best of times. If you have any questions don├óÔé¼Ôäót hesitate to comment or email, I├óÔé¼Ôäóll usually respond fairly quickly. If you want to correct me, again go right ahead.

I have another blog about Integrating OpenFeint with Unity (yes, number 3) that uses the latest (for now) versions of everything. Again there have been a few changes, so that should be up shortly. I├óÔé¼Ôäóll also endeavour to put up a little post on key/value pairs if anyone is interested. I haven├óÔé¼Ôäót used them much, but they are pretty straight forward.

Thanks for reading.

Adsy.

├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ├óÔé¼ÔÇØ

So that was the original post. It├óÔé¼Ôäós long, but I think sufficiently detailed for complete beginners. Bitstrings are a complex and somewhat abstract concept. But they can also be very useful. What I├óÔé¼Ôäóve described here was the first case I needed them for. Using them to store a heap of meaningful bools is one way. But they can also be very handy in tilebased systems. I will follow this post up with another in the coming weeks describing how to use them in much larger amounts to store information on visibility or binary tile states (such as active/inactive or walkable/non-walkable).

One final note, I├óÔé¼Ôäóve noticed, when using FlaggedEnums you can debug them out using .ToString() and it will actually display all the enum identifiers that are on. Eg. (ACHIEVEMENT_01,ACHIEVEMENT_03,ACHIEVEMENT_08)

Hope that was informative.

Chat later,

Adsy


