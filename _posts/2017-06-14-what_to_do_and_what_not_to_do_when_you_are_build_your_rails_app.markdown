---
layout: post
title:  "Do’s and Don’ts When You are Building a Rails App!"
date:   2017-06-14 10:21:34 -0400
---


I have to admit that I was overwhelmed and very nervous when I started my Rails project. Everyone always advised me to keep my app pretty simple. Not to have too many nested routes and be very very clear about all of the models and Active Record Relationship they will have. Well, I wasn't!

I had a big dream about what final output will be. So, I just started to make it even before I was very clear about all of the aspects. Just like a normal learner, I decided to handle each thing as they come up. And it was my biggest mistake. Few things that really took a lot of my time were:

* Not being very clear about relationship all of the models will have with each other, threw me many errors that I was puzzled to solve. Not only once, but many times! My piece of advice for this: Write down all of the models, relationship and details about what actions you expect from controllers in detail before even making a vanilla Rails app. It will save a great deal of time for you and will also give you a great piece of mind when you are handling other critical errors.

* I made four level deeply nested route. Though now I can say that I am kind of like an expert after getting so much practice ;-D, but if you other options to work around it, don't go for so nested routes. Here is what I have:

```
resources :habits do
    resources :comments
    resources :goals do
      resources :milestones do
        resources :statuses
      end
    end
  end
```

Many times, I got errors because of this and those were really difficult to handle. By the time I realized this, I went too far with my app. So, didn't want to start all over again but yes, have some pity on yourself. Don't go for this much-nested routes.

* I though to deploy my app to Heroku after it will be all ready, Well, it wasn't a good idea. It gave me many migrations and models errors that Heroku didn't like. At that point, I had no option but give all my time and energy to solve it. The best advice I could give you is if you are planning to deploy your app to Heroku, do that the first thing after you create your vanilla app. The deeper you go harder it will be to solve errors Heroku will give you.

**So, the steps to follow when you are generating your first Rails app are:
**

1. Ask yourself so many questions about what do you expect from this app before doing anything else.
2. Draw a detailed diagram of all of the models and relationship between them You could use sites like [Giffy](https://www.gliffy.com/) or [Draw](https://www.draw.io/) or may be an old fashion technique of using pen and paper will also give you the best result.
3. Try to be very clear about what do expect from all of the controllers. I am not telling you to be strict about it. Obviously, you may need to change your plans any moment or you may want to add additional functionality later on. But being clear will fasten the process.
4. Make a vanilla Rails app with `rails new`  command. It is the great way to start as it creates so many important files and folders for you that you will need down the line.
5. If you want to deploy your app to Heroku, this is the best time to do it. Few interesting tutorials to help you out are   [Ruby on Rails from the ground up - 5/5 ](https://youtu.be/5kVtmnZNC8w?t=333) and [The Complete guide how to deploy Rails App in Heroku](https://www.youtube.com/watch?v=ku2guLYS1_U).
6. Now the last thing before starting your models and controllers is getting done with the authentication. You could use RubyGem like Devise or Bcrypt. I personally used Bcrypt because it is simpler to handle.
7. You are free to take care of the rest. But always remember to handle one thing at a time. It will give you some piece.

And last but not the least, Happy Coding!
