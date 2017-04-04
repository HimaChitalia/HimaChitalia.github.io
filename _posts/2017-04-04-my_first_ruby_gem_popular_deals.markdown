---
layout: post
title:  "My first Ruby Gem 'Popular_deals'"
date:   2017-04-04 16:19:29 -0400
---


![Imgur](http://i.imgur.com/Ro62c6S.png)

# My first Ruby Gem 'Popular_deals'

I have always heard people saying that learning and putting that knowledge in work is two completely different experience. And yes, I agree with it. Learning about Ruby language was a very delightful experience but actually making a ruby gem was completely different.

As a part of the web development course at Flatiron School, I had to make a CLI Data Gem. An app that provides a Command Line Interface (CLI) to an external data source. Making of first Ruby Gem sounds so much exciting! Well, it was for me too. But I was also nervous at the same time. I had confidence in myself but I knew that journey is not going to be as simple as making a cup of tea.

One thing I have learned is when you code, do one thing at a time. I started with doing lots of research and of course, watching all the informative videos provided by learn.co. I got an idea, what would I need to do to start this project and which was the website I would like to scrap. I started with making my gem using bundler. Oh great, it wasn't too tough. The tough part was making sure your environment is properly set up. Started with making my bin file working, I moved on to lib folder and declaring 2 classes: CLI and a NewDeals class. and I decided to test it. I like to use m new sandbox, my bin file to try this. And while trying, I realized that my classes are not talking to each other. I get the famous 'No Method' error. Something was wrong. After looking into all of the files, I realized that I need to require my newly made classes! It took me few minutes to find this out but yes, I solved my error. As I knew I am also going to use Nokogiri and Open-uri, I also added a dependency for the Nokogiri and required Nokogiri and Open-uri in my environment file. Ah! It I was little relaxed by now. I felt like at least, I have the right environment now to create my app.

After this, I started with my Newdeals class and wrote necessary methods to scrap www.slickdeals.net/deals. After lots of debugging, I could see my methods working with the help of my bin file. Now, I need to work on my CLI to make sure that I can present a list of deals to my users in their terminal in a really readable format. I had to go through many trial and error to finally get what I wanted. Now, I can see the list of deals with their index numbers in my terminal. Yayyyyyy!


My project was not done yet, I needed to make another layer of it. If anyone would like to see details of this deal like a description or from where to purchase, they should be able to see that. and for that, I need to scrap particular deal's URL. Ooooo!

I had no idea how I would do it because the list of deals get updated after every few minutes and each of the deal will have their unique URL. After a lot of thinking for the solution, I decided to use the take URL from the deals list page. So, I created the @product_url which will use each loop and will receive value from the list of the deal by looking for deal_url attribute that I created in a list_deals method. Ahhhh.. I had some logic in place now. I kept on improving the functionality by adding more methods. And After this, I started working on my CLI to make sure that it displays deal information properly. In between all of these, I stopped many times, did lots of research, try to find solutions for errors I was receiving and played with my bin file a lot to make sure that I was progressing. And finally, my CLI was working! It was the victory of my confidence over my fear and nervousness!

But I didn't like the output much, It was just plain white text. I looked online to see any possible solution and I came across a gem 'colorize'. Very colorful gem, just like its name. After adding 'colorize' into my gem pocket, My app output was pleasant to eyes.

Now I just need to push it to the Rubygems.org. I created an account, created a .gem file and pushed it. 

If you would like to look at all of the functions of my app and what does it do, please watch [this](https://www.youtube.com/watch?v=lY00NAUOims&list=PLBouSX1ip6YNJrbnopQebYQZxgPcIMH4K) video.

I have a working Ruby Gem that anyone can use to see today's popular deals. The journey was not easy but worth going for!



