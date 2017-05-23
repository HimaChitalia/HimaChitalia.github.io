---
layout: post
title:  Why rails method "redirect_to" ate up my 1-2 hours? 
date:   2017-05-23 18:43:06 +0000
---


The best part of the Web Development Curriculum at Flatiron school is solving its labs. Though it's really tough many times but it really gives you a hands-on experience, challenges to exercise your brain, and face-to-face with errors which might get when you program in the real world.

I am currently learning "Rails". It's huge but very interesting. Tough but loving it. While trying to solve a lab "[Sessions Controller Lab](https://github.com/learn-co-students/sessions_controller_lab-v-000)", I encountered an error, which didn't look complicated but surely took me a while to figure out a bug.

![Imgur](http://i.imgur.com/FtzzfgA.png)

It is saying that I am missing keyword end somewhere. My code for the destroy method, which was using redirect_to helper method, was very simple.

![Imgur](http://i.imgur.com/3yBjqyM.png)

I didn't understand what was going on. Tried rewriting that method again and again, scanned entire controller carefully to find out if I had a typo. Also, tried writing same method in a different syntax.

![Imgur](http://i.imgur.com/DMKuAZb.png)

But, aa... No luck! Finally, after drinking a cup of coffee, spending really good chunk time on it, I decided to take help of Learn experts. And realized that my mistake was something that I never thought of!

A space between redirect_to and opening bracket after that!!!! Removed it, and everything started working again.

![Imgur](http://i.imgur.com/1AgcifF.png)

No errors! It's strange but out of 100 times you get any errors, 99% it's a typo or unexpected things! Also, I did a little bit of research after this. And found out the more interesting syntax to write the same line.

```
redirect_to("ActionName", "ControllerName"
```

Which in the case of our destroy method, will look something like this.

![Imgur](http://i.imgur.com/oUMhp4T.png)

Lesson learned. Space does matter. 

Please feel free to let me know if you have any similar experience where space in method made a chaos in your application!



