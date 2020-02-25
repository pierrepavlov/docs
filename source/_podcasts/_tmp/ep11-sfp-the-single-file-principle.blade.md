---
number: 11
filename: ep11-sfp-the-single-file-principle
title: 'SFP: The Single File Principle'
duration_in_minutes: '09:53'
published: true
description: 'Let''s chat about a super practical pattern I use in everyday life. Often when I''m adding a feature or a "concern" I try to isolate the entire changeset to one file. It''s surprisingly effective.'
long_description: "Okay today. I want to talk about a new principal object oriented principle. I just made up called SFP the single file principle and this is also sort of my way of communicating something without saying SRP single responsibility principle which tends to get people with their torch and pitchforks out or the stone tablets and preach from the mountaintops or be mad about the people preaching for the mountaintops or whatever, you know, there's been a lot of discussion around it.\r\n\r\nSo I don't really want to talk about it. But basically this is a really practical principle that I'm going to State as such if you are implementing a feature or if you recognize a I'm just going to use the word feature the fancy term would be a reason to change but if you recognize a feature or you're adding a feature in your app, see if you can keep it all to one file if the file is too big.\r\n\r\nThen maybe you'd have to think about this in smaller terms and see if there's some features you can keep to one file. This sounds so silly and so obvious but I'm telling you this is actually one of the most valuable patterns that I employ when I'm writing code. So I'll give you a few practical examples and hopefully it'll start to sink in.\r\n\r\nSo when I started writing Livewire basic, I mean there's a whole other episode to talk about how. How code design emerges rather than as like planned like I don't really plan designs almost ever. I just start writing code that feels right and I just keep following that right feeling and a couple principles and patterns and I start to see when patterns emerge and then I tackle them and refactor and things like that.\r\n\r\nI don't think you can really do much planning up front. But anyway started to build Live Wire and certain pattern started to emerge and here was one eye when I was we're going to talk about the make Command the famous make Command where you type artist and. Artisan make Livewire and then the component name.\r\n\r\nI had to turn the component name into a class name space because I need to create that file and I also need to convert it into a view name like a blade file name. There's a bunch of other stuff I need to do. What is the name of the component? Is it you know, if it's counter, it's just counter.\r\n\r\nWhat's the name of the class? What's the name of the class name space the full name space the file path the view the view name like Livewire dot counter and the view file clap path like resources views Live Wire, right? So there's a lot of these things so and there's relative paths and there's absolute past.\r\n\r\nThere's just it goes on and on and I recognized so for the first iteration, I wrote all this logic inside of the command class inside of the class. I wrote called Livewire may come in. And but I forget exactly where I needed it next but maybe I wrote the Livewire component discover, which we can talk about another episode but inside the discoverer I needed to I needed to use a lot of this logic.\r\n\r\nSo, you know, I had I had an option you have a fork in the road. I could just copy and paste the logic from the command. Or I could make an abstraction abstraction. And what would that be? I don't know. Maybe it would be a trait. Maybe it would be some helper functions. I'm not really sure yet. And as a rule of thumb, I generally just start with copy and pasting I think j-mac talks about the rule of three.\r\n\r\nI don't know if that's something he coin, but I've definitely heard it before. And the idea is that an abstraction or something is not duplicated or like you live with one level of duplications. So if you copy and paste code once that's fine, but if you need it for the third time, then you should think about an abstraction.\r\n\r\nIt's a good rule of thumb and I try to follow that myself but in this instance, so I did that and then another I forget the next thing but I needed it again. Then the meeting in a lot of places and so the question is, how am I going to do? You know, where am I going to put this logic and it's funny because what I'm saying it, it sounds so obvious make a class that's responsible for parsing out all of these different, you know.\r\n\r\nStrings from your the name of the component, but at the time you know how these things work. Like if you're literally copy and pasting maybe it's a little bit obvious but there's always like a minor variation in what one part of the code needs and the other one. So it's hard to recognize the duplication and it's even harder to recognize what the abstraction should be.\r\n\r\nBut basically I ended up making one file called Live Wire. I don't even know it's changed names a bunch of times now. It's like Livewire component parser or something like that and the idea is you pass in. We pass in like the app namespace of the laravel app and you pass in the views directory and you pass in the name of the component like counter and then you get all these methods like file classpath class name class namespace absolute path relative path few names United space all of these things contents tab contents all the stuff.\r\n\r\nAnd now it's all in one file so I can write a unit test now and this is funny because I don't write a lot of unit tests. But when I put it when I isolate all of this logic into one file, then I feel okay about writing unit tests where I can just throw at it like a hundred different permutations.\r\n\r\nLike I'm going to say test that counter spits out and then a bunch of assertions like specific assertions like this path this file path, whatever and then I do a bunch of those to test a bunch of permutations, and now that class is like really well unit tested and everything else can depend on.\r\n\r\nLively so that's kind of an obvious example, but maybe a harder example is something I talked about in episode 8 where we talked about my new design pattern called. What did I even call it? I don't know. I can't even see in the sidebar right now unless I expand and now I can see. It is called the plug-in pattern right where I kind of talked about making things extensible and I mentioned writing hooks.\r\n\r\nSo refactoring some front-end features two hooks instead of them just being code split out throughout the app. Okay, you probably aren't tracking what I'm saying. So let me break it down in Live Wire, there are let's say the we've been talking about polling. So let's stick with pulling wire: pole you register it.\r\n\r\nAnd then somewhere in the code base and live or is codebase. There's a file called node initializer that when a new when it's when it adds a new node it goes. Hey, is there any Livewire directives on this node? Okay, and it'll go through a switch statement and be like is there wire: pole? Okay.\r\n\r\nThere is now set up a set Interval Timer to fire an Ajax request every so often but the thing is I also need to add code and other places. I need to know that if it updates and it's removed to remove that that. So I have to go find the piece of the code where component element is updated and there's a bunch more of these things.\r\n\r\nSo like I said before I kind of had these I had these features sprawled out throughout different parts of the code base because that's honestly saying it right now it seems so obvious but that's that's like the thing to do. That's what you reach for that your knee-jerk reaction and it's hard to recognize how to do it.\r\n\r\nOtherwise. But I switched to this hooks pattern where I made this hooks manager where certain parts of the code Base fire Hooks and other parts register handlers for those hooks. So now I have one file in charge of polling and if I delete that file. Pulling disappears from the app. It's completely gone.\r\n\r\nSame thing with loading States anything like while you're loading inside Live Wire, that's all happening in one file. Now, it was sprawled out but cross like four five files. Now, it's one file called loading States dot JS because I can just register hook listeners inside of this one file. So anyway, I'm not I don't necessarily expect you to fully grasp exactly what I'm saying in terms of that implementation.\r\n\r\nBut basically just yesterday. I think I refactored the front-end code base to take I think four or five different features and I abstracted them all into one file each. So now I have separate files for each of these features. And if I delete the file the feature is gone and what I need to work on that feature, I know exactly where to look and that is the biggest benefit of this pattern.\r\n\r\nIs that whenever I need to work on this on this thing. I know exactly where I need to work on it. If there's a problem with this thing. I know exactly where the problem is finding to write a unit test. I know exactly what I'm unit. Testing it is so powerful. It's hard to overstate for me and it's so simple.\r\n\r\nKeep things in one file the but but the thing it's simple to say but to put it into practice you'll find that it is difficult because you need to put things in separate files or you think you do and sometimes you do need to but so just as some little code wisdom for you that I'm finding success with and I'm reminding myself of try to keep features inside one file.\r\n\r\nSee if you can do it and see if there's three factors that make it possible for you to isolate something into one file. It's really helped me while I have some time left. I'll read out some of the some of the files that I have in live where that I've done this with. I have like component cache manager.\r\n\r\nSo anything that has to do with a component talking to the caching layer all happens in one file. No other file actually calls on laravel as cash facade accept this file. So it's all right here component checksum. This is kind of this is a story for another time. But so I have all these manager classes and I don't love I know it like manager means nothing but for me, it's like.\r\n\r\nIt's kind of this. This is the file that handles all the things having to do with this thing and there's a bunch of other examples, but these are the ones that there's like component dehydrator or component hydrator that handles hydrating and dehydrating alive where component so if you need to do that, and if you're working on live wire you will need to do that.\r\n\r\nYou will need to look in those files and you know exactly where to look. I found this useful in there by laps myself. I can't think of an example offhand, but I know that I've communicated this on a couple of different calls in the past month with people that have been pairing with so hopefully you found this interesting or expiring or expiring inspiring or useful in some way.\r\n\r\nEnjoy twiddling your thumbs on a park bench. Thank you."
published_on: 'Oct 03'
sharing_url: 'https://simplecast.com/s/d871bd03'
iframe_markup: '<iframe frameborder=''0'' height=''200px'' scrolling=''no'' seamless src=''https://embed.simplecast.com/d871bd03?color=f5f5f5'' width=''100%''></iframe>'
---